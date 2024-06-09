---
title: "Is real estate in Switzerland a good investment?"
date: 2024-06-08T00:47:28+02:00
draft: true
limmat_temperature: 18.2
images: ["images/buy_vs_rent.png"]
tags: ["Investing", "Real Estate"]
---
Buying real estate in a big city like Zurich is [estimated to be 35% more expensive than renting](https://www.srf.ch/news/wirtschaft/immobilien-in-der-schweiz-wo-mieten-guenstiger-ist-als-kaufen-pruefen-sie-ihre-gemeinde) (as [briefly mentioned previously](/posts/work-and-time/#money)). A rough rule is that every 500k in purchase price results in about 1k rent, so a 2M apartment should rent for 4k per month. This is a palsy return-on-investment (ROI) of just 2.4% per year (ignoring taxes, which makes this an even poorer deal). For big, desirable cities like Zurich, the rental yield tends to be even worse, specially due to the lack of inventory (a popular swiss real estate website shows 243 properties for sale on June 8th vs 1039 for rent). 

{{< figure src="/images/buy_vs_rent.png" width="100%" caption="Where in Switzerland is it cheaper to buy than to rent? Blue indicates cheaper to buy than to rent. Red means the opposite. Source: [SRF](https://www.srf.ch/news/wirtschaft/immobilien-in-der-schweiz-wo-mieten-guenstiger-ist-als-kaufen-pruefen-sie-ihre-gemeinde).">}}

Don't let yourself be fooled by the chart above. While by area it may look like it tends to be cheaper to buy than to rent, the big centers where most people live such as Geneva, Basel, Bern, Zug, Luzern and Zurich are very red. These areas tend to have a terrible rental yield.

## So why does anyone even buy real estate?
You should not decide to invest in property just by looking at rental yields. There are mostly two other things that make investing in real estate interesting:
1. Appreciation. Properties tend to be an investment that is considered safe and that have been [increasing in value](https://realadvisor.ch/en/property-prices/city-zurich).
2. Mortgages. Mortgage interest payments are tax free in Switzerland, which is specially good if you're paying high income taxes (you probably have high income if you can afford buying anyways). Additionally mortgages are a form of leveraging your investment, which we will soon learn is a great way to boost your yield.

Of course, the other (irrational) reason is that people just want to own where they live. This is however a not financially sane argument, so let's focus on the reasons above.

### 1. Appreciation
Real estate in Switzerland has historically shown a steady appreciation in value. Property prices only [go brrrr](https://www.reddit.com/r/OutOfTheLoop/comments/hn3qnw/whats_up_with_the_memes_titled_thing_go_brr/) in Switzerland, as clearly shown by the graph below for the city of Zurich (I've also validated this with the data from the official [cantonal website](https://www.zh.ch/de/planen-bauen/raumplanung/immobilienmarkt/immobilienpreise.html); it's correct).

{{< figure src="/images/purchase_prices_zurich_history.png" width="100%" caption="Historical property purchase prices for the city of Zurich. Source: [Realadvisor](https://realadvisor.ch/en/property-prices/city-zurich).">}}

Note that a 148% return in 20 years translates to a ~4.5% ROI per year, which significantly boosts the previous rental yield that we estimated to be 2.4%. For Switzerland the 20 year median appreciation is not quite so extreme, but still a very respectable [84.5%](https://realadvisor.ch/en/property-prices) (~3.1% ROI/y).

#### Why does it keep growing in value?
I'm not an expert, but I believe there are 3 main reasons why real estate tends to gain value:
1. Limited land availability [[1]](https://www.admin.ch/gov/de/start/dokumentation/medienmitteilungen.msg-id-94847.html). Switzerland is a tiny country, with most urban areas already fully developed.
2. Steady population growth [[2]](https://www.bfs.admin.ch/bfs/en/home/statistics/population.assetdetail.26905448.html), mainly due to strong immigration. This creates a natural demand for housing.
{{< figure src="/images/population_growth_switzerland.png" width="70%">}}
3. Low interest rates [[3]](https://www.comparis.ch/hypotheken/zinssatz/zinsentwicklung). Combine low interest mortgage payments with tax deductions and you get people quite interested in buying.
{{< figure src="/images/mortgage_interest_rates_switzerland.png" width="100%" caption="5-year mortgage interest rates. Source: [Comparis](https://www.comparis.ch/hypotheken/zinssatz/zinsentwicklung).">}}

You basically have restricted availability and strong demand: a virtuous cycle of growth in property value. I don't think any of the three is changing any time soon. If anything interest rates will start to go down soon, which will push demand even higher.

### 2. Mortgages
Mortgages are a magic way of increasing your return-on-investment. don't ever fully pay them if you live in Switzerland

## Opportunity cost: stocks
Stocks vs real estate, advtanges and disadvtanges. portfolio diversification.

### Buying a property: the costs

I have been trying to collect all associated costs with buying-to-let property. I tried simulating it with a specific property (just looked for "apartment building" on homegate and picked up something that had clear documentation). This is what I have so far.

Assorted learnings:
*  For buy-to-let your income does not play a role. What plays a role is that Rental Income > All costs by some margin.
*  For buy-to-let you must have a downpayment of at least 25%
*  For buy-to-let you must reduce your mortgage to 65% within 10 years (instead of 15).
*  Capital gain tax (when selling a property for more than you bought): ?

Recurring costs:
*  General maintenance: 1% of value of property
*  Building insurance: Canton-dependent (https://www.comparis.ch/hausrat-versicherung/hausratversicherung/gebaeudeversicherung) but seems negligible compared to the other costs. 
*  Property tax: Canton-dependent, mostly 0 in german-speaking Cantons
*  Tenant insurance: Protects against tenants not paying rent and an apartment being empty for a long time. Not sure if necessary nor how much it costs
*  Management costs ("Verwaltung"): 3.5% bis 5% without new letting (https://www.cash.ch/news/top-news/immobilien-verwalten-lassen-wann-es-sich-lohnt-was-es-kostet-419274). With new letting 12% (!). (Tax deductible: https://www.shkb.ch/828-steuertipps-fuer-liegenschaftsbesitzende and https://moneypark.ch/news-wissen/hypotheken-und-zinsen/steuern-bei-renditeliegenschaften/)
*  Mortgage interest rates: 1.75% bis 2% (tax deductible)
*  Amortization of the mortgage down to 65 % within 10 years: ?
*  Income tax on rental income: Your marginal tax rate, I think.

One-off costs:
*  Mostly notary fees (typically contains some other taxes, not sure what exactly): mostly canton-dependent how much and who pays for it (buyer or seller), some ads seem to split this cost. 2 to 5%

Now simulating it for a specific property.

Property:
*  https://www.homegate.ch/kaufen/4001181128. Price 1.6M, net rental income: 71k
*  400k downpayment
*  1200k mortgage
*  Marginal tax rate: 40%

Costs:
*  Management costs (12% for full service; tax-deductible): 71k*12%*(100%-40%)=5.1k
*  Building insurance (taken from documentation): ~0.5k CHF
*  Maintenance (1% of total cost): 16k
*  Mortgage interest rate (2%, tax deductible): 1200*2%*(100%-40%): 14.4k
*  Income tax on rent: 71k*40% = 28.4k
*  Amortisation: ?
*  Total costs: 64.4k

Which gives a measly yield of (71k-64.4k) / (downpayment of 400k) = 1.6%. I'm hoping there's something wrong with my calculations, or maybe this just means this is not a good property. It's still missing the amortization costs and doesn't take appreciation into consideration.