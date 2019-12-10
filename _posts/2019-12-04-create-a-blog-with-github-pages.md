---
layout: post
title: Create a blog with GitHub Pages
categories: [Jekyll, Blogging]
---

Every man and his dog has a blog now and if you’re serious about a career in software development you should consider it too. Yet being in an industry that moves so quickly it’s hard to find the time to set up a site and blog as well. Conscious of all this I wanted to start a blog as quick and easy as possible without paying too much. Plus might be nice to give back to the developer community 🤷🏻‍♂️.

So here’s my first post on how to set up your own hopefully as quick as succinct as possible.

## The Plan

So the plan is to use GitHub Pages with a custom domain name and using the recommended site generator Jekyll to create your blog. Everything is free except the domain name. Which you can optionally ignore and stick to the default domain that GitHub Pages provides you `<username>.github.io`. Don’t worry nobody will think any less of you if you don’t <3. By the end of this post you should have a nice system to play about with your site. Which you can change or write and publish posts as easy as possible.

## Jekyll

We’re going to start with Jekyll the static site generator. This may seem counter intuitive but it will become clear why we did later.

So Jekyll is a static site generator that GitHub Pages is optimised to use. I’ve not got much (if any) experience with other site generators but on a cursory look it’s easy to keep your content and the site code separate. This could become useful if you ever decide you need to scale up or side step the platform.

First step find a Jekyll theme you like that has the capabilities you need. Here’s a list of things I wanted from my Jekyll theme:

- Simplistic design, since my site is mainly for the purposes of blogging technical articles I don’t need anything too fancy.
- Code block syntax highlighting, again technical blog so this would be useful.
- Mobile and desktop optimised.
- Capability to post in markdown, since I want to be able to create blogs as quick and easy as possible. I definitely don’t want to spend more time on formatting than I need too.

Having your own requirements in mind spend some time finding your own theme. The [Jekyll site](https://jekyllrb.com/) suggests some places to look for themes:

- [jamstackthemes.dev](https://jamstackthemes.dev/ssg/jekyll/)
- [jekyllthemes.org](http://jekyllthemes.org/)
- [jekyllthemes.io](https://jekyllthemes.io/)

Once you’ve decided on a theme you want to use we can move on to getting it on GitHub Pages.

Disclaimer: Choosing a theme was possibly the longest part of the process…

## GitHub Pages 

I won’t explain GitHub Pages in detail but for the purposes of this article all you need to know is that GitHub Pages is a static site hosting service. If you wanna dive in more you can checkout GitHub’s own [help page](https://help.github.com/en/github/working-with-github-pages/about-github-pages#about-github-pages).

So there are a few pro’s to using GitHub Pages:

- Easy
- Free
- On a well known and trusted platform
- Clear and well written docs

Also if the site did manage to gain some traction it would be relatively painless to migrate to another service thanks to how it’s set up using Jekyll.

It does have the limitation of being a static site only. For a blog this isn’t an issue but if you wanted to do something more in depth it might be a problem. GitHub Pages also has usage limits as you would expect. You can go to [GitHub's Pages usage limits](https://help.github.com/en/github/working-with-github-pages/about-github-pages#usage-limits) to find out what they are.

Okay awesome, at this point you’ve settled on a theme, what’s next?

Fork the theme on GitHub which is as easy as clicking fork on the theme’s GitHub repository page.

![alt fork](/images/fork.jpg)

Once you’ve forked it you need to rename the repository to `<username>.github.io` where `<username>` is your own personal GitHub username. There might be some extra setup dependent on the theme you choose or general minor changes to get it running. For example I had to change a few of the settings in the `_config.yml` file that you get with every Jekyll site. Once you’ve done all the little changes and pushed your repo you should see your own GitHub page at the URL `<username>.github.io` (although this can take up to 10 minutes to update).

Now at this point you could easily start blogging but there are a couple of extra’s that you might find useful or beneficial.

## Local Jekyll

Great you’re all set up you can create a post or change all the settings in the theme to make it your own but you’re likely to find you’ll quickly get frustrated with how long the feedback loop from a change to seeing it on your website. This could also be an issue if your site is already getting visitors regularly as you don’t want to do potentially breaking changes and publishing to your site just to review your latest blog post or fancy layout.

A solution to this is running a local version of Jekyll where you can play about and break all the things in the world before calming your passions and doing what you set out to do. I also found it useful to review a blog post in all it’s glory with links, formatting and images. To do this we’re going to need to install bundler and then use that to install Jekyll and all the gems needed to run your theme locally. I’m assuming that you already have a decent ruby environment setup, otherwise you may need to set that up first before continuing.

Install bundler with the following:

```
gem install bundler
```

Next we need to create a `Gemfile` at the root of your repository with the following:

```ruby
source 'https://rubygems.org'

gem "jekyll"

# Jekyll plugins
gem "jekyll-sitemap"
gem "jekyll-feed"
gem "jekyll-seo-tag"
gem "jekyll-paginate"
```

The above is my `Gemfile` but you’ll need to replace the Jekyll plugins section with whatever plugins your theme uses. You should be able to find yours in the `_config.yml` file in the root of your repository.

Once you’ve created that you can install the gems:
```
bundle install
```

And finally run your local Jekyll site:
```
bundle exec jekyll serve
```

You should now be able to see your site at [http://localhost:4000](http://localhost:4000) and reload it to view your changes. 🎉

## Domain Name

So maybe you’re like me and want to go the whole hog and not even want people to realise that you’re not actually hosting your own site? Well then you’re going to need a domain name.

First step, pick a domain name. This sounds relatively easy but it’s been a while since I owned a domain name and unfortunately I am cursed with a surprisingly common name and a poor imagination. I resorted to finding a [random blog name generator](https://www.name-generator.org.uk/blog/). Then waiting till I was amused which landed me this slightly egotistical reminder of a lot of swift protocols. Perfect for a predominately iOS mobile developer.

Finding a domain provider isn’t difficult but I went with [godaddy.com](https://uk.godaddy.com/) because it was one of the cheapest ones. Equally I don’t think it matters too much which domain name provider you go for unless you have other aspirations.

Now we need to manage the DNS. This is one of the harder parts of setting up your own domain name and I don’t fully understand it nor do I mind that I don’t. Lucky for me that topic has already been covered by Hackernoon: [how to set up GoDaddy domain with GitHub Pages](https://hackernoon.com/how-to-set-up-godaddy-domain-with-github-pages-a9300366c7b).

## Summary

So in summary:

- You’ve found a Jekyll theme
- Forked it and renamed it to become your own github pages website
- Customised your theme a little
- Setup up a local Jekyll server so you can preview your site
- And optionally also set up your own domain name

Now all that’s left is for you to go away and blog to your hearts content.. have fun and share this article if you found it helpful!