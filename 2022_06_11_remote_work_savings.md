Title: Time and Gas saved by switching to remote work
Date: 2022-06-12 01:00
Tags: job, meta
Category: Jobs
Slug: time-gas-saved-with-remote-work
Summary: Nearing 5 years of full time remote, a look back on what this has saved me in terms of time commuting and gas
Status: published

[TOC]

## A brief history

In August of 2017, I left a decade long career that consisted of driving at least 90 minutes every day. During construction, poor weather or winter weather it was closer to two hours daily. I discussed this as [one of the factors][1] in my initial search for a new role at the time. I also briefly covered it in [my reflection][2] on how I found that role. An hour and a half to two hours daily adds up quickly.

I'm quickly approaching half a decade of full time remote work. I spent a majority of that at [PacketFabric][3] and recently started at [Woven][4] (still need to post about the new role). I wanted to see what I've saved in terms of time commuting, gallons of gasoline and maybe a few other interesting stats.

## Starting Numbers

Before I can do some of these calculations, I needed to get some starting numbers. I'm not trying to calculate to the penny or to the minute, so these are rough averages. However, over the course of 5 years, a couple extra minutes every day add up quickly. These numbers assume I would have stayed at that same office, same house, and accumulated vacation time the same way I was while still employed there.

    Miles per day: 90 (45 miles one way)
    Commute time per day (minutes): 106 minutes (53 minutes one way, this is roughly in the middle of my 1.5 to 2 hour commute depending on traffic)
    First day remote: 2017-08-01
    Calculate until: 2022-06-10
    Total number of days: 1774
    Total number of week days (remove the weekends): 1269
    Total vacation days: 180
    Total number of work days: 1089
    Average MPG: 22.5

I also needed to find an average gas price. While starting to write this post I thought I'd pull official numbers from the [US Energy Information Administration][5]. They have numbers going back to 1993 broken down by week. This would be amazing, but I was spending to much time trying to figure out when I was on vacation and how to handle weeks with a holiday in it. I don't need exact numbers. I'm looking for ball park, so instead I went with the [yearly gas price average for 2017 through 2021][6] then I used the [inflation calculator from the Bureau of Labor Statistics][7] to adjust for inflation using May of that year to May of 2022.

    2017: $2.52 -> $3.01
    2018: $2.81 -> $3.26
    2019: $2.69 -> $3.07
    2020: $2.25 -> $2.57
    2021: $3.10 -> $3.37
    2022: $4.00   (based on average price of January through May 2022)

This gets an average gas price of `$3.21`.

It's probably important to note that if I run this again in August at the full 5 year mark that gas cost is going to be much higher. [Inflation was 8.6% in May 2022][8], with [average gas prices hitting $5.00 nationwide][9]. In my area, it's been above $5 for over a month. This is also part of the reason I wanted to run these numbers.

Lastly, I wanted to see if I could figure out how much CO2 I had not produced based on how much gasoline I hadn't used. I was expecting a complicated formula. Turns out that [1 Gallon of Gas = 19.59 pounds of CO2][10] (8,887 grams of CO2 per gallon).

## The calculations

With my initial numbers determined, the calculations are pretty easy.

    Total Miles Not Driven = Miles per day * Total number of work days
                           = 90 * 1089
                           = 98,010 miles

    Total Time Not Commuting = Commute time per day (minutes) * Total number of days
                             = 106 * 1089
                             = 115,434 minutes
                             = 1923.9 hours
                             = 80.1 days

    Gallons of Gas Not used = Total Miles Not Driven / Average MPG
                            = 98,010 / 22.5
                            = 4,356 gallons of gasoline

    Dollars not spent on gas = Gallons of Gas Not used * Average gas price
                             = 4,356 * 3.21
                             = $13,997.28

    CO2 Not produced = Gallons of Gas Not Used * 19.59
                     = 4,356 * 19.59
                     = 85,334.04 pounds of CO2


## Analysis

Those are some impressive numbers. I saved almost 100,000 miles of commute time just going to and from work over five years. Put another way - I was NOT in my car for 80 full days over the course of five years. Literally a full time job's worth of travel time at nearly 2,000 hours saved over 5 years.

I didn't put over 4,300 gallons of gas in the vehicle and I didn't spend almost $14,000 on gasoline. That also prevented the production of 85,000 pounds of CO2. Sounds like a big number, but it's a passenger vehicle. It doesn't compare to the output of a large truck, an airplane, a tanker, or a factory. But, it's something.

## Conclusion

These are just the easily calculable numbers on what shifting to remote work has saved me. Things that I can't put a number on: time with family, flexible scheduling, ability to take the dogs for a walk when the sun is out, and more. I'm not spending hours in a car. I'm at home when the family gets home from school and work.

I hadn't tried to put hard numbers on this before, but now that I have, I am even more satisfied with my decision five years ago to start looking for a company that was 100% remote. 


 [1]: {filename}2017_07_31_a_decade_at_caterpillar.md#the-long-drive
 [2]: {filename}2017_11_28_how_i_found_an_awesome_remote_job.md#full-time-work-from-home
 [3]: https://packetfabric.com
 [4]: https://www.woventeams.com/
 [5]: https://www.eia.gov/dnav/pet/hist/LeafHandler.ashx?n=pet&s=emm_epm0_pte_nus_dpg&f=w
 [6]: https://www.eia.gov/dnav/pet/hist/LeafHandler.ashx?n=pet&s=emm_epm0_pte_nus_dpg&f=a
 [7]: https://www.bls.gov/data/inflation_calculator.htm
 [8]: https://www.cnbc.com/2022/06/10/consumer-price-index-may-2022.html
 [9]: https://www.cnn.com/2022/06/11/business/gas-prices-five-dollars-national-june/index.html
 [10]: https://www.epa.gov/greenvehicles/greenhouse-gas-emissions-typical-passenger-vehicle#burning
