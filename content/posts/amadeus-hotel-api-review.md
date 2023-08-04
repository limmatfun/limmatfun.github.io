---
title: "Amadeus Self-Service Hotel API: a short review"
date: 2023-08-04T00:47:28+02:00
draft: false
limmat_temperature: 20.7
images: ["images/dynamic_hotel_pricing.jpg"]
tags: ["Tech", "QuartoVerde"]
---
[Amadeus](https://developers.amadeus.com/) is a GDS (Global distribution system), offering a diverse set of travel APIs: flight offers, cars and transfers, hotels, destination experiences, market insights and more. Every API is offered both as a "self-service" API and as an enterprise API. The self-service API is rather unbureaucratic. Anyone can sign up for it and start using it immediately. The API has a [free tier and is otherwise on a pay-as-you-go](https://developers.amadeus.com/pricing) fashion, charging per API call. The downside is that its functionality is more limited compared to the enterprise version. Enterprise however requires a formal sign-up process and implies larger costs: every client must go through a travel consultant, who will take you through the API. 

This post focuses on the [Amadeus "self-service" Hotel API](https://developers.amadeus.com/self-service/category/hotels). Amadeus claims it has deals with more than 150,000 hotels worldwide. In particular I want to understand how this number compares to large OTA's such as Booking.com and Expedia, which claim to have access to millions of hotels.

They offer a myriad of ways of interacting with their API, their documentation is well written and they have a rich set of [guides and prototypes](https://amadeus4dev.github.io/developer-guides/examples/prototypes/). They have examples in an impressive set of languages: Python, PHP, Kotlin, Swift, Java, Ruby, Javascript and more.  In my case I was mostly using their [Python lib](https://github.com/amadeus4dev/amadeus-python), which worked very well and was very intuitive to use.

### Hotel List API
The [Amadeus Self-Service Hotel List API](https://developers.amadeus.com/self-service/category/hotels/api-doc/hotel-list/api-reference) is not very powerful. You are limited to searching for hotels by airport code (finds hotels within a 5km range of the airport) or by latitude and longitude coordinates. There is no option to search by a city's name (or id). It's pretty clear how Amadeus has strong ties to the Airline industry given the focus on searching by [IATA airport codes](https://www.iata.org/en/publications/directories/code-search/). Luckily Amadeus does provide an [API to search for cities](https://developers.amadeus.com/self-service/category/destination-experiences/api-doc/city-search/api-reference) and it returns geocode coordinates you can use to search hotels.

#### City Search API
The City Search API however is not great. I haven't tested cities outside of Brazil, but it does not work well for the cities I tested. The API is very simple, you pass a keyword and it gives you matches back. Here's the result for `Ubatuba`, a popular touristic city in São Paulo.

{{< highlight JSON >}}
[
   {
      "type":"location",
      "subType":"city",
      "name":"Mtubatuba",
      "iataCode":"DUK",
      "address":{
         "countryCode":"ZA",
         "stateCode":"ZA-ZZZ"
      },
      "geoCode":{
         "latitude":-28.41789,
         "longitude":32.18483
      }
   },
   {
      "type":"location",
      "subType":"city",
      "name":"Ubatuba",
      "iataCode":"UBT",
      "address":{
         "countryCode":"BR",
         "stateCode":"BR-SP"
      },
      "geoCode":{
         "latitude":-23.43389,
         "longitude":-45.07111
      }
   }
]
{{< / highlight >}}
Note that the results are not ordered based on relevance or popularity. Even though one of the results is an exact match, it still returns `Mtubatuba` first and `Ubatuba`, what I was looking for, second.

The API can also behave oddly, sometimes returning repeated records, and even missing geocodes, which is the whole point of the API in the first place. Here's the response for `Gramado`, one of the most popular touristic cities in the south of Brazil.
{{< highlight JSON >}}
[
   {
      "type":"location",
      "subType":"city",
      "name":"Gramado",
      "iataCode":"QRP",
      "address":{
         "countryCode":"BR",
         "stateCode":"BR-RS"
      },
      "geoCode":{
         
      }
   },
   {
      "type":"location",
      "subType":"city",
      "name":"Gramado",
      "iataCode":"QRP",
      "address":{
         "countryCode":"BR",
         "stateCode":"BR-RS"
      },
      "geoCode":{
         
      }
   }
]
{{< / highlight >}}

For other popular touristic cities in Brazil such as `Porto de Galinhas` and `Jericoacoara` the API simply returns an empty response, which is surprising. Those are truly popular destinations and Amadeus simply does not provide a programmatic way of looking for hotels in those cities.

## Hotel coverage in Brazil: Amadeus vs Booking.com vs Hotels.com
I wanted to understand how the coverage of hotels in Brazil differs between Amadeus, [Booking.com](http://booking.com) and [Hotels.com](http://hotels.com) (part of the Expedia group). In theory Amadeus is more focused on business hotels, so I'm expecting its coverage to be worse in small touristic cities compared to large cities. For this non-scientific test I used two sets of 10 cities: touristic and business cities (to simplify I took the top 10 by population). I checked how many hotels were returned by each API. 

Notes: 
*  The comparison is not entirely fair, as Amadeus can only search by range (I used the default 5km), while Booking.com and Hotels.com truly search by city. This is an advantage for Amadeus in smaller touristic cities like `Itacaré` and a disadvantage in large business cities such as `São Paulo`.
*  Additionally Amadeus is returning all hotels that exist in that range, regardless of their availability. Booking.com's API however only returns hotels which are available in a certain period. To try to leverage the playing field, I took a period of 2 nights spanning November 7th to November 9th, requesting a double room. This period is not during school holidays, so I'd expect most hotels to be available and a good proxy for the number of hotels in a city.
*  For Amadeus I had to manually input the city center coordinates for every city to be able to make this comparison.
*  I also sanity checked the results from the Booking.com and Hotels.com APIs on the corresponding websites. They matched.

### Touristic cities
 City   |      Amadeus      |  Booking.com | Hotels.com |
|:----------|-------------:|------:|------:|
| Ubatuba |  1 | 1120 | 319 |
| Porto de Galinhas |    10   |   969 | 250 |
| Jericoacoara | 9 |   215 | 124 |
| Ilhabela | 4 |   367 | 221 |
| Foz do Iguaçu | 45 |   279 | 95 |
| Gramado | 25 |   691 | 237 |
| Ouro Preto | 6 |   88 | 32 |
| Paraty | 3 |   491 | 196 |
| Bonito | 4 |   91 | 47 |
| Itacaré | 3 |   150 | 57 |

### Business cities (top 10 by population)
 City   |      Amadeus      |  Booking.com | Hotels.com |
|:----------|-------------:|------:|------:|
| São Paulo |  116 | 2063 | 476 |
| Rio de Janeiro |    42   |   1264 | 234 |
| Brasília | 27 |   227 | 76 |
| Fortaleza | 30 |   422 | 185 |
| Salvador | 27 |   466 | 135 |
| Belo Horizonte | 52 |   201 | 131 |
| Manaus | 20 |   120 | 50 |
| Curitiba | 64 |   470 | 118 |
| Recife | 9 |   286 | 106 |
| Goiânia | 33 |   287 | 81 |

### Conclusion
Unsurprisingly Amadeus' coverage in Brazil is not great. While it has some minimum coverage in large cities, where hotel chains are largely present, the coverage in touristic cities is abysmal. What I was surprised to see is how Booking.com truly dominates as an OTA, dwarfing even the results from the Expedia Group (Hotels.com). Booking.com frequently had 4x the number of hotels compared to other providers.

Additionally both Booking.com and Hotels.com had very convenient APIs for searching locations. The response from this endpoint was also ordered by relevance, making a programatic solution to lookup hotels for a large number of cities only based on their city names very easy to program. 

I plan on using the Booking.com API in the future for QuartoVerde.

#### Appendix: A note on test vs prod environments for Amadeus
Oddly, the test environment for Amadeus consistently returned more hotels than the production environment. I manually checked the results from the test environment and those seem to be real hotels, so I don't quite understand why production would have fewer results. 🤷

 City   |      Amadeus Test Environment    |  Amadeus Production Environment | 
|:----------|-------------:|------:|
| Ubatuba |  8 | 1 |
| Porto de Galinhas |    13   |   10 |
| Jericoacoara | 8 |   9 |
| Ilhabela | 17 |   4 |
| Foz do Iguaçu | 102 |   45 |
| Gramado | 39 |   25 |
| Ouro Preto | 10 |   6 |
| Paraty | 22 |   3 |
| Bonito | 4 |   4 |
| Itacaré | 12 |   3 |
