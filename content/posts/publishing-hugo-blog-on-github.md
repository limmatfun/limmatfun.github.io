---
title: "How this blog is published"
date: 2022-10-20T12:44:28+02:00
draft: true
limmat_temperature: 16.0
tags: ["Tech"]
---

Once I decided I wanted to write a blog, I started looking at what options were out there. You can divide blogging into almost 3 different categories:

*  Large, estabilished platforms such as [Wordpress](https://wordpress.com/), [Medium](https://medium.com), [Wix](https://wix.com) and [Squarespace](https://squarespace.com). These options tend to be on the commercial side and also include ads in their free oferring. Wordpress is perhaps the most popular, or used to be the most popular blogging framework, riding on the wave of PHP and MySQL.
* No-ads, privacy-focused, minimalistic platforms: [Bear](https://bearblog.dev/) and [Mataroa](https://mataroa.blog/). These are also the ones that tend to be hyped by [Hacker News](https://news.ycombinator.com/). You can start a blog quickly and they support custom domains, but you can't even change the design or stylesheet of your blog.
* Self-hosted, static content generators: [Jekyll](https://jekyllrb.com/) and [Hugo](https://gohugo.io/) are the largest contenders in this space. Jekyll is more popular and Hugo tends to be known for its speed.

The first category was not an option for me as I definitely didn't want any ads on my blog. I could self-host Wordpress and then have no ads, but honestly I was already dreading having to use PHP and MySQL. Plus I was interested in trying something new. 

The minimalistic platforms were aligned with my values, but not being able to change the design even by a little bit was a dealbreaker for me. I also want my blog to have some personality and not look as boring as all other blogs in the same platform. 

So only one option was left, and I quited liked the idea of a static content generator. It just makes sense for a blog. Blog posts typically never change, so why would you even need a database for storing them. You write posts in [Markdown](https://www.markdownguide.org/getting-started/), which is a quite simple language to use and I'm already very familiar with it anyways.

## Static content hosts

The nice thing about them is that there are many ways of hosting them, even for free, with large, reliable hosts such as [Github Pages](https://pages.github.com/), [Gitlab Pages](https://docs.gitlab.com/ee/user/project/pages/) and [Netlify](https://www.netlify.com/). They all also allow using a custom domain and SSL encryption, for free. I think it's quite neat that you can have your own personal blog, with full control of it, and the only cost you have is the custom domain you're using. For the hosting platforms the cost is also very little, as static content generators are very lightweight and as their name indicates, generate static content. Serving this kind of content is fast and cheap to do.

## Jekyll vs Hugo

I must confess I probably didn't do much due deligence in deciding between Jekyll and Hugo. Jekyll is quite famous for also having an [official integration](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll) with Github Pages. This makes it specially practical to host your blog, as it can be done for free and by using the integration, whenever you push your repository, it will also update your blog automatically via [Github Actions](https://github.com/features/actions). Jekyll runs using Ruby.

On the other hand Hugo is known for being much faster than Jekyll, only requiring a single binary to run as it's compiled using Go. This makes it convenient to run from anywhere as installation is minimal. Since I was planning on writing this blog from at least two different computers, that's how my decision for Hugo went: practicality.

## Hosting a Hugo blog on Github Pages

I also didn't do any diligence on where to host my blog and just decided to use Github Pages because I like Github. Obviously the first thing I did was to Google how to host a Hugo blog on Github Pages and found an official tutorial from [Hugo on this topic](https://gohugo.io/hosting-and-deployment/hosting-on-github/). Somewhat disappointingly, the tutorial instructions did not work. I would just get an error when trying to host it.

To my surprise, Github itself had a pre-generated Github Action for Hugo blogs, and it seemed to detect that my repository was a Hugo blog, so this was automatically suggested to me. 

![hugo_github_action.png](/images/hugo_github_action.png)

I was quite pleased that this one actually worked out of the box. After pointing my [custom domain to Github Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages), my blog was live at [limmat.fun](https://limmat.fun)! 

{{< figure src="/images/github_pages_live.png" caption="Yay, my blog is live!">}}

It was also quite easy to request SSL, just one checkbox. 

![github_pages_domain_ssl.png](/images/github_pages_domain_ssl.png)

Overall the experience was great and I can really recommend Github Pages. It's super practical that whenever I push content to my repository that it also automatically updates my blog. This is exactly the setup I was looking for and requires minimum work from my side.

## Customizing Hugo

### Themes

The nice thing about Hugo as well is how easy it is to customize it. There's a very large list of free themes available at [Hugo Themes](https://themes.gohugo.io/). I picked [Ink](https://github.com/knadh/hugo-ink) as a theme and made some modifications to its stylesheet to make it more water-themed. All this required was creating a new file under `static/css`, changing the `config.toml` to set the parameter `customCSS = "/css/water_default.css"`.

### Adding the Limmat temperature

Now it was time to sprinkle some personality into this blog. My partner gave me the brilliant idea of putting the Limmat temperature prominently in the header since this blog is named after the river Limmat anyways. This involves two steps: 

1. Querying some API that will give me the temperature;
2. Calling this API from Hugo at content generation time.
3. Periodically rebuilding the blog.

#### Step one: Using the Hydrological API

First step was actually getting the data. Typically I accessed this information via the city's [official website listing the temperature of different bodies of water in Zurich](https://www.zh.ch/de/umwelt-tiere/wasser-gewaesser/messdaten/wassertemperaturen.html), which just queries the data from [BAFU](https://www.bafu.admin.ch/bafu/de/home.html) (BAFU is the 'Bundesamt für Umwelt', i.e. the Federal Office for the Environment). I then found the official form to [request access to the hydrological API from BAFU](https://www.bafu.admin.ch/bafu/de/home/themen/wasser/zustand/daten/messwerte-zum-thema-wasser-beziehen/aktuelle-hydrologische-daten-beziehen.html). I was actually ready to go through with it and make the request when I found the [Existenz API](https://api.existenz.ch/), a free API for non-commercial usage by [Christian Studer](https://www.hydrodaten.admin.ch/de/2243.html). It supports the [Hydrology API](https://api.existenz.ch/#hydro) natively, which is exactly what I was looking for.

It was a matter of just finding the ID that identifies the Limmat river measurement station in Zurich. The official website has  a map containing all [measurement stations](https://www.hydrodaten.admin.ch/de/messstationen_temperatur.html), so all I had to do was to click the right one and ta-da, the station ID is [2243](https://www.hydrodaten.admin.ch/de/2243.html). 

Querying the [Hydrological Existenz API with ID 2243](https://api.existenz.ch/apiv1/hydro/latest?locations=2243&parameters=temperature,flow&app=limmat.fun) gives me the temperature of the Limmat nicely formatted as JSON. Jackpot!

{{< highlight JSON >}}
{
   "source":"Swiss Federal Office for the Environment FOEN \/ BAFU, Hydrology",
   "apiurl":"https:\/\/api.existenz.ch#hydro",
   "opendata":"https:\/\/opendata.swiss\/en\/dataset\/wassertemperatur-der-flusse",
   "license":"https:\/\/www.hydrodaten.admin.ch\/uploads\/attachments\/148\/agb_fur_das_herunterladen_aktueller_hydrologischer_messdaten.pdf",
   "payload":[
      {
         "timestamp":1666365600,
         "loc":"2243",
         "par":"temperature",
         "val":16.3
      },
      {
         "timestamp":1666365600,
         "loc":"2243",
         "par":"flow",
         "val":77.93
      }
   ]
}
{{< / highlight >}}

#### Step two: Calling the API from Hugo
Hugo natively supports [getting remote data](https://gohugo.io/templates/data-templates/#get-remote-data) via its `getJSON` function. Getting the data was as simply as calling the function below in `layouts/_default/partials/head.html`:

```
{{ $dataJ := getJSON "https://api.existenz.ch/apiv1/hydro/latest?locations=2243&parameters=temperature,flow&app=limmat.fun" }}
```

[Partials](https://gohugo.io/templates/partials/), by the way, is how Hugo keeps your templates DRY. By creating `head.html` inside `layouts/_default/`, I'm telling Hugo I want it to override the theme's `head.html`.

#### Step three: Periodical rebuilding on Github Pages
The blog only gets rebuilt if I push to the repository, but I want the temperature on the header to be somewhat accurate. To do this I simply added a new Github Action using the `schedule` clause, rebuilding the blog every hour.

{{< highlight YAML  >}}
on:
  # Run every hour at :30
  schedule:
    - cron: '30 * * * *'
{{< / highlight >}}

bla Quite happy with Hugo overall as well.