---
title: "Building a new computer: small, fast and silent!"
date: 2022-12-11T08:44:28+02:00
draft: true
limmat_temperature: 9.4
tags: ["Tech"]
---

After 11 happy years with my current desktop it was finally showing signs of decay. I started getting BSoDs randomly but frequently, roughly every 30 minutes. Always watchdog alerts (?), which can be a bunch of different things. 

{{< figure src="/images/github_pages_live.png" caption="An infamous blue screen of death. And yes, I'm very much a boomer. I use Windows in my spare time.">}}

I bought most of the parts 11 years ago and assembled it myself:

| Part | Date of Purchase | Price
| :--- | :--------------- | ----:
| **CPU**: Intel Core i5-2500K | 2011-10-16 | €184.74
| **GPU**: 1024MB Gigabyte GeForce GTX 560 Ti | 2011-10-23 | €187.15
| **RAM**: 8GB (2x 4096MB) Corsair Vengeance DDR3-1600 | 2011-10-23 | €42.67
| **SSD**: OCZ Agility 3 60GB | 2011-10-16 | €78.30
| **HDD**: SEAGATE Barracuda Green 2TB 5900.3 SATA 3 | 2011-10-16 | €62.41
| **Power Supply**: Super-Flower SF550P14XE Golden Green Pro 80plus gold | 2011-10-16 | €68.42
| **Motherboard**: Asus P8H67 H67 Sockel 1155 ATX Rev3 | 2011-10-16 | €76.97
| **Case**: Thermaltake Commander MS-I | 2011-10-16 | €37.72
| Total |  | €738.38

At the end of 2015 I was working on my master thesis and needed a more powerful GPU to train [convolutional neural networks](https://www.tensorflow.org/tutorials/images/cnn). Also I wanted an excuse to play games in higher resolution. So I sold my GPU for 45 euros on eBay and got a GeForce GTX 960 for 170 euros, also on eBay. I also bought another SSD and added an additional 8GB of RAM.

| Part | Date of Purchase | Price
| :--- | :--------------- | ----:
| **GPU**: MSI GeForce GTX 960 GAMING 4G | 2015-12-23 | €170.00
| **RAM**: 8GB (2x 4096MB) Corsair Vengeance DDR3-1600 | 2015-12-18 | €46.40
| **SSD**: Samsung EVO 850 250gb | 2015-12-16 | €77.89
| Total |  | €294.29

* 250gb SSD, with Windows 10.
* 60gb SSD, with Debian.

I have a strong suspicion it's actually my 250GB SSD that is dying as I get no crash on Linux. It's actually quite surprising that my HDD is still going strong after such a long time. I tried some debugging (what's the right word?) tools for SSD but actually none found an issue. Perhaps it's not the SSD afterall.

Anyways, everything is so old that even Windows is reminding me that I can't upgrade to Windows 11 as my parts are not supported. I agree with Microsoft and decided it's time to actually change the entire setup.

Incredible it lasted 11 years, considering I went through 6 different phones since then:
* Samsung Galaxy S1
* Google Nexus 5
* Google Pixel 1
* Google Pixel 2
* Google Pixel 3
* Google Pixel 5

## Deciding on the new parts
It has been such a long time since I last built a PC that I barely even know where to start. I just know I want something silent and hopefully a bit more compact. I reach out to my brother for advice and he introduces me to the tiny PC he uses as home server. The box is so small it doesn't support a GPU, but that's actually totally OK with me. I don't game that much and when I do I usually do it on my [Nintendo Switch](TODO).

Here are all the parts I bought and their prices:
| Part | Date of Purchase | Price
| :--- | :--------------- | ----:
| **CPU**: AMD Ryzen 5 5600G | 2022-11-22 | CHF 138
| **RAM**: Crucial Laptop Memory, SO-DIMM, DDR4,  32 GB (2x16GB), 3200MHz | 2022-11-22 | CHF 100.30
| **SSD**: WD Blue SN570 (2000 GB, M.2 2280) | 2022-11-22 | CHF 139
| **WiFi**: ASRock DeskMini WiFi Kit | 2022-11-22 | CHF 27.90
| **Cooler**: Noctua NH-L9a-AM4 | 2022-11-22 | CHF 52.80
| **Case + Power + Motherboard**: AsRock Deskmini X300 | 2022-11-22 | CHF 168
| Total |  | CHF 626

On 2022-11-22 one swiss franc (1 CHF) was worth 1.02 euros (€), so the total cost of the new PC was ~€638, which is about 100 euros less than what I spent 11 years ago. Granted, I did not have to buy a GPU this time, which definitely saved me some money.

{{< figure src="/images/github_pages_live.png" caption="All the parts have arrived! Excitement! Time to assemble.">}}

Talk about order of assembly and which order would have made more sense.
Connecting the cooler was the trickiest one.

Connecting the antennas to the wifi chip was also very finnicky.