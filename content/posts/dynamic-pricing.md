---
title: "Dynamic hotel pricing: planning the MVP"
date: 2023-05-02T00:45:28+02:00
draft: true
limmat_temperature: 11.9
images: ["images/dynamic_hotel_pricing.png"]
tags: ["Tech"]
---
I've done some research into the [dynamic hotel pricing idea](/posts/inspiration-from-startup-school#idea-dynamic-hotel-pricing) I mentioned at the end of the last post and the idea is now slowly materializing into something concrete. I now understand the space better and what it would involve to implement such an idea. This post will give a summary of a concrete plan: how the minimum viable product (MVP) could look like and how to identify potential first customers. Hopefully with somewhat minimal work. 

## What is dynamic pricing?
The basic idea behind dynamic pricing is to maximize revenue based on demand. If demand is high then prices rise and conversely, if demand is low, prices drop to attract more customers. Concretely for a hotel this means adjusting prices based on a multitude of factors: 
* Competitors rates (charge more than competitors and your attractivity drops)
* Seasonality including Holidays (e.g. for a business property there's more reservations during weekdays while for a vacation property it tends to concentrate on weekends, holidays; there may be a summer/winter high/low season)
* Local events (festivals, work conferences attract more guests than usual)
* Occupancy rates (both from your own property and from your competitors) and even weather.

## Planning the MVP
These are the following steps that need to be solved to have a basic dynamic pricing solution:
1.  Integrate it into the hotel's system, i.e., how it would set the room rates for the hotels.
2.  Get the necessary data for predicting prices. As mentioned above we need competitor rates, local events, current occupancy rate and understand seasonality effects for each hotel.
3.  Train a model to predict the ideal price that hotels can charge for their rooms.

## Step 1: Integrating the software into the hotel world
So how would a software that predicts hotel room rates be used by hotels? Before we start we need to understand how the hotel distribution industry works.

### A crash course on the hotel distribution industry 
I learned a lot about how property owners manage their availability, rates and hotel room distribution from this excellent post by [altexsoft on Hotel Booking APIs](https://www.altexsoft.com/blog/hotel-api/). In summary, these are the main players:
* Online Travel Agencies (OTAs): They are the dominant force in this space and where the bulk of reservations are made. You have probably heard of the two largest in this space: Booking Holdings (Booking.com, Kayak, Agoda, etc.) and Expedia.
* Global distribution systems (GDSs): They originate in the airline industry where they are the dominant player and have access to the majority of fares. GDSs aggregate data from multiple sources and distribute them across OTAs and travel agents. The largest players are Amadeus, Sabre and Travelport. Their hotel coverage is not as a high as OTAs.
* Channel managers: This is a tool that helps hotel to distribute their inventory across multiple channels (Booking, Expedia, AirBnB, GDSs, etc.). A channel manager ensures no overbooking can happen and that your rates are consistent across all the different platforms. This happens in both directions. Once a reservation is made at any of those platforms, the channel manager is notified and then notifies all other platforms.

Hotels may work with an OTA or a channel manager directly, but more often than not they use a property management system (PMS) instead. A PMS is a software that handles front- and back-office operations, reservations, occupancy management, housekeeping and other administrative tasks for a hotel. It usually also integrates with a channel manager so that the hotels inventory and its rates are distributed properly.

{{< figure src="/images/hotel_distribution_industry.jpg" width="80%" caption="Hotels use a PMS or a Channel Manager to have their rates distributed to multiple GDSs, OTAs and bedbanks (the relationship is bi-directional: when a booking is made the PDS or Channel Manager is also updated)." >}}

### Integrating a dynamic pricing solution
So how does a dynamic pricing solution fit in the big picture? Since hotels typically use a PMS or Channel Manager to distribute their rates, this piece of software needs to be able to communicate directly with them. 

{{< figure src="/images/dynamic_hotel_pricing.jpg" width="80%" caption="The dynamic pricing software gets its data from a GDS and talks directly with the PDS of the hotel to update rates." >}}

## Step 2: Getting the data
I mentioned how a hotel room's rate should depend on 4 factors: competitor rates, seasonality, local events and current occupancy rates. To simplify things for the MVP I'm reducing this just to competitor rates. They should be a good proxy anyways for the other 3 factors. 

The best place to get the data would be an OTA like Booking.com or Expedia. The vast majority of hotels are on them and both offer APIs to programatically access rates and availability at scale. However, Booking is currently not accepting new partners and Expedia requires partners to be able to generate $1.5M in bookings for them. Indeed OTAs typically offer their APIs for free so they expect revenue to be generated for them in exchange.

The only option I've found without much bureaucracy is using a GDS. Their coverage tends to mostly focus on corporate travelers, so they may have many hotels where tourists stay in. Amadeus offers a ["self-service API"](https://developers.amadeus.com/self-service) where you pay per API call. Their [hotel API](https://developers.amadeus.com/self-service/category/hotels) allows you to query hotels in a city/location and get their rates. It's still an open question for me to understand how large their coverage is in Brazil. 

## Step 3: Training the model
This is definitely the step where things are most unclear to me. I would need historical data containing occupancy rates, the price charged per booking and the data of the booking. This data is actually not easy to get directly.

## Initial plan
I'd like to focus on the advice given on the previous post and before I start spending too much time on building the MVP, I want to get user feedback. My plan is to get a list of potential first customers by leveraging data from step 2. It will allow me to understand which hotels currently don't have dynamic pricing and I plan on reaching out to them to gauge their interest in a potential software for this. I also want to understand if they believe this is a problem at all, how they are currently tacking it and also what PMS / Channel Manager they use to distribute their rates.


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

## Pricing
Haven't given much thought, but should be a win-win kind of pricing. Users only pay when dynamic pricing is able to extract more money. Your interests are then aligned.
