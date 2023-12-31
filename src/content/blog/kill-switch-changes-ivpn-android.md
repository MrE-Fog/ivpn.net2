---
title: Kill switch changes in IVPN for Android
url: /blog/kill-switch-changes-ivpn-android/
highlighted: false
draft: false
authors:
  - Aleksandr Mykhailenko
categories:
  - Under the Hood
tags:
  - Apps
  - Security
date: 2021-10-14T10:15:00.000Z
thumbnailImage: /images-static/uploads/thumb-2x_andr.png
---
TL;DR - Before our latest Android update (2.7.0) customers had two different options for a kill switch: one implemented by IVPN, and another available through device settings in the Android OS. We have removed our custom solution from the IVPN app and suggest using the native Android solution from now on. 

A Kill switch is implemented to block all network traffic when the VPN connection is active and it fails/disconnects without the user explicitly stopping the connection. We added a custom kill switch to IVPN for Android in 2018 so customers who had no access to the native solution or preferred not to use it could have a different option.

We built our solution on an Android service called VPNService - the same that facilitates the VPN connection. The Android OS can only have one VPNService active at the same time; when one starts, the other stops. This means that the kill switch functionality can only become active after the VPN halts; otherwise, the kill switch would stop the connection.  

What are the ramifications?

1. Since the VPN and the kill switch service cannot run in parallel, there is a small gap between the old service stopping and the new one starting. This has the potential to cause traffic leaks.

2. Due to how Android OS treats service life cycles, there are no guarantees the kill switch service will be active without interruptions:  
   a. the OS can terminate it when it’s out of memory and for various other reasons - it is possible to restart the service, but traffic may leak before this happens  
   b. the app may crash for various other reasons  

These cases are unlikely, but they fail to deliver on the promises of a kill switch. Furthermore, most customers can now access the (better) native solution.

For these reasons we have removed the custom solution from the Android app, and we recommend using the native kill switch. You can find a quick guide for enabling it in the IVPN Android app. 
