---
title: "Design of a system for scraping a 3rd party API using serverless"
date: 2024-02-11T00:47:28+02:00
draft: true
limmat_temperature: 6.9
images: ["images/system_design_scrape_booking.jpg"]
tags: ["Tech"]
---
As mentioned in the [previous post](/posts/first-impressions-golang), I'm collecting hotel booking data regularly, scraping the Booking.com API. I'm making over 1M requests per month to this API and inserting over 100k rows per day to the database using [Google Cloud's free tier](https://cloud.google.com/free/docs/gcp-free-tier). I realized that for this task [serverless computing](https://en.wikipedia.org/wiki/Serverless_computing) was a great fit and I was excited to learn something new. So how does this system work and what are the advantages of serverless?

## Goal
I want to collect data to train a model to predict hotel room prices. I'd like to use the following signals in the model:
*  City-level occupancy rate
*  Hotel-level occupancy rate   
*  Day of the week of the desired booking date
*  Distance in days to the desired booking date
*  Price difference to competitors

To be able to get city-level occupancy rates I will need to scrape **all** hotels in a city. I imagine hotel booking patterns depend on the city (is it a business-oriented city, i.e., most bookings are during the week or is it a touristic city where bookings are mostly done for the weekend), so I will need to scrape a few different ones.

## Overview
The system is scraping a touristic city (Ouro Preto, 48 hotels), a business city (Belo Horizonte, 100 hotels) and a mixed city (Rio de Janeiro, 271 hotels). The scraping is done daily and gets pricing for the next 90 days, for a total of ~38k requests per day (`(48+100+271 hotels)*(90 days)`). Each request gets data for multiple rooms in each hotel, inserting more than 100k rows per day to the database.

{{< figure src="/images/system_design_scrape_booking.jpg" width="70%">}}

The scraping is kicked off daily by the [Cloud Scheduler](https://cloud.google.com/scheduler), which is basically a [cronjob](https://en.wikipedia.org/wiki/Cron) on the cloud. It calls a [Cloud Function](https://cloud.google.com/functions) that enqueues all the requests for the next 90 days for a particular city using [Cloud Tasks](https://cloud.google.com/tasks). Each task is a thin wrapper around another Cloud Function that makes a request to the Booking API and writes the results to [BigQuery](https://cloud.google.com/bigquery). The entire system is implemented using [Golang](/posts/first-impressions-golang).

## Component deep-dive
### Cloud Scheduler
Bla, 3 cities, what time, how cloud scheduler works, free tier explanation

### Cloud Function: Enqueue city
How cloud function / cloud run works, free tier explanation

### Cloud Tasks: Scrape Queue
How it works, show graphs from cloud tasks

### Cloud Function: Scrape Booking.com
How it works, show graphs from cloud function / cloud run

### BigQuery: Storing room prices
How it works, table definition, free tier description

## When to use serverless
Ease deployment, check wikipedia