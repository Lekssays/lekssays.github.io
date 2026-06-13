---
title: "In-browser End-to-End Encryption: A myth?"
date: "2021-12-18T10:44:08+00:00"
modified: "2021-12-18T13:52:37+00:00"
slug: "in-browser-end-to-end-encryption-a-myth"
author: "Ahmed Lekssays"
featured_image: "../images/in-browser-end-to-end-encryption-a-myth/e5ac6148-markus-winkler-ojsg0e_qcbo-unsplash.jpg"
categories: ["Computer Science"]
tags: ["Cyber Security", "encryption", "end-to-end", "privacy"]
original_url: "https://lekssays.wordpress.com/2021/12/18/in-browser-end-to-end-encryption-a-myth/"
excerpt: "End-to-End (E2EE, for short) is an emerging technology for privacy-preserving messaging and content sharing in mobile applications such as ProtonMail, WhatsApp, Telegram, Signal, etc. However, we start to see web applications that claim to have E2EE in the browser including ProtonMail, WhatsApp, Tel"
---
End-to-end encryption (E2EE, for short) is an emerging technology for privacy-preserving messaging and content sharing in mobile applications such as ProtonMail, WhatsApp, Telegram, and Signal. However, we are starting to see web applications that claim to offer E2EE in the browser, including ProtonMail, WhatsApp, and Telegram. In this blog post, we explore these claims in more detail.

## What is end-to-end encryption?

E2EE is a cryptographic procedure in which the shared content is encrypted on the sender's side (A) and decrypted on the receiver's side (B). It is a typical key-exchange use case, implemented with various cryptographic algorithms such as *Elliptic Curve Diffie-Hellman* (ECDH) in WhatsApp. As a result, the message cannot be decrypted during the transfer phase. Explaining how ECDH works is out of the scope of this blog post, but you can check [this paper](http://koclab.cs.ucsb.edu/teaching/ecc/project/2015Projects/Haakegaard+Lang.pdf) and [this demo](http://www-cs-students.stanford.edu/~tjw/jsbn/ecdh.html). It is worth noting that E2EE can work even over insecure channels (for example, using HTTP).

## End-to-end encryption in mobile applications

End-to-end encryption in mobile applications is used widely by any application that promotes privacy-preserving services. Indeed, if an application implements it, the service providers will not have access to the shared content. However, in any security model, we have to place trust somewhere. In this case, the user needs to trust the encryption implementation in the mobile application.

That trust is reasonable for several reasons:

- Most mobile applications (especially Android ones) can be reverse engineered, so the implementation can be inspected freely.
- Some applications, such as Signal and Telegram, share their source code as open source.

We can extend this trust further, to the underlying operating system and hardware. So, technically, the user has to trust the application, the operating system, and the hardware.

## End-to-end encryption in desktop applications

Desktop applications that offer end-to-end encryption are also an emerging trend. They behave similarly to mobile applications, since the encryption libraries are hosted on the device and not on the server. Many companies are therefore providing their services as cross-platform desktop applications, especially with the existence of cross-platform frameworks like Electron.

## What is wrong with in-browser end-to-end encryption?

Let us pick up where we left the discussion: trust. In the browser, each web application runs in a sandbox and connects to a server to fetch the resources it needs to do its job. By now, you might have figured out the problem: the user has to trust the server. In other words, the server provides the encryption implementation on the fly. So if the server is compromised (or the service provider wants to spy on users), an attacker can inject a malicious encryption library and then decrypt all the messages on the server. The critical problem here is that malicious libraries can be sent alongside legitimate content, which makes such an attack hard to detect.

Some applications put considerable effort into mitigating this issue, such as Mega. In their case, they use a browser extension that encapsulates all the encryption methods. However, even though this approach is more secure than trusting on-the-fly libraries, a lot of trust must still be placed in the extension itself.

This problem is not a new one. Trusting a server is a common practice in our everyday life. Simply by writing this post, I already trust many servers along the way. The real problem is when a company claims to offer E2EE for marketing purposes while knowing that, technically, it cannot be called E2EE, as is the case with ProtonMail. Nadim Kobeissi, in his paper "[An Analysis of the ProtonMail Cryptographic Architecture](https://eprint.iacr.org/2018/1121.pdf)," explained the underlying issues with ProtonMail's webmail (web application) and, specifically, their PGP usage.

To sum up, in-browser end-to-end encryption does not provide the same security guarantees as mobile applications. Many applications, such as Signal, have avoided offering an in-browser service for that specific reason. So the in-browser encryption dilemma continues, and yes, in-browser E2EE is still a myth.

Stay safe 🙂

## References

Kobeissi, Nadim. "An Analysis of the ProtonMail Cryptographic Architecture." IACR Cryptol. ePrint Arch. 2018 (2018): 1121.

Cover image by [Markus Winkler](https://unsplash.com/@markuswinkler) on [Unsplash.com](https://unsplash.com/photos/OjSG0E_qcbo)
