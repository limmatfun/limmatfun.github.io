---
title: "Analysis of 3 months of pricing data from Booking.com"
date: 2024-04-27T00:47:28+02:00
draft: true
limmat_temperature: 9.3
images: ["images/system_design_scrape_booking.jpg"]
tags: ["Tech", "QuartoVerde"]
---
Mention previous 2 posts
Why we are doing this
can we accomplish that?


As mentioned in the [previous post](/posts/first-impressions-golang), I'm collecting hotel booking data regularly, scraping the Booking.com API. I'm making over 1M requests per month to this API and inserting over 100k rows per day to a database. The entire system runs on [Google Cloud's free tier](https://cloud.google.com/free/docs/gcp-free-tier). During the 3 months this system scraped about 10M hotel room prices, from 3 different cities (Rio de Janeiro, Belo Horizonte and Ouro Preto) distributed from January 22nd to April 18th.

{{< figure src="/images/analysis_datapoints.png" width="80%">}}

The system was pretty stable, with the exception of February 28th where no data was collected and March 18th, where the system scraped all hotels twice. Unfortunately I didn't monitor this and logs are gone by now, so there is no way to know what happened on February 28th (I also see now that I did not see set it up to retry in case it failed, so there's a potential configuration improvement to be made). Regarding March 18th, I just assume it ran twice. [Cloud Scheduler's documentation](https://cloud.google.com/scheduler/docs/overview) does warn this can happen:

> In some rare circumstances, it is possible for a job to run multiple times in association with a single instance of the schedule, so your code must ensure that there are no harmful side-effects of repeated execution.

I did not prepare for this, so this is yet another chance for making the system more robust.

{{< figure src="/images/analysis_datapoints_no_outliers.png" width="80%" caption="The data removing the outliers">}}