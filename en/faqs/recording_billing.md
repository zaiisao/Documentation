
---
title: Billing for the on-premise recording
description: 语音通话计费说明
platform: All Platforms
updatedAt: Mon Dec 16 2019 20:59:08 GMT+0800 (CST)
---
# Billing for the on-premise recording
## Calculating service minutes






Agora adds up the following two types of minutes used by the projects under your [Agora Account](https://console.agora.io/) on a monthly basis:

- [Recording video minutes](#rvmin)
- [Recording audio minutes](#ramin)
  




> 


### <a name="rvmin"></a>Recording video minutes 

The time that the recording server records video streams in the channel counts as the recording video minutes.

Agora adds up the resolutions of the video streams that the recording server records at a time, to get the aggregate resolution. The aggregate resolution can be classified into two brackets, and Agora charges you accordingly. 

| Video Bracket         | Aggregate Resolution |
| :-------------------- | :------------------- |
| High Definition (HD)  | ≤ 1280 x 720         |
| Super High Definition (HD+) | > 1280 x 720         |

The aggregate recording resolution varies with the resolution of the video streams being recorded in real time. Agora adds up the corresponding recording video minutes down to the accuracy of a few seconds.





**Calculate the aggregate recording resolution**

Suppose that the recording server has been in an RTC channel for 45 continuous minutes, recording the video streams of users A, B, and C for the first 30 minutes, and recording the video streams of A, B, and D for the subsequent 15 minutes. The following table shows the resolutions of A, B, C, and D during this period:

|                           | A's Resolution | B's Resolution | C's Resolution | D's Resolution | Aggregate Recording Resolution |
| ------------------------- | ------------ | :----------- | ------------ | ------------ | ------------------------------ |
| **Initial 30 minutes**      | 640 x 360    | 640 x 360    | 640 x 360    | N/A          | 691200 < 1280 x 720            |
| **Subsequent 15 minutes** | 640 x 360    | 240 x 180    | N/A          | 1280 x 720   | 1195200 > 1280 x 720           |

As you can see from the above table: 

- The aggregate recording resolution for the initial 30 minutes = A's resolution + B's resolution + C's resolution = 691200 < 1280 x 720, falling in the HD bracket. 
- The aggregate recording resolution for the subsequent 15 minutes = A's resolution + B's resolution + D's resolution = 1195200 > 1280 x 720, falling in the HD+ bracket.

Total recording fee = Unit price (recording video minutes HD) x 30 min + Unit price (recording video minutes HD+) x 15 min 

> See [Pricing](#billing) for the pricing information of the recording video minutes.



### <a name="ramin"></a>Recording audio minutes 

If you deduct the time that the recording server records the video streams in the channel from the total time that the server stays in that channel, you get the recording audio minutes, regardless of whether the server records any audio stream.

For example, let's say a recording server is in a channel for 30 minutes. It records a video stream for 20 minutes, and is idle for the rest 10 minutes. In this case, Agora records 20 recording video minutes and 10 recording audio minutes.

<div class="alert note"><li>The recording audio minutes do not add up, even if the recording server records multiple audio streams at the same time.</li><li>See <a href="#billing">Pricing</a> for the pricing information of the recording audio minutes.</li></div>



## Pricing







| Service<a name="billing"></a> | Pricing （Dollars/1,000 minutes) |
| :---------------------------- | :------------------------------- |
| Recording audio               | 0.99                             |
| Recording HD video                  | 3.99                             |
| Recording HD+ video                 | 14.99                            |







## Agora's policy of 10,000 free-of-charge minutes

See [Agora's policy of 10,000 free-of-charge minutes](https://docs.agora.io/en/faq/billing_free).
