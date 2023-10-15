---
title: "Introducing QuartoVerde"
date: 2023-10-15T00:45:28+02:00
draft: false
limmat_temperature: 18.7
images: ["images/quarto-verde-logo.png"]
tags: ["Tech", "QuartoVerde"]
---

I talked about the idea of building a dynamic hotel pricing product in the posts [inspiration from startup school](/posts/inspiration-from-startup-school#idea-dynamic-hotel-pricing) and also [dynamic hotel pricing: an initial plan](/posts/dynamic-hotel-pricing-initial-plan). I decided to follow the advice given by Y Combinator and focus on getting early feedback from potential clients, even before building the real product. I created a barebones landing page and QuartoVerde is finally live on [quartoverde.com.br](https://quartoverde.com.br)!

{{< figure src="/images/quartoverde_hotel_page.png" width="80%" caption="A custom hotel page on QuartoVerde">}}

## Making of: how the landing page was built
I decided the website would consist of only two pages: 
*  Landing page: basic introduction to the product (which doesn't exist so far) and an easy way for potential clients to reach out
*  Hotel-specific page: a custom page for each hotel I plan on reaching out to. It only displays basic hotel information such as its name, location, one picture of it and a calender of prices for the current month. It tries to lures hotels to get in touch by showing an example date and a suggestion of price adjustments.

Since I work fulltime, I don't have the time to build everything perfectly. I tried leveraging existing tools and frameworks wherever I could. 

### Landing page design
The landing page uses a [Bootstrap](https://getbootstrap.com/) template called [Start Bootstrap - New Age](https://startbootstrap.com/theme/new-age) with some modifications. I had used Bootstrap many ages ago when I used to do websites and was pleasantly surprised by how easy it is to use. There's CSS classes for everything you want to do, so there's no need to write any special CSS or HTML.

### Logo
For the logo I used a free online tool called [logo.com](https://logo.com). This was great inspiration and generated awesome logos. I made some small modifications using [Gimp](https://www.gimp.org/) as the original had too much whitespace. It now fits nicely at the top of the landing page.

{{< figure src="/images/quarto-verde-logo.png" width="60%">}}

### Call to action: WhatsApp
whatsApp is absolutely ubiquitous in Brazil. **Everyone** has it and it is used by many companies as the main form of communication with customers. This is actually a practical thing for me in my case, as I'm abroad and would have otherwise trouble communicating with clients via landline phone calls. WhatsApp makes it very easy for me to stay in touch with them in an absolutely natural way.

Since I'm based in Switzerland and using [WhatsApp Business](https://business.whatsapp.com/) requires a phone number, one of the challenges was getting a brazilian phone number to use. Luckily I'm a brazilian national and was able to register an e-SIM from [Claro](https://www.claro.com.br/celular/esim) fully online for R$20 (about $4). What went not so smoothly is that scanning the QR code for loading up the e-SIM on my phone did not work from Switzerland. I assume some part of the authentication relies on the cellphone network connection actually being located in Brazil. To solve this I had to reach out to a friend in Brazil to register the e-SIM on their phone. WhatsApp sends the verification code via SMS, so once my friend received the SMS and shared the code with me I was able to setup the account on WhatsApp on my side of the world.

### Tracking website visits
To track website visits I used [smartlook](https://www.smartlook.com/), which is free to use up to 3k monthly sessions. I was impressed by how nicely it tracks what the user doing. For desktop visits it tracks the users' mouse movements and on mobile it tracks taps. It's a brilliant insight into what users are doing on the website and how long they are spending on it. 

### Web framework: Flask
For the web framework I decided to use [Flask](https://flask.palletsprojects.com/). While I used to do websites using [Django](https://www.djangoproject.com/), I decided to start something more lightweight this time. The documentation for Flask is great and it was very easy to get up and running in no time.

### Hosting & Deployment
For hosting I used bare metal shared hosting from [Hetzner](https://www.hetzner.com/cloud). It has quite good value for money, only costing €4.50 per month. The downside is that you need to setup everything on your own and I'm not a particularly experienced sysadmin. It consumed a lot of my time to understand how to deploy using [Nginx](https://www.nginx.com/), [gunicorn](https://gunicorn.org/) and automatic SSL certificates. I used a starter [GitHub project](https://github.com/smallwat3r/docker-nginx-gunicorn-flask-letsencrypt) that had all these tools and docker to make environments and deployment easier to manage. Still, I don't feel like my setup is particularly robust or safe, as I'm not regularly updating the linux environment on Hetzner nor the Python dependencies.

I might consider moving to something like [Google App Engine](https://cloud.google.com/appengine) or [Fly.io](https://fly.io) as that would allow me to focus on actually developing instead of being a sysadmin.

### Email: Google Workspace
For e-mail I actually started with [Zoho](https://www.zoho.com/mail/) but decided in the end to move to [Google Workspace](https://workspace.google.com/) which unfortunately doesn't have a free tier anymore. Nonetheless it's not too costly at $7 a month. I decided to move for two reasons:
1.  I wanted to use a cold e-mailing tool and [GMass](https://www.gmass.co/) had excellent reviews and seemed easy to use. It's specifically made for Gmail and promises excellent delivery rate.
2.  I'm already very familiar with Gmail, including having the app on my phone. It's very practical to have a second account on the same app. Additionally this grants me access to other Google tools which I like such as [Docs](https://www.google.com/docs/about/) and [Colab](https://colab.google/).

## Generating a list of potential interested hotels
The next step was generating a potential list of interested hotels. My idea was to reach out to hotels that currently to not employ any form of dynamic pricing, i.e. hotels that have a constant price every day. To do this I used the Booking.com API I mentioned on the post comparing [Amadeus, Booking.com and Expedia](/posts/amadeus-hotel-api-review). 

I decided it would be a good idea to start with a touristic city and chose [Ouro Preto](https://whc.unesco.org/en/list/124/) (Ouro Preto is a UNESCO World Heritage Site) as a starting point. I collected and analysed the data on Colab. From my analysis there's about 120 hotels in Ouro Preto and out of those roughly 50 don't use dynamic pricing. I then sorted them by review score and review count on Booking.com to prioritize the ones that are probably largest and most active in getting in bookings.

### Cold-emailing hotels
Now that I have a landing page and potential list of interested hotels, I'm ready to reach out to people! Exciting! One last step was thinking about how the e-mail should look like. I followed the advice from [Y Combinator](/posts/inspiration-from-startup-school#how-to-get-your-first-customers) and focused on a small number of sentences to describe the problem and the product. Here's how it looks like (translated from portuguese):

> Title: An idea for your hotel
>
> Hello {{ hotel name }},
>
> I found your incredible property on Booking.com and noticed that your prices are constant, regardless of the day. I think there's a lot of potential in increasing your revenue with dynamic pricing.
>
> I'm <my name> from QuartoVerde. We are a small company that helps you set your prices automatically using artificial intelligence.
>
> We have created a custom page taiored to your hotel: {{ hotel link }}
>
> Would you be interested? Just respond to this email or send a message via WhatsApp: https://wa.me/message/NCKXO6CQBFRSH1

### First results & next steps
Out of the first 5 hotels I reached out to, 4 didn't even open their hotel-specific page and the other hotel straight out rejected the proposal. These results are a bit sobering, but there's two lessons here:
1.  5 hotels is too little. I need to scale up the number of properties being reached out to. However many of these small properties don't even have an e-mail adress to reach out to.
2.  It's not clear to me anymore if selecting hotels that don't use dynamic pricing at all is actually a good strategy. There's perhaps a reason that they are doing this and don't want to change at all. 

For the next batch I want to reach out to a larger number of hotels and focus on a different slice: those that have some form of dynamic pricing but that is not very robust, i.e. only have a few different prices (e.g. a price for weekedays and another one for weekends).