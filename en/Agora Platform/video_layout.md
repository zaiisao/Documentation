
---
title: video layout
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 12:47:14 GMT+0800 (CST)
---
# video layout
Video layout arranges the display of users when multiple users are mixed into one stream, such as in CDN live streaming or a composite recording. Video layout sets the relative size and position of each user on the canvas. It is sometimes referred to as **picture-in-picture** layout.

In the following image, the background of the video is the canvas, and each user occupies a region on the canvas.

![img](https://web-cdn.agora.io/docs-files/1577697787996)

Developers may have to set video layout when using Cloud Recording, On-premise Recording, or pushing streams to CDN.

- Recording: When using On-premise Recording or Cloud Recording to record in the composite recording mode, developers can choose from three predefined video layouts: floating, best fit, and vertical. Developers can also customize the video layout by setting the size and position of each user's region on the canvas.

- Push Streams to CDN: When multiple hosts are in a CDN live streaming channel, transcoding combines the streams of all hosts into a single stream. Use `TranscodingUser` to set the video layout for each user.

<div class="alert info">See also:<li><a href="https://docs.agora.io/en/cloud-recording/cloud_recording_layout">Set Video Layout</a>（Cloud Recording）</li><li><a href="https://docs.agora.io/en/Recording/recording_layout">Set Video Layout</a>（On-premise Recording）</li><li><a href="https://docs.agora.io/en/Audio%20Broadcast/cdn_streaming_apple">Push Streams to CDN</a></li></div>

<a href="../../en/Agora%20Platform/terms.md"><button>Back to glossary</button></a>
