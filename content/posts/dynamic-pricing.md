---
title: "Dynamic pricing"
date: 2023-03-25T00:45:28+02:00
draft: true
limmat_temperature: 8.8
tags: ["Tech"]
---
start with amadeus because less bureaucractic. GDS however doesn't have small properties (e.g. airbnb, vacation rentals, urban homes). "While GDSs generally have large hotel inventories, they may miss some segments that are of little interest to corporate travelers."
move to OTA (expedia, booking) when possible, but they all require real companies and proof that you are generating revenue for them. "Yet, you need to have a significant volume of bookings to be vetted by large players since they are typically not interested in integrations with startups and small businesses."
alternative may be connectivity partners - should have widest hotel offer
bedbanks if you want to sell rooms, in our case i want to get competition rates, so no need

understand amadeus coverage in brazil
understand what kind of rates to support (non-refundable) and if want to support length-based rates
understand how to store the data and how to efficiently look it up
estimate costs with amadeus
decide what hotels to scrape and to offer service to (need to retrieve competitor rates as well)
understand how long into future availability needs to be scraped
understand what data would need to train model
understand which PMSs most hotels in brazil use (check which ones roompricegenie support; how does rpg work?)

system needs to integrate with property management systems or channel managers (I think most hotels use PMSs that already integrate with channel managers; channel managers are the ones actually distributing your rates across different OTAs) - need to understand which ones are the most popular (check roompricegenie?). A channel manager is software that most hotels would use. It helps them distribute inventory and rates across multiple channels simultaneously via a single interface. "Another significant group of channel managers partners is tech providers that integrate with channel managers or property management systems offering revenue management modules, room service software, etc."

is there a simple way to test that customers would be interested in this? What if it's just demo without any data?

Check if milhas123 has hotel reservations and API

Idea: Dynamic pricing tailored for a city / area.

How to get data:
Scrape booking.com information? All hotels seem to be getting occupancy rates from somewhere, start logging it.

How to get customers
E-mail new hotels on booking.com? Emails hotels that currently do not use dynamic pricing?

## MVP
Just a landing page
Let user pick city in switzerland; enter a city to get started
* chart on occupancy rates over time
* next events in the city
* average price per night by number of stars

Tabs to simulate dynamic pricing
* Based on days of the week
* Based on next events
* Based on simulated occupancy rates (discount when empty; surcharge when full)

Chart on how much more rent you could extract

## Pricing
Haven't given much thought, but should be a win-win kind of pricing. Users only pay when dynamic pricing is able to extract more money. Your interests are then aligned.
