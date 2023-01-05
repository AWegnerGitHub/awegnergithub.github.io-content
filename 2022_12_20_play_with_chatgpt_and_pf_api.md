Title: Using ChatGPT to interact with an API
Date: 2022-12-20 23:45
Tags: technical, chatgpt
Category: Technical Solutions
Slug: chatgpt-interact-with-external-api
Summary: I'm going to take ChatGPT for a spin against an API that I know. Let's see what it's capable of producing.
Status: published
Series: ChatGPT Commentary

[TOC]

## Introduction

I've played with [ChatGPT][1] a lot since it was released publicly a few weeks ago. It's gotten a fair bit of media coverage (and I helped generate
some of that with the [Stack Overflow ban of ChatGPT][2]). In my last post, I talked about how 
[I hope that ChatGPT kills the LeetCode interview][3] for Software Engineers. 

Today, I want to test it out against an API that I know well. In April 2022, I left PacketFabric where I was Vice President of 
Software Engineering. I played with the PacketFabric API _a lot_ during my tenure. I'm going to use that experience to see what ChatGPT 
can whip up in a few minutes. 

## The Goal

My goal here is to ask ChatGPT for a quick script to interact with a portion of the API, test that output and see how I can extend it. My 
thought is that this is how an engineer is going to go about their work. They have a task they need to accomplish (call this API end point),
they need to make sure it works (test it), and then work on refining it to do what's needed.

Another goal that I have is to write as little code as possible during this test. I'd love to copy and paste all of this.

## Getting started

For this test, I'll be using the [Get Locations List][4] end point. It's unauthenticated so I don't need to sign up for a demo account to get an 
API key. _Side Note: The documentation is wrong in that it states this end point requires a Bearer Token. This will be seen in the generated code and I don't fault ChatGPT for adding it into the code, because the documentation says it is required_

I started with a prompt:

> Use PacketFabric's API (https://docs.packetfabric.com/api/v2/redoc/#tag/Locations/operation/get_location_list) to get a list of locations in California (`CA`)

The code that I was provided:

    import requests

    api_key = "YOUR_API_KEY"
    headers = {
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json"
    }

    response = requests.get(
        "https://api.packetfabric.com/v2/location?state=CA",
        headers=headers
    )

    if response.status_code == 200:
        locations = response.json()
        print(locations)
    else:
        print(f"Request failed with status code {response.status_code}")

As I mentioned above, the Bearer token isn't required for this end point. It's inclusion does not impact the functionality of this 
script, and assuming I'm making more than calls to the `locations` end point, it'll be needed anyway. I'll keep it here. 

Now, there is a mistake in the code. For exactly the [same reason we banned ChatGPT on Stack Overflow][2], we see a subtle error here too. If
you run the snippet above, you will get:

> Request failed with status code 404

Why? Because ChatGPT is hitting the `location` not `locations` (plural). The PacketFabric API uses plurals. This is a minor problem, and easily fixed,
though it is explicitly stated in the documentation link which end points exist.

Moving on.

## Finding All US Locations

Next, I wanted to get all US locations.

> How do I use this to get all US locations?

The correct way to do this would be to use the `region` query parameter. ChatGPT had a different answer.

> To get a list of all locations in the United States using PacketFabric's API, you can simply omit the state query parameter from the GET request. This will return a list of all locations across all states in the US.

The same code provided simply strips the `state=CA` from the `requests.get()` call. *This answer is wrong*. It will, technically, return all 
public locations in the United States. But it will also return locations outside of the United States too. This is a much larger result set than I was 
looking for.

A developer should be able to catch this during testing and realize that the code is incorrect. However, ChatGPT has a tendancy to write responses in a 
very confident manner. This one is no exception.

## Finding a specific POP location

Moving to the next test, I wanted to find locations just in a single point of presence (POP).

> How do I get all locations in the `ATL1` pop?

The response returned seemed reasonable. There was a bit of prose with this explaination and a code snippet that adjusted the query parameters.

> To get a list of all locations in the ATL1 pop (point of presence) using PacketFabric's API, you will need to send a GET request to the /location endpoint with the `pop_code` query parameter set to ATL1.

    "https://api.packetfabric.com/v2/locations?pop_code=ATL1"

Again, this is incorrect. `pop_code` isn't a valid query parameter. It's not in the documentation and it doesn't return the expected result. 

I attempted to explain this to the bot:

> `pop_code` isn't a parameter defined in the documentation. Is this an undocumented parameter?

I received an unexpected response, with a new code snippet. 

> I apologize for the mistake in my previous response. `pop_code` is not a valid query parameter for the /location endpoint in PacketFabric's API.
> The documentation for the /location endpoint (https://docs.packetfabric.com/api/v2/redoc/#tag/Locations/operation/get_location_list) lists the valid query parameters as state, city, and zip_code. These parameters can be used to filter the locations returned by the API based on the state, city, and ZIP code of the location.
> For example, to get a list of all locations in Atlanta, Georgia (GA), you could use the following request:

    import requests

    api_key = "YOUR_API_KEY"
    headers = {
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json"
    }

    response = requests.get(
        "https://api.packetfabric.com/v2/location?state=GA&city=Atlanta",
        headers=headers
    )

    if response.status_code == 200:
        locations = response.json()
        print(locations)
    else:
        print(f"Request failed with status code {response.status_code}")


> This will send a GET request to the /location endpoint with the state query parameter set to GA and the city query parameter set to Atlanta, and will print the list of locations returned by the API.
> I apologize for any confusion my previous response may have caused. Please let me know if you have any further questions.

This is a better attempt - but still wrong. Going this route will return more POPs than expected. I'll receive everything in the city of Atlanta, instead
of my original request, which was to only get locations in the `ATL1` POP. This is another subtly wrong answer.

It's also interesting to note that ChatGPT says that only `state`, `city` and `ZIP code` are valid query parameters here. `state` and `city` are defined 
in the documentation. `ZIP Code` is not. That's not a valid query parameter for the locations end point. Also notice that it's still using the singular
`location`

## Results

My impression of ChatGPT has not changed much since it was released at the end of November. It is a novel tool - much like Github's copilot, but 
it's not going to take a developer's job. It can provide a good starting point for a problem, but if you are depending on it to write your code for you,
you will find it's not going to work long term. 

In this test, I learned that I don't know what it is doing about reading external documentation. I simply provided a link to the API documentation,
when asking how to get locations. It parsed out the correct URL schema, including version code. But, at the same time, it couldn't parse out the 
query parameters. It incorrectly added two parameters that don't exist (`pop_code` and `zip_code`). It also missed the plurality of the API end points.
It introduced a subtle bug - omitting the `region` to get US based locations, returns locations around the world.

Personally, I'd be upset if an engineer on my team committed code generated in this test. It's buggy, and has subtle inaccuracies. The code
provided by ChatGPT is a good _skeleton_. It is up to the developer to validate that it works as expected. Without the crucial step of developer testing,
it is guarenteed that bugs will be committed to the code base because "the AI said it'd work".

If you are going to use ChatGPT (or Copilot or a system similar to either), it is vital that you verify the code you are running. Does it do what you 
expect? Does it perform as you expect? The tools are there to help you build this, but that's just it. *Help*, not do. The testing I did here took
only a handful of minutes. For someone unfamiliar with the API they are interacting with, this quick turn around should mean they have a lot more time
to do robust testing.


 [1]: https://openai.com/blog/chatgpt/
 [2]: {filename}2022_12_05_stack_overflow_bans_chatgpt.md
 [3]: {filename}2022_12_15_get_rid_leetcode_interviews.md
 [4]: https://docs.packetfabric.com/api/v2/redoc/#tag/Locations/operation/get_location_list