
---
title: How do I set the video profile of the recorded video?
description: 
platform: Linux
updatedAt: Tue Apr 14 2020 10:42:52 GMT+0800 (CST)
---
# How do I set the video profile of the recorded video?
In individual recording mode, the recorded video keeps the original video profile.

In composite recording mode, you can set the video profile (resolution, frame rate, and bitrate) by the `mixResolution` parameter. We recommend setting the video profile according to the table below.

> - We recommend setting the recording resolution lower than the [aggregate resolution](https://docs.agora.io/en/faq/cloud_recording_billing?_ga=2.204176151.605592384.1576459020-1595988498.1574647397) of original video streams, otherwise the recorded video might be blurry.
> - Agora only supports the following frame rates: 1 fps, 7 fps, 10 fps, 15 fps, 24 fps, 30 fps, and 60 fps. The default value is 15 fps. If you set other frame rates, the SDK uses the default value.

## **Video Profile Table**

| Resolution (width x height) | Frame rate (fps) | Base bitrate (Kbps, for Communication) | Live bitrate (Kbps, for Live Broadcast) |
| --------------------------- | ---------------- | -------------------------------------- | --------------------------------------- |
| 160 &times; 120        | 15               | 65                                     | 130                                    |
| 120 &times; 120        | 15               | 50                                     | 100                                    |
| 320 &times; 180        | 15               | 140                                    | 280                                    |
| 180 &times; 180        | 15               | 100                                    | 200                                    |
| 240 &times; 180        | 15               | 120                                    | 240                                    |
| 320 &times; 240        | 15               | 200                                    | 400                                    |
| 240 &times; 240        | 15               | 140                                    | 280                                    |
| 424 &times; 240        | 15               | 220                                    | 440                                    |
| 640 &times; 360        | 15               | 400                                    | 800                                    |
| 360 &times; 360        | 15               | 260                                    | 520                                    |
| 640 &times; 360        | 30               | 600                                    | 1200                                   |
| 360 &times; 360        | 30               | 400                                    | 800                                    |
| 480 &times; 360        | 15               | 320                                    | 640                                    |
| 480 &times; 360        | 30               | 490                                    | 980                                    |
| 640 &times; 480        | 15               | 500                                    | 1000                                   |
| 480 &times; 480        | 15               | 400                                    | 800                                    |
| 640 &times; 480        | 30               | 750                                    | 1500                                   |
| 480 &times; 480        | 30               | 600                                    | 1200                                   |
| 848 &times; 480        | 15               | 610                                    | 1220                                   |
| 848 &times; 480        | 30               | 930                                    | 1860                                   |
| 640 &times; 480        | 10               | 400                                    | 800                                    |
| 1280 &times; 720       | 15               | 1130                                   | 2260                                  |
| 1280 &times; 720       | 30               | 1710                                   | 3420                                   |
| 960 &times; 720        | 15               | 910                                    | 1820                                   |
| 960 &times; 720        | 30               | 1380                                   | 2760                                   |
| 1920 &times; 1080      | 15               | 2080                                   | 4160                                   |
| 1920 &times; 1080      | 30               | 3150                                   | 6300                                   |
| 1920 &times; 1080      | 60               | 4780                                   | 6500                                   |
| 2560 &times; 1440      | 30               | 4850                                   | 6500                                   |
| 2560 &times; 1440      | 60               | 6500                                   | 6500                                   |
| 3840 &times; 2160      | 30               | 6500                                   | 6500                                   |
| 3840 &times; 2160      | 60               | 6500                                   | 6500                                   |

> The base bitrate in this table applies to the Communication profile. The Live-broadcast profile generally requires a higher bitrate for better video quality. You can also set the bitrate of the Live-broadcast profile as twice the base bitrate.
