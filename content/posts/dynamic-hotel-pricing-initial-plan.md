---
title: "Dynamic hotel pricing: an initial plan"
date: 2023-07-15T00:45:28+02:00
draft: false
limmat_temperature: 24.8
images: ["images/dynamic_hotel_pricing.jpg"]
tags: ["Tech"]
---
I've done some research into the [dynamic hotel pricing idea](/posts/inspiration-from-startup-school#idea-dynamic-hotel-pricing) I mentioned and it's now slowly materializing. I now understand the space better and what it would involve to implement such an idea. This post will give a summary of a concrete plan on how the minimum viable product (MVP) could look like and how to identify potential first customers. Hopefully with somewhat minimal work. 

## What is dynamic pricing?
The basic idea behind dynamic pricing is to maximize revenue based on demand. If demand is high then prices rise and conversely, if demand is low, prices drop to attract more customers. Concretely for a hotel this means adjusting prices based on a multitude of factors: 
* Competitors rates (prices should be competitive; otherwise if you charge more than competitors your attractivity drops)
* Seasonality including Holidays (e.g. for a business hotel there's more reservations during weekdays while for a vacation property it tends to concentrate on weekends and holidays; there may be a summer/winter high/low season)
* Local events (festivals, work conferences attract more guests than usual)
* Occupancy rates (both from your own property and from your competitors)
* Weather

## Planning the MVP
These are the steps that need to be solved to have a basic dynamic pricing solution:
1.  Integrate it into the hotel's system, i.e., it needs to be able to set the prices for hotels directly.
2.  Get the necessary data for predicting prices. As mentioned above we need competitor rates, local events, current occupancy rate and understand seasonality effects for each hotel.
3.  Train a model to predict the ideal price that hotels can charge for their rooms.

## Step 1: Integrating the software into the hotel world
So how would a software that predicts hotel room rates be used by hotels? Before we start we need to understand how the hotel distribution industry works.

### A crash course on the hotel distribution industry 
I learned a lot about how property owners manage their availability, rates and hotel room distribution from this excellent post by [altexsoft on Hotel Booking APIs](https://www.altexsoft.com/blog/hotel-api/). In summary, these are the main players:
* **Online Travel Agencies (OTAs)**: They are the dominant force in this space and where the bulk of reservations are made. You have probably heard of the two largest in this space: Booking Holdings (Booking.com, Kayak, Agoda, etc.) and Expedia.
* **Global distribution systems (GDSs)**: They origin is in the airline industry where they are the dominant player and have access to the majority of fares. GDSs aggregate data from multiple sources and distribute them across OTAs and travel agents. The largest entities are Amadeus, Sabre and Travelport. Their hotel coverage is not as a high as OTAs.
* **Channel managers**: This is a tool that helps hotels to distribute their inventory across multiple channels (Booking, Expedia, AirBnB, GDSs, etc.). A channel manager ensures no overbooking can happen and that your rates are consistent across all the different platforms. This happens in both directions. Once a reservation is made at any of those platforms, the channel manager is notified and then notifies all other platforms.

Hotels may work with an OTA or a channel manager directly, but more often than not they use a **Property Management System (PMS)** instead. A PMS is a software that handles front- and back-office operations, reservations, occupancy management, housekeeping and other administrative tasks for a hotel. It usually also integrates with a channel manager so that the hotels inventory and its rates are distributed properly.

{{< figure src="/images/hotel_distribution_industry.jpg" width="80%" caption="Hotels use a PMS or a Channel Manager to have their rates distributed to multiple GDSs, OTAs and bedbanks (the relationship is bi-directional: when a booking is made the PDS or Channel Manager is also updated)." >}}

Note that the dynamic pricing software also needs to talk to either a GDS or OTA. While the Channel Manager or PMS is doing this as well, they are mostly concerned about updating their own hotel rates and receiving bookings. A dynamic pricing software needs to get rates at scale, including competitors, to be able to price appropriately.

### Integrating a dynamic pricing solution
So how does a dynamic pricing solution fit in the big picture? Since hotels typically use a PMS or Channel Manager to distribute their rates, this piece of software needs to be able to communicate directly with them. 

{{< figure src="/images/dynamic_hotel_pricing.jpg" width="80%" caption="The dynamic pricing software gets its data from a GDS (or OTA) and talks directly with the PDS of the hotel to update rates." >}}

## Step 2: Getting the data
I mentioned how a hotel room's rate should depend on 4 factors: competitor rates, seasonality, local events, current occupancy rates and weather. To simplify things for the MVP I'm reducing this just to competitor rates. They should be a good proxy anyways for the other 4 factors. 

The best place to get the data would be an OTA like Booking.com or Expedia. The vast majority of hotels are on them and both offer APIs to programatically access rates and availability at scale. However, Booking is currently not accepting new partners and Expedia requires partners to be able to generate $1.5M in bookings for them. Indeed OTAs typically offer their APIs for free so they expect revenue to be generated for them in exchange.

The only option I've found without much bureaucracy is using a GDS. Their coverage tends to mostly focus on corporate travelers, so they may not have many hotels where tourists stay in but rather mostly business hotels. The downside is that business hotels tend to be larger and to already employ dynamic pricing techniques. 

Amadeus offers a ["self-service API"](https://developers.amadeus.com/self-service) where you pay per API call. Their [hotel API](https://developers.amadeus.com/self-service/category/hotels) allows you to query hotels in a city/location and get their rates. Functionality is relatively limited on their self-service API, requiring one API call for each desired pair of dates and hotels. It's also still an open question for me to understand how large their coverage is in Brazil. 

## Step 3: Training the model
This is the step where things are most unclear to me. I would need historical data containing occupancy rates, the price charged per booking and the dates of the booking. I doubt OTAs or GDSs would actually make this data available, as they are mostly interested in selling current availability, not in providing historical data.

There's actually quite a few papers in this area [[1]](https://www.rairo-ro.org/articles/ro/abs/2018/01/ro170280/ro170280.html), [[2]](https://www.sciencedirect.com/science/article/abs/pii/S0278431911000958), [[3]](https://link.springer.com/article/10.1057/rpm.2012.44) and it's easy to find more on [Google Scholar](https://scholar.google.ch/scholar?hl=de&as_sdt=0%2C5&as_vis=1&q=dynamic+pricing+hotel&btnG=), so I'm not too worried about this. I'm hoping my background in machine learning also helps me here.

## Initial plan
I'd like to focus on the advice given on the previous post and before I start spending too much time on building the MVP, I want to get user feedback. My plan is to get a list of potential first customers by leveraging data from step 2. It will allow me to understand which hotels currently don't have dynamic pricing and I plan on reaching out to them to gauge their interest in a potential software for this. I also want to understand if they believe this is a problem at all, how they are currently tacking it. Potentially it would be useful as well to understand what PMS / Channel Manager they use to distribute their rates.

I'll start with the self-service API from Amadeus, as that's the best option I have right now. I plan on creating a very basic starter website, containing just two kinds of pages:

1.  A landing page. This is the homepage and will contain information on what dynamic pricing is, how it can help hotels and how it would look like in the future.
2.  Custom pages for each of the hotels I reach out to. It should contain a list of competitors, a map with the current location, the competitors locations and their rates. It should show the potential of knowing your competitor rates. Perhaps I could show a particular date I believe they could charge more, or a simulation on how days of the week influence the pricing, or how occupancy rate should influence their rates.

### QuartoVerde
I also have decided on a name already. Quarto Verde, which means green room in portuguese. The motivation being that the main UI of the software will be a calendar. Each day will have a color indicating if their rooms are appropriately priced on that particular day. It will be green if the pricing matches what is suggested by the dynamic pricing software.

It has taken me 2 months to publish this post and I haven't even started working on the website yet. 😅 I'm hoping things can move a bit faster from now on. On the next post I plan to describe how I got the initial list of hotels, the e-mail I plan on sending to the hotels and hopefully a link to the online website!