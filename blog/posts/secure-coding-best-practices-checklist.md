---
title: "Secure Coding Best Practices Checklist"
date: "2018-07-30T00:11:53+00:00"
modified: "2018-07-30T00:11:53+00:00"
slug: "secure-coding-best-practices-checklist"
author: "Ahmed Lekssays"
featured_image: "../images/secure-coding-best-practices-checklist/81ff31fc-softwarebug.jpg"
categories: ["Computer Science"]
tags: ["cryptography", "Cyber Security", "programming"]
original_url: "https://lekssays.wordpress.com/2018/07/30/secure-coding-best-practices-checklist/"
excerpt: "I have been dealing with programmers and programs for a long time as a penetration tester and as a developer. I have been conducting many audits of web and mobile applications with different architectures. I made this checklist for developers to ensure the strict minimum of a &#8220;secure&#8221; co"
---
## Introduction

I have worked with programmers and programs for a long time, both as a penetration tester and as a developer. Over the years, I have conducted many audits of web and mobile applications built on different architectures. I put together this checklist to help developers ensure the bare minimum of "secure" code. I would like to share the following guidelines with you.

## The Checklist

1. Determine the context and intended usage of the software product, along with its operating environment, and specify the required security requirements.
2. Make the vulnerabilities and potential exposures associated with the chosen programming languages and operating systems available to programmers, developers, reviewers, and test teams before the architectural design phase.
3. Set up security parameters for access to services such as FTP. For example, where anonymous FTP is allowed, grant write-only access (no read or list) to the incoming directory and read-only access to the outgoing directory.
4. Check for architecture-specific vulnerabilities and review how data flows through the code.
5. Check for implementation-specific vulnerabilities such as race conditions, randomness problems, and buffer overflows.
6. Do NOT allow programmer backdoors or unauthorized access paths that bypass security mechanisms.
7. Avoid storing secrets such as passwords, private keys, or tokens in the code, and avoid using weak encryption schemes.
8. Identify all points in the source code where the program takes input from another program or an untrusted source.
9. Check API (Application Programming Interface) calls to security modules or interfaces.
10. Investigate secure connections. Verify that they are actually secure and that they connect, as intended, only to the systems they are supposed to reach.
11. Investigate the software's built-in extensibility features.
12. Investigate the security of data as it is passed from application servers to databases.
13. Avoid default or otherwise improper configurations that may open the door to attackers.
14. Default to the "highest security" needed, and require validation and approval for any deviations.
15. Perform security testing for both unit and system integration.
16. Keep yourself updated on the latest vulnerabilities by consulting security news and exploit databases such as [https://thehackernews.com](https://thehackernews.com/) (news) and [https://www.exploit-db.com](https://www.exploit-db.com/) (Exploits Database).
17. Avoid carrying development-phase configurations into production. For example, in Firebase, access to databases without authentication is always allowed during the development phase unless you disable it.

Happy coding! Oh, sorry: happy *secure* coding!

Image Copyright: <../images/secure-coding-best-practices-checklist/5d6a1b58-softwarebug.jpg>
