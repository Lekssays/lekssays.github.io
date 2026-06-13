---
title: "Analysis of Wiqaytna Application for COVID-19 Tracking"
date: "2020-06-01T20:16:40+00:00"
modified: "2020-06-03T03:44:02+00:00"
slug: "analysis-of-wiqaytna-application-for-covid-19-tracking"
author: "Ahmed Lekssays"
featured_image: "../images/analysis-of-wiqaytna-application-for-covid-19-tracking/aba2fca7-logo.png"
categories: ["Computer Science"]
tags: ["covid-19", "Cyber Security", "privacy"]
original_url: "https://lekssays.wordpress.com/2020/06/01/analysis-of-wiqaytna-application-for-covid-19-tracking/"
excerpt: "I spent some time today analyzing Wiqaytna application (Android Version) published by the Moroccan ministry of interior and I have some comments on the ongoing debate. So, regarding the process, first, I have downloaded the source code published on GitHub (https://github.com/Wiqaytna-app/wiqaytna_an"
---
I spent some time today analyzing the [Wiqaytna](https://www.wiqaytna.ma) application (Android version) published by the Moroccan Ministry of Interior, and I have some comments on the ongoing debate.

## How I Analyzed the Application

My process had three steps:

1. **Comparing the source code.** I downloaded the source code published on GitHub, and also downloaded the APK of the version published on the Play Store, then reverse engineered it to recover the source of the actually published application. This let me compare both sources, since developers could publish the source of a different application on GitHub than the one they ship.

   The published source is on GitHub: [https://github.com/Wiqaytna-app/wiqaytna\_android/](https://github.com/Wiqaytna-app/wiqaytna_android/?fbclid=IwAR0s8eMAc7gfFBdJ0BYEmZDcirPPtes7BaX8OxSqEAciUMTzO1guF8gsZGw).

2. **Reviewing the permissions.** I analyzed the permissions in both source trees, and they are identical. The permissions seemed normal and expected: mainly Bluetooth LE, internet access, and a few permissions needed to keep the app running in the background. However, one suspicious permission, "android.permission.ACCESS\_FINE\_LOCATION," needed further investigation. The authors wrote in the AndroidManifest.xml file that "the ACCESS\_FINE\_LOCATION doesn't give any access to the user's GPS location," and I will comment on this at the end of this post.

3. **Inspecting network traffic.** I checked the requests the app makes to the outside world and found that it only makes normal requests to some plotting services and to a Firebase database.

## Comments on the Privacy and Security Debate

The application IS NOT SPYWARE. People describing it as "the government will access our messages, photos, etc." are making WRONG claims. The application does not do or ask for anything of the kind.

I did have one concern, prompted by videos and articles from friends claiming that the app does not ask for LOCATION. In fact, the application does ask for LOCATION (and is ethically correct to do so). This is necessary for Bluetooth Low Energy to operate in the background. The application can access your location at any moment, but it was not used to track you; it was needed for the other features to work ([https://stackoverflow.com/…/bluetooth-le-scan-doesnt-work-o…](https://l.facebook.com/l.php?u=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F32708374%2Fbluetooth-le-scan-doesnt-work-on-android-m-in-the-background%3Ffbclid%3DIwAR1aZLv-JSxYN5H4I2X6ZqbAX0Dx_J8lsWMB-Vy3lJbY-S-xHbf8EAu6qdE&h=AT3KESylucNhyPcPXw1V5-ETqs8ZC3SEzqqQN5a_JZaVXGlFtoRwJM1hXBfzU56kV3suh6RBQB5H9EJPdnf-eM5ga36h97ftX7PfPsqzPP9lbenL47Zps15EvHNRsT83rbCqbFcM0ov0m4hcQBqUz5HUqUMI-gJ7yJmXUHw)).

It is also worth mentioning that the application heavily consumes battery, since it always runs in the background and uses many sensors concurrently and continuously. On iOS, it always runs in the foreground, which leads to battery problems. Another point worth adding is that iOS does not grant permission to read location data in background mode. I would also note that the Moroccan version of the application is based directly on the Singaporean version ([https://github.com/opentrace-community/opentrace-android/](https://github.com/opentrace-community/opentrace-android/?fbclid=IwAR1M-X84eMM8sAzGoDquPoY_TPluAJ1Gs1Q0YJ3RIoV2IvQ7Yy0-tnhvo0g)).

## On Bluetooth Low Energy

Some friends raised a point about Bluetooth Low Energy (BLE). The first step in BLE is connection, where a device broadcasts advertisement packets while other devices actively scan. Next is the pairing process, which follows many insecure protocols (this would really require dedicated research on this application). Then comes the communication part. This communication follows a structure called the GATT hierarchy: there are multiple services, and within each service there are multiple characteristics (data gathered from the sensors, in this case location, etc.), exchanged in an encrypted way. Each service and each characteristic has a UUID. So, regarding what was mentioned, the UUID can correspond to any characteristic that this application shares. There is a good research paper on this topic if you want to dig deeper: [https://dl.acm.org/doi/pdf/10.1145/3319535.3354240](https://l.facebook.com/l.php?u=https%3A%2F%2Fdl.acm.org%2Fdoi%2Fpdf%2F10.1145%2F3319535.3354240%3Ffbclid%3DIwAR2ny08Qs4QIgVeIThmTqP599Wq2NpECtzt9ySrLeID02rRuP60y5LRWC8A&h=AT3wjP9hl25_v5pn_Rue8sy4FY8W6FrqZKGMpmGvAOdiKct4SHZW4cdNN1NHaeAfUIOviCLR4aelDTpWEMxw7QE9aYN5crstT0tLSu3a_W390Eqy7nLahF3qduseYLkvwPY1YGmPKQ&__tn__=-UK-R&c[0]=AT1Ph7YvG0hknWSRbPJeSg-Fv-JUMBgNJ9dV2X12C9i9ThuHHUM2l49Vhc-5wURWVnsoPODBWuxbYTdknyrt5E9SAsLa7QxkE0sFtJOx-E7DzWE_MZRpxKUfh4U4zkheQdeSgdfX3wsazN9cKLM-).

As I mentioned, proper research would be needed to determine whether the protocols used for data exchange in this process are secure, because many applications use a protocol called "Just Works" that is insecure by design.
