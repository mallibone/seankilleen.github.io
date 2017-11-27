---
title: "A Free, Open-Source URL shortener for your business -- Introducing xluhco!"
layout: post
date: 2017-11-27 04:39:00.000000000 -05:00
excerpt: "Excella's latest contribution to the OSS ecosystem."

comments: true
---

I'm excited to announce Excella's latest foray into the open-source world -- our new URL shortener, located at <http://xluh.co>.

## The Business Problem

Even as a technology company, sometimes we don't get to pick which URLs we use. Internal applications, vendor tools, and CMSs sometimes generate long URLs that are hard to remember and distribute.

If we created our own URLs, we may not get access to the analytics of who's clicking on our links and where they're coming from. And, if we did go with a tool such as `bit.ly` or any of the other competitors on the market, we wouldn't be able to use our own domain name without paying a pretty penny (`bit.ly` charges roughly $900/month for this functionality at last glance).

What's a tech company to do?...

## Our Solution: xluh.co

...Build our own, of course! And what better way to do so than in the open, with guidance for other businesses on how they can use it as well?

We decided to use [Microsoft's NET Core Framework](TODO) -- a reincarnation of Microsoft's .NET stack. It's cross-platform, which is important to us because we want anyone using this to be able to service it up on Windows, Linux, or on a Mac for maximum flexibility.

On this project, we're using [Appveyor](TODO) for continuous integration to make sure all of our tests pass whenever we submit changes.

The application is deployed via a Microsoft Azure web site that auto-deploys from the `master` branch of the source code repository.

We wanted the application to be accessible even for those who might be less familiar with those concepts, so we also included [a guide to adapting xluco](TODO) for your business's own domain and use. We placed all of the continuous integration settings into a file so that you can set up Appveyor for your own version quickly, and we provide steps on how to set up a deployment to your own domain as well.

## Check it out!

You can find the application live at <http://xluh.co>.

If you'd like to explore the source code, submit an issue, or adapt this tool for your own business, we hope you'll check out [the xluhco repository on the ExcellaLabs Github](http://github.com/ExcellaLabs/xluhco).