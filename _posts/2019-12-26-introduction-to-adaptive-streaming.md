---
layout: post
title: "Introduction to video streaming"
description: "Introduction to adaptive streaming: transcoding, transmuxing, adaptive streaming"
comments: true
keywords: "blog, tech, why, question"
---
## Why video engineering?

According to [Cisco projections](https://www.webmarketingpros.com/internet-video-to-account-for-80-of-global-traffic-by-2019/), 80% all web traffic in 2019 is video, which shows that video streaming is a very important and growing area. Besides the well-known video platforms such as [Youtube](www.youtube.com) or [Netflix](www.netflix.com), large companies are building their streaming platforms, and some examples are [Disney](https://preview.disneyplus.com/uk/) and [Apple](https://www.pocket-lint.com/tv/news/apple/133233-apple-tv-subscription-streaming-service-what-s-the-story-so-far).
For someone without experience or technical background, video streaming looks like a trivial task and without many challenges; you record the content and then stream it, right? Well..not really.
In this post, I will try to explain from a high-level perspective how an end to end video streaming platform works and which are some of the most tricky and important problems in this realm, alongside some external resources for further reading.

## Introduction
There are two main types of video: **VoD** ([Video on Demand](https://en.wikipedia.org/wiki/Video_on_demand)) which is broadcasting of recorded video, delivered at any moment in time when requested by the client, and **[Live video](https://en.wikipedia.org/wiki/Live_streaming)** which focuses on streaming live content to the users.
The source of the video can be pristine videos, recorded with professional cameras and tools (e.g. Netflix, Hulu), or UGC - User Generated Content (e.g. Facebook, Instagram, Twitter, Youtube). For the pristine videos, the challenges are different as the videos don't have artefacts and noise dues to the lack of quality of the recording tools, but usually, in this case, the content is longer and it has multiple subtitles and audio languages.
From a high-level perspective, the video is uploaded, for the UGC case, by a client device or inserted into the pipeline in the case of the pristine videos, transcoded, muxed, delivered to CDNs from where it is retrieved and played by the clients.

## Adaptive streaming
Is a streaming technique where the video is encoded at multiple bitrates. Thereafter, the video is split into chunks of equal size (e.g. Netflix has 15s chunks) and the client chooses which bitrate it can play, for each chunk. The player has an algorithm which predicts the current bandwidth and from all the available bitrates it selects the largest bitrate (the best quality) it can play, which is usually the largest bitrate smaller than the bandwidth estimation, but that depends on the buffer size, streaming algorithm, etc.

## Server-side 
### Flow
When the video is received on the server after it is processed clientside (not mandatory but recommended). Modern platforms, including Twitter and Facebook, have a resumable upload API, where the video is split into chunks and each chunk is uploaded independently.
In the case of pristine videos, they are directly uploaded by the creator on the platform, which is easier as the users are internal engineers.
### [Transcoding](https://en.wikipedia.org/wiki/Transcoding)
Transcoding is generally a lossy process and it involves the conversion of a video from one encoding to another. A video codec(encoding) is a piece of software that compresses or decompresses digital video. Some codec examples are [H264](https://en.wikipedia.org/wiki/Advanced_Video_Coding), [H265(HEVC)](https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding), [AV1](https://en.wikipedia.org/wiki/AV1). Depending on the content and of the codec profile level, which defines the compression/decompression tools allowed by the codec, the codec can achieve different compression rates. As the codec level increases, the time to compress the video increases and the decoding is more expensive both in terms of time and of computational complexity.

### [Transmuxing](https://blog.stackpath.com/transmuxing/)
The video stream alongside the audio stream and possibly subtitles are packaged together in a "container". This container has details about syncing, codec used, etc. Some examples of containers are MP4, MOV, MPEG-TS.

### Encryption
Depending on the content type and platform/company, the video stream can be encrypted. No one can view the encrypted videos without firstly decrypting them. One of the most common video encryptions is AES 128 - Advanced Encryption Standard, using a block size of 128 bits. AES is a symmetric key algorithm, i.e. the encryption key is also the decryption key.
DRM - Digital Rights Management encryption involves a Content Decryption Module which is proprietary and it is part of the device or the browser. In this way, the key is not directly exposed to the user.

## Client-side
### Upload
On the modern video streaming platforms, if the client has the functionality to upload videos, it usually demux and decompress the video and then it will re-encode and remux the video. Thereafter, with this newly encoded video, the upload API is called. Usually, the API is resumable so the video is split by the client into chunks and each chunk is uploaded separately and they are ordered by the server.
### Player
Depending on the platform, there are different video clients used. For iOS the Apple player framework, [AVFoundation](https://developer.apple.com/av-foundation/) is the most used, for Android there are more options but the most popular player is [Exoplayer](https://github.com/google/ExoPlayer) which is open-sourced but it is created and maintained by Google, whilst for web, there are multiple options, such as [video.js](https://videojs.com/) or [jPlayer](http://jplayer.org/). The player should be able to estimate the bandwidth and request the most suitable segment based on that, manage the buffer, decode (usually using hardware decoders) and play the video but also to record metrics from that session, if needed.


## Open problems
Probably the most difficult problem in video engineering is to assess how good a video looks for the human eye. This is called <u>subjective quality analysis</u> and there are many tools to do that, but none of them is very accurate. Some of them, which use signal processing approaches, are [PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio), [SSIM](https://en.wikipedia.org/wiki/Structural_similarity). Lately, Netflix developed a tool which is considered to be the best tool available, called [VMAF](https://medium.com/netflix-techblog/vmaf-the-journey-continues-44b51ee9ed12) which uses Machine Learning alongside traditional analysis techniques.
All the tools mentioned above are <u>reference based measurements</u>. These work very well especially for pristine videos, as we want to keep our encoded versions as close as possible to the original. Besides that, there are new approaches in assessing the quality without any reference. Some interesting work can be found [here](https://arxiv.org/pdf/1810.08169v1.pdf) and [here](https://arxiv.org/pdf/1908.11517.pdf).
Another interesting problem is <u>bandwidth estimation</u>. This involves measuring the bandwidth from the last seconds and based on that to estimate the bandwidth for the next segment. The quality of estimation is very difficult to be assessed as we don't know the bandwidth, but estimations can be done with network throttling with given limits.

## Further readings and talks
- [AV1 and UGC transcoding](http://watch.bigapple.video/)
- [Low Latency Streaming](https://medium.com/@periscopecode/introducing-lhls-media-streaming-eb6212948bef)
- [Subjective quality analysis for mobile devices](https://www.youtube.com/watch?v=vHVMr4jH8rI)
- [A/B testing when CDN misses influences the results](https://www.youtube.com/watch?v=k0F8gtEFGDE)