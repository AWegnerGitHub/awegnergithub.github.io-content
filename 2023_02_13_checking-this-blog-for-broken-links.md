Title: Automatically checking for broken links using Github Actions
Date: 2023-02-13 14:30
Tags: technical, Pelican, meta
Category: Side Activities
Slug: find-broken-links-with-github-actions
Summary: This blog is over a decade old with over 100 posts. This post covers my recent work to find links that have broken so that I can fix them quickly.
Status: draft


[TOC]

## Introduction

Over the past six months or so, I've been making changes to the site. This culminated in November with a theme update
and [site relaunch][1]. I have a couple more articles planned about things I've learned from that relaunch, started with today's
article. The site is [over a decade old][2], with the initial article in 2009, with over 100 posts since then.

A decade plus is a long time to assume that links will remain operational. I wrote a short series of articles about how 
[broken links impacted Stack Overflow][3] over 7 years ago, including a [proposal to fix it][4] and how I performed the 
[link checking from a technical perspective][5]. Now, I want to make sure that I don't have dead links all over _my_ site,
especially because this is the site I give out in a professional context.

## GitHub Action

### Scheduling

My goal for this project was to check links for the entire site on a regular basis. I was hoping roughly once a week. Fortunately,
GitHub Actions allow you to [schedule your actions][6], using syntax like this:

    on:
        schedule:
            - cron:  '12 1 * * 5'

The important thing to note, which is called out in the docs, is that `*` is a special character in YAML, so the string 
has to be quoted. Using this schedule, my link checks will run every Friday at 1:12am. For me, timezone doesn't matter, but the
documentation says this is going to be UTC time.

### Checking links

The next thing that has to happen is the actual checking of the links. I use [broken-link-checker][7] for now. I picked this because 
I'm familiar with it. It hasn't been updated since 2018, though, so in the future I may spend some time either looking for an 
alternative or building an alternative. For the time being though, it works just fine.

I put this snippet into my actions file.

    steps:
        - name: Run Broken Links Checker
        run: npx broken-link-checker $WEBSITE_URL --ordered --recursive \
        --user-agent "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/109.0" \
        --exclude linkedin \
        --exclude "udemy" \
        --exclude "ude.my" \
        --exclude "eia.gov" \
        --exclude "backpack.tf" \
        --exclude hlsw \
        --exclude dell \
        --exclude supermicro \
        --exclude mysql

The quick break down:

* `$WEBSITE_URL` is an `env` variable that I define as `https://andrewwegner.com/`
* I set up a `user-agent` because several sites block the default one. I did something similar in my [Stack Overflow analysis 7 years ago][5].
* All of those `excludes` are due to those specific domains still not allowing automatic scraping. It's a little disappointing that I need these, but excluding these few to ensure the rest are functional is more important to me than trying to figure out a work around.

### Create an issue

If broken links are found, it doesn't do me any good to have that buried in a log somewhere without alerts. Perhaps unsurprisingly,
I don't check Github every day. My goal was to create an issue on the repository if broken links were found. I did that using the following, and the [Create an issue action][8].

    - uses: actions/checkout@v3
      if: failure()

    - uses: JasonEtco/create-an-issue@v2
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        filename: ${{ env.ISSUE_TEMPLATE }}
      if: failure()

The important line here is the `filename:` line. Using another environment variable, I defined a file with my issue template. 
Everytime an issue is created by the action, it will have the same format.

### Issue Template

My [template][9] is defined in the same directory as my workflow and currently looks like this:

    ---
    title: Website Contains Broken Links
    labels: ''
    assignees: ''
    ---

    ## Broken Links Detected

    Broken Link Checker found broken links on https://andrewwegner.com

    [View Results](https://github.com/AWegnerGitHub/awegnergithub.github.io-content/actions/workflows/check-broken-links.yml)

    _Use search filter `─BROKEN─` to highlight failures_

The top meta data defines the issue title, and any labels or assignees I want to set up. I'll probably add myself as an assignee
once I'm happy with the stability and reliablity of the checks.

### Environment Variables

What do these mean?

    env:
        WEBSITE_URL: "https://andrewwegner.com/"
        ISSUE_TEMPLATE: ".github/workflows/check-broken-links.md"

## Gotchas

workflow_dispatch
link-checker: Occassional failure of link causes problems. Uses weird DASH in output.
Template: Would like to show broken links in the issue
full script
Nice to haves

 [1]: {filename}2022_11_17_relaunch_personal_site.md
 [2]: https://andrewwegner.com/archives.html
 [3]: {filename}2015_08_06_analysis-of-links-posted-to-stack-overflow.md
 [4]: {filename}2015_08_07_a-proposal-to-fix-broken-links-on-stack-overflow.md
 [5]: {filename}2015_08_10_link-analysis---technical-explanation.md
 [6]: https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule
 [7]: https://www.npmjs.com/package/broken-link-checker
 [8]: https://github.com/marketplace/actions/create-an-issue
 [9]: https://raw.githubusercontent.com/AWegnerGitHub/awegnergithub.github.io-content/master/.github/workflows/check-broken-links.md