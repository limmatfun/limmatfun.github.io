---
title: "Amadeus Self-Service Hotel Search API: a short review"
date: 2023-07-15T00:47:28+02:00
draft: true
limmat_temperature: 24.8
images: ["images/dynamic_hotel_pricing.jpg"]
tags: ["Tech"]
---
*  What is Amadeus
*  Features from the self-service API
[Amadeus](https://developers.amadeus.com/) is a GDS (Global distribution system), offering a diverse set of travel APIs: flight offers, cars and transfers, hotels, destination experiences, market insights and more. Every API is offered both as a "self-service" API and as an enterprise API. The self-service API is rather unbureaucratic. Anyone can sign up for it and start using it immediately. The API is on a pay-as-you-go fashion, charging per API call. The downside is that its functionality is more limited compared to the enterprise version. Enterprise however requires a formal sign-up process and implies larger costs. Every client must go through a travel consultant, who will take you through the API. 

This post focuses on the [Amadeus "self-service" Hotel Search API](https://developers.amadeus.com/self-service/category/hotels/api-doc/hotel-search). Amadeus claims it has deals with more than 150,000 hotels worldwide. This number is however is not so impressive compared to Booking's millions of hotels.

[Python lib](https://github.com/amadeus4dev/amadeus-python)
