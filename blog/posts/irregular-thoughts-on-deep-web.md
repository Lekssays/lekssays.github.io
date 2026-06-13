---
title: "Irregular Thoughts on Deep Web"
date: "2018-03-01T22:28:42+00:00"
modified: "2018-03-01T22:39:18+00:00"
slug: "irregular-thoughts-on-deep-web"
author: "Ahmed Lekssays"
featured_image: "../images/irregular-thoughts-on-deep-web/bd14326a-deep_web_02a-1050x630.jpg"
categories: ["Computer Science"]
tags: ["Cyber Security", "Dark Web", "deep web", "privacy"]
original_url: "https://lekssays.wordpress.com/2018/03/01/irregular-thoughts-on-deep-web/"
excerpt: "I have been reading many articles that tackeled the topic of deep web especially in Arabic. And I noticed that it became a &#8220;fancy&#8221; topic to attract people&#8217;s curiosity. However, they lack the sound technical understanding of the matter. I know some of you would think that I should w"
---
## Introduction

I have been reading many articles that tackle the topic of the deep web, especially in Arabic, and I noticed that it has become a "fancy" subject used to attract people's curiosity. However, these articles lack a sound technical understanding of the matter. I know some of you would think I should write this post in Arabic, since the source of confusion comes from Arabic-based hacking websites, but I believe that people who are genuinely interested in this topic will read it in English. So let's start the adventure.

In this post, I want to clear up two main points of confusion that some websites share:

1. The deep web does not mean the dark web.
2. The layered view of the deep web is technically wrong.

## The Deep Web Is Not the Dark Web

The web in general can be divided into two types: the indexed web and the deep web.

- **Indexed web:** This is what we usually see and use. It is called the indexed web because it is accessible by search engines.
- **Deep web:** This can be further divided into two layers, the restricted access zone and the dark web.
  - **Restricted access zone:** The part that is confidential to you, so no one else has access to it, not even search engines. Examples include your Gmail, AWS Console, and private GitHub repositories.
  - **Dark web:** A deeper layer than the restricted access zone. Special software, such as Tor, is needed to explore it. The dark web consists of a set of darknets that cannot be accessed through search engines. They are only known and accessible to those who created them (or who know the address), and they typically use an address in the following format: *somethingencrypted.onion*.

## The Layered View Is Technically Wrong

When a person hears about the deep web, the first idea that comes to mind is that the web we know today sits at the top and the deep web lies at the bottom. This is also how it is promoted on Arabic "technical" websites. This view is technically wrong.

The web we use now is actually at the bottom, because it provides the necessary protocols and infrastructure to access the deep web. What I noticed is that the writers of those articles talk about the deep web in a fancy, fantastic way, as if they were directing a movie. Come on, guys! You are "technical writers". The way this matter is handled hides the scientific and technical core of the deep web.

For example, the Tor browser is built on top of Firefox. This simple example alone proves that the promoted layered view is technically incorrect, since the endpoint of the dark web is built on top of software we already know from the "normal" web.

Thus, considering the deep web to be deeper than the web we know is wrong.

I hope this clarifies the ambiguity, and thank you for reading!

Image Copyright: <http://www.the13thfloor.tv/2017/05/16/thinking-of-exploring-the-deep-web-heres-a-handy-road-map/>
