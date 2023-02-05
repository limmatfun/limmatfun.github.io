---
title: "Building a new computer: small, fast and silent!"
date: 2023-02-05T00:44:28+02:00
draft: false
limmat_temperature: 6.3
tags: ["Tech"]
---

After 11 happy years with my current desktop, it was finally showing signs of a slow death. I started suddenly getting the dreaded blue screen of deaths, roughly every 30 minutes. They were always DPC Watchdog violations. DPC stands for Deferred Procedure Call and Watchdog refers to a bug checker from Windows. When Watchdog doesn't respond within 100ms, the timeout results in a BSoD. This can be caused by a bunch of different things according to the websites I read [[1]](https://answers.microsoft.com/en-us/windows/forum/all/windows-10-dpc-watchdog-violation/3bb79ffc-3e1d-4bbd-b936-d7df59e6034c), [[2]](https://www.howtogeek.com/742322/how-to-fix-a-dpc-watchdog-violation-in-windows-10/), [[3]](https://www.tomshardware.com/news/how-to-fix-dpc-watchdog-violation-windows-10,36200.html), [[4]](https://www.partitionwizard.com/partitionmagic/dpc-watchdog-violation.html):
*  Device drivers are outdated, or installed incorrectly
*  Newly installed hardware is incompatible with the OS
*  Corrupted or damaged system files

I have a strong suspicion that it's the latter, that some system files are getting corrupted as otherwise I haven't really made any update to the OS or to the hardware, so I don't see how any drivers could be a problem.

{{< figure src="/images/bsod_watchdog.png"  width="60%" caption="An infamous blue screen of death. And yes, I'm very much a boomer. I use Windows in my spare time.">}}

I tried a few of the recommendations, specially using [Windows' System File Checker](https://support.microsoft.com/en-us/topic/use-the-system-file-checker-tool-to-repair-missing-or-corrupted-system-files-79aa86cb-ca52-166a-92a3-966e85d4094e): `sfc /scannow`. It always found a few corrupted files and claimed to fix them, however after 30 minutes the PC would crash again. Since the files would once again be corrupted, I assume something is wrong either with my HDD or with one of my SSDs. I have two SSDs, one with Windows and another with Debian. My PC actually doesn't crash while on Debian, so I suspect the SSD where Windows lives is damaged. I did run a few tests and checked its health and everything seemed fine. However, Windows itself was already complaining my parts are too old to upgrade to Windows 11. I bought most the parts 11 years ago, and assembled it myself. Here's how much it cost back then:

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

I made a small upgrade at the end of 2015. I was working on my master thesis and needed a more powerful GPU to train [convolutional neural networks](https://www.tensorflow.org/tutorials/images/cnn). Also I wanted an excuse to play games in higher resolution. So I sold my GPU for 45 euros on eBay and got a GeForce GTX 960 for 170 euros, also on eBay. I also bought another SSD and added an additional 8GB of RAM.

| Part | Date of Purchase | Price
| :--- | :--------------- | ----:
| **GPU**: MSI GeForce GTX 960 GAMING 4G | 2015-12-23 | €170.00
| **RAM**: 8GB (2x 4096MB) Corsair Vengeance DDR3-1600 | 2015-12-18 | €46.40
| **SSD**: Samsung EVO 850 250gb | 2015-12-16 | €77.89
| Total |  | €294.29

Thinking about it, it's actually incredible that my HDD survived all those years. In the same timeframe I actually went through 6 different phones. According to a [study from Backblaze](https://www.backblaze.com/blog/how-long-do-disk-drives-last/), HDDs median life expectancy is six years and nine months.

{{< figure src="/images/hdd-survival-chart.webp" caption="In Backblaze's experience, an HDD's failure rate no longer matches the bathtub curve. When an HDD is new it's failure rate roughly matches that of the constant failure rate." width="60%">}}

Instead it seems to be actually my SSD that is dying, which is expected to have a longer life expectancy. Anyways, because everything is already quite old and because I use this PC basically daily, I decided it's time to make a full upgrade.

## Deciding on the new parts
It has been such a long time since I last built a PC that I barely even know where to start. I just know I want something silent and hopefully a bit more compact. I reached out to my brother for advice and he introduces me to the tiny PC he uses as home server. The box is so small it doesn't support a GPU, but that's actually totally OK with me. I don't game that much and when I do I usually do it on my Nintendo Switch. Integrated graphic cards today are also quite powerful, here's a video of the [performance AMD Ryzen 5600G vs a real graphics card, GTX 1050 Ti](https://www.youtube.com/watch?v=t2cfIxBCdHA). While the 1050 Ti clearly beats the 5600G, the 5600G can still play many modern games with a decent FPS and high quality. 

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

{{< figure src="/images/computer_all_parts.jpg"  width="80%" caption="All the parts have arrived! Excitement! Time to assemble.">}}

I installed the parts in the following order:
1. CPU
2. RAM
3. WiFi
4. SSD (goes directly over WiFi)
5. Cooler

{{< gallery title="Assembling the PC" description="Some parts were easier to install than the others.">}}
  {{< img src="images/computer/computer_motherboard.jpg" short="Motherboard" title="The X300 motherboard. Small but has everything you need." >}}
  {{< img src="images/computer/computer_cpu.jpg" short="CPU" title="Quite spiky. The triangle on the bottom right corner indicates the correct orientation." >}}
  {{< img src="images/computer/computer_wifi.jpg" short="WiFi" title="It was so annoying to connect these cables to this tiny WiFi board." >}}
  {{< img src="images/computer/computer_cooler.jpg" short="Cooler" title="The Noctua cooler came in quite a fancy box." >}}
  {{< img src="images/computer/computer_cpu3.jpg" short="CPU installed" title="CPU was pretty easy to install. Some minimal force required to insert it. Interesting device with the handle." >}}
  {{< img src="images/computer/computer_ram.jpg" short="RAM installed" title="Installing the RAM required a surprising amount of brute force to make it go in." >}}
  {{< img src="images/computer/computer_wifi3.jpg" short="WiFi installed" title="Once the antennas were in place it was pretty trivial to connect it to the motherboard. Interestingly it goes right below the SSD and has the same M2 plug." >}}
  {{< img src="images/computer/computer_almost.jpg" short="SSD installed" title="CPU, RAM, WiFi and SSD are now in. Only the cooler is missing." >}}
  {{< img src="images/computer/computer_thermal_paste.jpg" short="Thermal paste" title="I likely applied a bit too little thermal paste on top of the CPU." >}}
  {{< img src="images/computer/computer_assembled.jpg" short="Assembled!" title="And we're done!" >}}
{{< /gallery >}}

It was a surprisingly fun process to assemble the PC. There was this feeling of excitement which I would like to have more often. Some of the hardware was really easy to install, such as the CPU and the SSD. The RAM required a surprising amount of brute force; the WiFi cables that connect to the WiFi board were a true pain to connect, extremely finnicky. But the most complicated one was installing the cooler, as you need to install it from underneath, and it goes right over the thermal paste you applied on top of the CPU, without being able to see how it's being spread. 

I'm not sure if the order I installed the pieces was optimal, but I think it was close to it. Perhaps the SSD could have been installed after the cooler, but I think it wouldn't have made a big difference.

{{< figure src="/images/computer_size_difference.jpg" width="60%" caption="The size difference is pretty massive. From 48.4 x 20.2 x 42.6cm (41.6L) to 15.5 x 15.5 x 8.0cm (1.9L), a 95% volume reduction!">}}

So far I've been really happy with the PC. It boots extremely fast and the cooler is incredibly quiet, which is really pleasant to me,  as my old PC would sometimes out of the blue get relatively loud. I hope this one also lasts 10+ years!

### Diggresion: New tools for the blog
As I was writing this post I realized a feel tools were still missing in this blog. I've now added styling for tables and also a small plugin ("shortcode" in Hugo) for an image gallery. It relies on lightbox for presenting the images and I made a small modification to allow arrow keys and escape to browse through them. I'm quite happy with the new stlying and also specially with the custom gallery plugin I wrote! Hugo rocks so far.