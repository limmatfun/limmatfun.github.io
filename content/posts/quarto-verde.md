---
title: "QuartoVerde is online!"
date: 2023-07-15T00:46:28+02:00
draft: true
limmat_temperature: 24.8
images: ["images/dynamic_hotel_pricing.png"]
tags: ["Tech", "QuartoVerde"]
---

### Future steps
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

Check if milhas123 has hotel reservations and API

Idea: Dynamic pricing tailored for a city / area.

try to create a fake hotel using current software
what kind of problems do you have?
how well are they solved?
check how booking.com dynamic pricing works

Try to understand size of market
scrape booking.com in brazil? check which hotels always have the same price, try different regions. which hotels are new and maybe need help with pricing?
potential pivoting: upselling

try to find hotels that would talk to you
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
