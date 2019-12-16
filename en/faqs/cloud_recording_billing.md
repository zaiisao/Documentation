
---
title: Billing for the voice call
description: 语音通话计费说明
platform: All Platforms
updatedAt: Mon Dec 16 2019 20:50:54 GMT+0800 (CST)
---
# Billing for the voice call
## Calculating service minutes






Agora will add up the following two minutes used by the project corresponding to your [App ID](https://console.agora.io/) on a monthly basis:

- [Recording video minutes](#rvmin)
- [Recording audio minutes](#ramin)
  




> 


### <a name="rvmin"></a>Recording video minutes 

The time that the recording server records video stream(s) in the channel counts as the recording video minutes.

Agora adds up the resolutions of the video streams that the recording server records at a time, to get the aggregate resolution. The aggregate resolution can be classified into two brackets, and Agora will charge you accordingly. 

| Video Bracket         | Aggregate Resolution |
| :-------------------- | :------------------- |
| High Definition (HD)  | ≤ 1280 x 720         |
| Super High Definition | > 1280 x 720         |

The aggregate recording resolution varies with the resolution of the video stream(s) being recorded in real time. Agora adds up the corresponding recording video minutes with an accuracy of seconds.





**Calculate the aggregate recording resolution**

Suppose that the recording server has been in an RTC channel for 45 straight minutes, recording the video streams of user A, B, and C for the first 30 minutes, and recording the video streams of A, B, and D for the subsequent 15 minutes. And the following table shows the resolutions of A, B, C, and D during this period:

|                           | Resolution A | Resolution B | Resolution C | Resolution D | Aggregate Recording Resolution |
| ------------------------- | ------------ | :----------- | ------------ | ------------ | ------------------------------ |
| **First 30 minutes**      | 640 x 360    | 640 x 360    | 640 x 360    | N/A          | 691200 < 1280 x 720            |
| **Subsequent 15 minutes** | 640 x 360    | 240 x 180    | N/A          | 1280 x 720   | 1195200 > 1280 x 720           |

As you can see from the above table: 

- The aggregate recording resolution for the first 30 minutes = Area A + Area B + Area C = 691200 < 1280 x 720, falling in the HD bracket. 
- The aggregate recording resolution for the subsequent 15 minutes = Area A + Area B + Area C = 1195200 > 1280 x 720, falling in the HD+ bracket.

Total recording fee = Unit price (recording video minutes HD) x 30 min + Unit price (recording video minutes HD+) x 15 min 

> See [Pricing](#billing) for the pricing information of the recording video minutes.



### <a name="ramin"></a>Recording audio minutes 

If you deduct the time that the recording server records the video stream(s) in the channel from the total time that the server stays in that channel, you get the recording audio minutes, regardless of whether the server records any audio stream.

<div class="alert note"><li>The recording audio minute will not add up, if the recording server subscribes to multiple audio streams.</li><li>See <a href="#billing">Pricing</a> for the pricing information of the recording audio minutes.</li></div>



## Pricing









| Service<a name="billing"></a> | Pricing （Dollars/1,000 minutes) |
| :---------------------------- | :------------------------------- |
| Recording audio               | 1.49                             |
| Recording HD                  | 5.99                             |
| Recording HD+                 | 22.49                            |





## Agora's policy of 10,000 free-of-charge minutes

See [Agora's policy of 10,000 free-of-charge minutes](https://docs.agora.io/en/faq/billing_free).
