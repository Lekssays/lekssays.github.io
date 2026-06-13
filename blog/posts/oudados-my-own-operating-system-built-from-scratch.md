---
title: "oudadOS.. My own Operating System Built from Scratch"
date: "2017-03-09T13:37:21+00:00"
modified: "2017-04-28T16:38:15+00:00"
slug: "oudados-my-own-operating-system-built-from-scratch"
author: "Ahmed Lekssays"
featured_image: "../images/oudados-my-own-operating-system-built-from-scratch/e50898f8-oudados.png"
categories: ["Computer Science"]
tags: ["open source", "operating systems", "oudadOS"]
original_url: "https://lekssays.wordpress.com/2017/03/09/oudados-my-own-operating-system-built-from-scratch/"
excerpt: "I am happy to write these lines. It took me a lot of time to understand how operating systems behave. I have been dreaming to make my own operating system from scratch or at least based on Linux kernel. And today I am greatly happy to announce that I finish coding my operating system called [&hellip"
---
## Announcing oudadOS

I am happy to write these lines. It took me a long time to understand how operating systems behave. I have long dreamed of making my own operating system from scratch, or at least one based on the Linux kernel. Today I am thrilled to announce that I have finished coding my operating system, “*[oudadOS](https://github.com/Lekssays/oudadOS)*,” for educational purposes.

My aim is for it to be used as a learning tool in operating systems classes around the world. I tried to illustrate concepts discussed in operating systems books so that it would fit this educational purpose. It is built with C++ as much as possible, alongside Assembly, in an object-oriented manner, and it is meant to run on the Intel 8086 architecture. The name *oudad* comes from the Amazigh language and means red deer in English.

You can check it out here: <https://github.com/Lekssays/oudadOS>. I will be posting articles about each part of the operating system on my blog. I also made an official website for *oudadOS*, which you can access here: <http://oudados.lekssays.com>. I will write documentation for it in the coming weeks, and it will be posted here: <https://lekssays.github.io/oudadOS/>.

## A Learning Experience

You may notice that some parts are not well done or are poorly designed. I tried my best to make it look better, and I believe it reflects my level, because I put a huge effort into it. I faced many problems and stopped writing it for weeks because I could not figure out how to implement some parts, especially interrupts, multitasking, and the GUI.

So please, I invite you to raise an issue on the GitHub repository or make a pull request if you see that some parts should be changed or improved. As I mentioned in the README file, this is a learning experience for me. I am proud that I built it, but that does not mean it is perfect or good. It reflects my understanding of the topics I have implemented. For future improvements, I will try to implement what I mentioned in the README file in the GitHub repository.

## How I Got Here

I posted some announcements about building a Moroccan penetration testing operating system, but I realized that our community was not contributing. Some friends contacted me, which really made me happy because they were ready to help. However, I learned that I was not experienced enough to lead such projects.

I started looking for books and articles about building an operating system, and I found that I should first learn about computer architectures. This was a big step on my path to understanding how operating systems work. I took computer architectures as a course at Al Akhawayn University in Ifrane, and it helped me a lot to achieve my goal. After that, I started reading about the design of the Linux kernel. Then I learned about cross compilers, makefiles, bootloaders, and many other topics.

## Building an OS Is Hard

I agree that building an operating system is not an easy task at all, because it requires a good background in everything I have discussed above, in addition to passion. You cannot build an operating system if you do not master the programming language of your choice.

Moreover, there are no clear or complete tutorials about how to build an operating system, at least none that I have seen so far in OS tutorials or articles. You cannot copy and paste code, because such topics are not usually discussed on StackOverflow. Another big issue is that even if you find an operating system that is already implemented somewhere, it is hard to understand its logic, because each programmer sees things differently.

The bible of building an operating system is <http://www.osdev.org>, but it provides an abstract understanding of the concepts and sometimes only a small implementation of a feature. You should know that the OS developer community can be tough: they will not help you if they find that you are copying someone else's code, or if they see that you do not even understand what you are doing. However, they are very helpful when it comes to complex problems, and I experienced this when I was implementing interrupts. So please, if you want to build your own operating system, make sure that you understand the theory and the programming language of your choice well.

## On Believing

Operating systems design teaches you how to believe. You will notice that a lot of hexadecimal codes are already defined by manufacturers. For instance, the boot magic code *0x1badb002*, port connections for different devices, PCI device IDs, PCI vendor IDs, and so on. They tend to push you to simply believe in them.

## Acknowledgments

At this specific moment of my life, I have to acknowledge some people who helped me a lot in achieving this, whether by supporting me, giving me feedback, or helping me with design decisions:

- I would like to thank *Viktor Engelmann* for his YouTube series “Make your own OS.” He gave me insights about how to build an operating system, even if some parts are not well explained because some concepts are hard. I followed his design for major parts of *oudadOS*.
- I would like to thank the osdev community for the awesome wiki.
- I would like to thank *Saad Taame* for helping me with some design decisions and for giving me feedback when I got stuck implementing some parts.
- I would like to thank *Dr. Hamid Harroud* and *Abdelhamid Limami* for supporting me to finish this work.
- I would also like to thank *Abdelghafour Mourchid* for taking care of the graphical side of the operating system and for the awesome logo.

## Looking Forward

I hope that the Moroccan community will one day contribute to building a different operating system for penetration testing, because it is a great feeling to see that you have built an operating system. It is like your child: you really love it. Maybe some of you will say that there are plenty of penetration testing operating systems already. I would argue that if other developers had followed that reasoning, we would not see BugTraq, ParrotOS, BackBox, DEFT, BlackArch, and many others. I strongly believe that we have the ability to make it, and that we just need to believe in it.

> *oudadOS* is made with ![148836](../images/oudados-my-own-operating-system-built-from-scratch/e0dabafc-148836.png) in Morocco, and it highly contributes to my ultimate goal, which is sharing code, love, and knowledge. I hope that oudadOS will be an added value to the community. I wish it could motivate some of you to contribute to an OS, give you a better understanding of operating systems, or at least give you an overall idea of how operating systems are built.
