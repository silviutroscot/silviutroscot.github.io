---
layout: post
title: "Introduction to video streaming"
description: "Introduction to adaptive streaming: transcoding, transmuxing, adaptive streaming"
comments: true
keywords: "blog, tech, why, question"
---

According to [Cisco projections](https://www.webmarketingpros.com/internet-video-to-account-for-80-of-global-traffic-by-2019/) 80% all web traffic in 2019 is video, which shows that video streaming is a very important and growing area. Besides the well known video platforms such as [Youtube](www.youtube.com) or [Netflix](www.netflix.com), large companies are building their own streaming platforms, and some examples are [Disney](https://preview.disneyplus.com/uk/) and [Apple](https://www.pocket-lint.com/tv/news/apple/133233-apple-tv-subscription-streaming-service-what-s-the-story-so-far). 
For someone without experience or technical background, video streaming looks like a trivial task and without many challenges; you record it and then stream it, right? Well..not quite right.
In this post I will try to explain how a video streaming platform works end to end and which are some of the most tricky and important problems in this realm, alongside some external resources for further reading.

## Context
There are two main types of video: **VoD** ([Video on Demand](https://en.wikipedia.org/wiki/Video_on_demand)) which is broadcasting of recorded video, delivered at any moment in time, when requested by the client, and **[Live video]()** which focuses on streaming live content to the users. 
The source of the video can be pristine videos, recorded with professional cameras and tools (e.g. Netflix, HboGo), or UGC - User Generated Content (e.g. Facebook, Instagram, Twitter, Youtube). 
From a very high level perspective the video is uploaded, for the UGC case, by a client device or inserted into the pipeline in the case of the pristine videos, transcoded, muxed, delivered to CDNs from where it is retrieved and played by the clients. 

## Server side 
### Flow
When the video is received on the server after it is processed clientside (not mandatory but recommended). Modern platforms, including Twitter and Facebook, have a resumable upload API, where the video is split into chunks and each chunk is uploaded independently.
### Transcoding

### Transmuxing

## Client side
### Upload
On the modern video streaming platforms, if the client has the functionality to upload videos, it usually demux and 
### Player
Depending on the platform, there are different video clients used. For iOS the Apple player framework, [AVFoundation](https://developer.apple.com/av-foundation/) is the most used, for Android there are more options but the most popular player is [Exoplayer](https://github.com/google/ExoPlayer) which is open sourced but it is created and maintained by Google, whilst for web there are multiple options, such as [video.js](https://videojs.com/) or [jPlayer](http://jplayer.org/). 


## Open problems
Probably the most difficult problem in video engineering is to assess how good a video looks for the human eye. The most 

## Further readings