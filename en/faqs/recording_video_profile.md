
---
title: How do I set the video profile of the recorded video?
description: 
platform: Linux
updatedAt: Wed Aug 14 2019 17:31:00 GMT+0800 (CST)
---
# How do I set the video profile of the recorded video?
In the individual recording mode, the recorded video keeps the original video profile.

In the composite recording mode, you can set the video profile (resolution, frame rate, and bitrate) by the `mixResolution` parameter. We recommend setting the video profile according to the table below.

> We recommend setting the recording resolution lower than the aggregate reoslution of the video streams, otherwise the recorded video might be blurry.
> Agora only supports the following frame rates: 1 fps, 7 fps, 10 fps, 15 fps, 24 fps, 30 fps, and 60 fps. The default value is 15 fps. If you set other frame rates, the SDK uses the default value.

| Resolution (width x height) | Frame rate (fps) | Base bitrate (Kbps, for Communication) | Live bitrate (Kbps, for Live Broadcast) |
| --------------------------- | ---------------- | -------------------------------------- | --------------------------------------- |
| 160 x 120                   | 15               | 65                                     | 130                                     |
| 120 x 120                   | 15               | 50                                     | 100                                     |
| 320 x 180                   | 15               | 140                                    | 280                                     |
| 180 x 180                   | 15               | 100                                    | 200                                     |
| 240 x 180                   | 15               | 120                                    | 240                                     |
| 320 x 240                   | 15               | 200                                    | 400                                     |
| 240 x 240                   | 15               | 140                                    | 280                                     |
| 424 x 240                   | 15               | 220                                    | 440                                     |
| 640 x 360                   | 15               | 400                                    | 800                                     |
| 360 x 360                   | 15               | 260                                    | 520                                     |
| 640 x 360                   | 30               | 600                                    | 1200                                    |
| 360 x 360                   | 30               | 400                                    | 800                                     |
| 480 x 360                   | 15               | 320                                    | 640                                     |
| 480 x 360                   | 30               | 490                                    | 980                                     |
| 640 x 480                   | 15               | 500                                    | 1000                                    |
| 480 x 480                   | 15               | 400                                    | 800                                     |
| 640 x 480                   | 30               | 750                                    | 1500                                    |
| 480 x 480                   | 30               | 600                                    | 1200                                    |
| 848 x 480                   | 15               | 610                                    | 1220                                    |
| 848 x 480                   | 30               | 930                                    | 1860                                    |
| 640 x 480                   | 10               | 400                                    | 800                                     |
| 1280 x 720                  | 15               | 1130                                   | 2260                                    |
| 1280 x 720                  | 30               | 1710                                   | 3420                                    |
| 960 x 720                   | 15               | 910                                    | 1820                                    |
| 960 x 720                   | 30               | 1380                                   | 2760                                    |
