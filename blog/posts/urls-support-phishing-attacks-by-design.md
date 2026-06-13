---
title: "URLs Support Phishing Attacks by Design"
date: "2020-04-15T02:58:20+00:00"
modified: "2020-04-15T02:58:20+00:00"
slug: "urls-support-phishing-attacks-by-design"
author: "Ahmed Lekssays"
featured_image: "../images/urls-support-phishing-attacks-by-design/e4f85bd9-4240dcff-66ab-4e5e-a0e3-79879b5fb80f-1520x800-1.png"
categories: ["Computer Science"]
tags: ["Cyber Security", "phishing", "privacy", "urls"]
original_url: "https://lekssays.wordpress.com/2020/04/15/urls-support-phishing-attacks-by-design/"
excerpt: "I was reading some articles and watching a video of @LiveOverflow where he was talking about how hard it is to understand URI and then he mentioned a presentation of Orange where he explained how libraries of different programming languages parse URIs and this helped Orange to find many SSRF securit"
---
## Introduction

I was recently reading some articles and watching a video by [@LiveOverflow](https://www.youtube.com/watch?v=0uejy9aCNbI), in which he discussed how hard URIs are to understand. He mentioned a presentation by [Orange](https://www.blackhat.com/docs/us-17/thursday/us-17-Tsai-A-New-Era-Of-SSRF-Exploiting-URL-Parser-In-Trending-Programming-Languages.pdf) that explained how libraries in different programming languages parse URIs, which helped Orange find many SSRF security issues in large companies. I thought it was necessary to clarify some misleading "facts," such as the belief that websites using HTTPS are trustworthy, or that a website is trustworthy simply because it starts with a known host like Facebook or YouTube. In this article, I will show you that this is not always true.

## What Is a URL?

Our digital life is full of URIs, which serve as a unique way to identify resources. A URI (Uniform Resource Identifier) is a string that identifies a resource. This string follows a specific, well-defined generic URI syntax, along with a process for resolving URI references. This generic syntax is designed with the security considerations of using URIs on the internet in mind.

It is worth mentioning that URLs, or Uniform Resource Locators, are a subset of URIs. The main difference between the two is that URLs always have a protocol at the beginning. In this blog, we will focus on URLs because we want to show how they can be misleading by design. All URLs follow a specific structure, described as:

```
Example: foo://user:pass@example.com:8042/over/there?name=ferret&q=2#nose
```

- foo: protocol (http, https, ftp, etc…)
- user:pass@example.com:8042: authority
  - user:pass: user information for logging in to a protected resource (optional)
  - example.com: host
  - 8042: port
- over/there: path
- ?name=ferret&q=2: queries (appended with the & symbol) (optional)
- #nose: fragment (optional)

As we can see, some parts are optional, such as the user information for logging in to a protected resource, the queries, and the fragment. The grammar syntax is well defined in RFC 3986, but parsing URLs can be very tricky, both for normal users and for library developers.

## How URLs Can Mislead Users

As a normal user, if you encountered the following URL:

```
https://www.youtube.com:0@bit.ly/2XxM1Hf
```

you would think it was legitimate, since it uses HTTPS and contains <http://www.youtube.com>. In fact, it is not legitimate, because it will redirect you to bit.ly/2XxM1Hf (which is my website). If we apply the rules we defined earlier, we get:

- https: protocol
- <http://www.youtube.com:0>: user and password (you can do this without specifying a password)
- bit.ly: host
- 2XxM1Hf: path

## Ambiguities in Parsing URLs

The other point I wanted to discuss is the ambiguity in parsing URLs. Orange used a simple example to show how different parsing libraries within the same programming language can produce different outputs for the same URL.

```
http://1.1.1.1 &@2.2.2.2# @3.3.3.3/
```

- urllib2 httpliv: 1.1.1.1
- requests: 2.2.2.2
- urllib: 3.3.3.3

I will explain in another article why this difference in parsing can have very dangerous security implications. A good resource is Orange's presentation, listed in the references.

## Conclusion

To sum up, do not trust URLs, and always check where the "@" is, because it might be a clue that saves you from a critical cyber attack.

## References

- RFC 3986: Generic Syntax of URIs <https://tools.ietf.org/html/rfc3986>
- A New Era of SSRF: Exploiting URL Parser in Trending Programming Languages!
  <https://www.blackhat.com/docs/us-17/thursday/us-17-Tsai-A-New-Era-Of-SSRF-Exploiting-URL-Parser-In-Trending-Programming-Languages.pdf>
- Image: [https://cdn.searchenginejournal.com/wp-content/uploads/2019/07/4240dcff-66ab-4e5e-a0e3-79879b5fb80f-1520×800.png](../images/urls-support-phishing-attacks-by-design/4c1fec74-4240dcff-66ab-4e5e-a0e3-79879b5fb80f-1520x800.png)
