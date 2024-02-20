---
title: "Design of a system for scraping a 3rd party API using serverless"
date: 2024-02-11T00:47:28+02:00
draft: true
limmat_temperature: 6.9
images: ["images/system_design_scrape_booking.jpg"]
tags: ["Tech"]
---
As mentioned in the [previous post](/posts/first-impressions-golang), I'm collecting hotel booking data regularly, scraping the Booking.com API. I'm making over 1M requests per month to this API and inserting over 100k rows per day to a database. The entire system runs on [Google Cloud's free tier](https://cloud.google.com/free/docs/gcp-free-tier). 

This was a great opportunity to learn a new paradigm: [serverless computing](https://en.wikipedia.org/wiki/Serverless_computing). "Serverless" is a misnomer as there's still a server. The main difference to the 'traditional' paradigm is that instead of renting a server and having it dedicated for your usage, on serverless, a server is only allocated on demand and is automatically ramped down when no longer necessary. I really enjoyed learning something new, it [put me in the flow](/posts/imagine-blogging-in-2022/#so-why-even-start-blogging-now) and I was genuinely looking forward to working on this. I specially enjoyed the simplicity of deploying the code to the cloud, with many additional benefits, such as built-in monitoring and logging.

So how does this system work and what are the advantages of serverless?

## Goal
I want to collect data to train a model to predict hotel room prices. I'd like to use the following signals in the model:
*  City-level occupancy rate
*  Hotel-level occupancy rate   
*  Day of the week of the desired booking date
*  Distance in days to the desired booking date
*  Price difference to competitors

To be able to get city-level occupancy rates I will need to scrape **all** hotels in a city. I imagine hotel booking patterns depend on the city (is it a business-oriented city, i.e., most bookings are during the week or is it a touristic city where bookings are mostly done for the weekend), so I will need to scrape a few different ones.

## Overview
The system is scraping a touristic city (Ouro Preto, 48 hotels), a business city (Belo Horizonte, 100 hotels) and a mixed city (Rio de Janeiro, 271 hotels). The scraping is done daily and gets pricing for the next 90 days, for a total of ~38k requests per day (`(48+100+271 hotels)*(90 days)`). Each request gets data for all rooms in a hotel and I collect all of it. The data about every room in every hotel is inserted, resulting in more than 100k rows per day.

{{< figure src="/images/system_design_scrape_booking.jpg" width="70%">}}

The scraping is kicked off daily by the [Cloud Scheduler](https://cloud.google.com/scheduler), which is basically a [cronjob](https://en.wikipedia.org/wiki/Cron) on the cloud. It calls a [Cloud Function](https://cloud.google.com/functions) that enqueues all the requests for the next 90 days for a particular city using [Cloud Tasks](https://cloud.google.com/tasks). Each task is a thin wrapper around another Cloud Function that makes a request to the Booking API and writes the results to [BigQuery](https://cloud.google.com/bigquery). Both Cloud Functions are implemented using [Golang](/posts/first-impressions-golang). All of these 'fancy' cloud names are part of the serverless paradigm and in the next section we will go over each one of these components to learn what they do.

## Component deep-dive
### Cloud Scheduler
[Cloud Scheduler](https://cloud.google.com/scheduler) is the simplest component of the system. As mentioned this is basically a managed scheduler on the cloud, a glorified cronjob. It allows you to execute any task you want at regular intervals. The biggest difference to a regular cronjob is that Cloud Scheduler allows you to trigger another Google Cloud service, such as a Cloud Function. Additionally you can easily monitor its status and configure automatic retries. I appreciate how easy it was to setup.

The [free tier](https://cloud.google.com/scheduler/pricing) allows for 3 jobs.

### Cloud Function
Arguably the most important component of the system, [Cloud Functions](https://cloud.google.com/functions?hl=en) allow you to X.

The [free tier](https://cloud.google.com/functions/pricing) allows for 2 million invocations per month, 400k GB-seconds of memory, 200k GHz-seconds of compute time and 5GB of internet data transfer out per month.

#### Enqueue city
How cloud function / cloud run works, free tier explanation

#### Cloud Function: Scrape Booking.com
How it works, show graphs from cloud function / cloud run

### Cloud Tasks: Scrape Queue
[Cloud Tasks](https://cloud.google.com/tasks)
How it works, show graphs from cloud tasks

The [free tier](https://cloud.google.com/tasks/pricing) allows for 1 million operations per month .

### BigQuery: Storing room prices
[BigQuery](https://cloud.google.com/bigquery/?hl=en)
How it works, table definition, free tier description

The [free tier](https://cloud.google.com/bigquery/pricing) allows for 1 TiB of query data processed per month and 10 GiB of storage. 

## When to use serverless
Ease deployment, check wikipedia