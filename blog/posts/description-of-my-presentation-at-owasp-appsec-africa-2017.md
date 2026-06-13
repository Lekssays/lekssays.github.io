---
title: "Description of my Presentation at OWASP AppSec Africa 2017"
date: "2017-02-03T00:32:19+00:00"
modified: "2017-04-19T13:21:01+00:00"
slug: "description-of-my-presentation-at-owasp-appsec-africa-2017"
author: "Ahmed Lekssays"
featured_image: "../images/description-of-my-presentation-at-owasp-appsec-africa-2017/18c86550-shutterstock_165303932.jpg"
categories: ["Computer Science"]
tags: ["Cyber Security", "open source"]
original_url: "https://lekssays.wordpress.com/2017/02/03/description-of-my-presentation-at-owasp-appsec-africa-2017/"
excerpt: "I was invited to talk at OWASP AppSec Africa 2017 in Casablanca, Morocco on Wednesday, February 1st, 2017. My presentation was entitled:&#8221;How Did I Hack Twitter and WhatsApp for iOS?&#8221;. I had the honor to present in front of well-educated people about cyber security. I hope this made a cha"
---
I was invited to speak at OWASP AppSec Africa 2017 in Casablanca, Morocco, on Wednesday, February 1st, 2017. My presentation was entitled "How Did I Hack Twitter and WhatsApp for iOS?". I had the honor of presenting on cyber security in front of a well-educated audience, and I hope it made a difference or opened a path for people who want to pursue a career in cyber security, especially on the iOS platform.

## Two Discoveries: Twitter and WhatsApp

In this presentation, I discussed two of my discoveries as a security researcher: one in the Twitter application for iOS (2014) and one in WhatsApp for iOS (2015).

- The first was an open authentication flaw that allowed me to hijack an active session in the Twitter application.
- The second was an encryption problem in WhatsApp that allowed me to steal the conversations and contacts stored on a device. After I reported that vulnerability, WhatsApp applied end-to-end encryption, which has since protected millions of users.

These discoveries were considered achievements because they were the first Moroccan discoveries on the iOS platform.

## An Introduction to iOS Security

As an introduction to these discoveries, I talked about iOS security architecture, a rare field in the Moroccan cyber security community. I shed light on the system vulnerabilities that allowed me to access important files in installed applications, and I gave an overview of the iOS security system. I also mentioned some design patterns in operating system design that distinguish the system, kernel, and user modes, known as GDT entries (Global Descriptor Table entries).

## Bypassing the Lock and Accessing System Files

One of the famous bugs in iOS is bypassing the lock, either from the device itself or from a computer. At this point, there are three main paths to follow: Ubuntu (or another Linux-based distribution), Mac OS X, or Windows. I tried them all, and I noticed that they dealt with the iDevice in different ways. Ubuntu tried to access it as a physical hard drive, while the others treated it as an iDevice (trying to connect it with iTunes).

For WhatsApp, the bug was in iOS 9. I could access the system files, including the files of the applications themselves. At this level, I would like to describe how an iOS application works based on the general file hierarchy in iOS. In other words, I would like to explain the role of ".plist" files in the iOS system.

## Authentication and the Twitter Bug

Concerning Twitter's bug, I shed light on the multiple authentication levels in mobile applications, for instance the access token method, which was the main factor in the bug I discovered in Twitter. I would also like to talk briefly about the third-party applications that are widely used today and the security risks they pose to users. This bug leads us to explain the difference between authorization and authentication, and to explore in depth the real role of the access token.

## Motivation and Closing Thoughts

As a motivation, I shared the responses of the two companies' security teams, which confirmed the vulnerabilities. In addition, I want to share some of the tips I used to find these vulnerabilities, which should help security researchers interested in iOS. They will change their minds, because most security researchers consider iOS a monster, known for its high security mechanisms. However, it has some flaws that can be used to discover serious security issues in well-known applications.

I hope this presentation sheds light on the problem of authentication in cyber security and brings the question of whether the password is a good or bad authentication factor to the Moroccan cyber security community.

With love,

Image Copyright: <../images/description-of-my-presentation-at-owasp-appsec-africa-2017/7545094e-shutterstock_165303932.jpg>
