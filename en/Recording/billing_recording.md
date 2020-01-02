
---
title: Billing for On-premise Recording
description: 
platform: All Platforms
updatedAt: Thu Jan 02 2020 08:02:22 GMT+0800 (CST)
---
# Billing for On-premise Recording
## Overview


Each month, Agora charges you by the services you use. This article describes how Agora calculates the services you use and provides the billing information of the On-premise Recording SDK. If you have used other products or add-ons, you can also see the billing information below:



- [Billing for Voice Call](https://docs.agora.io/en/Voice/billing_audio?platform=All%20Platforms)
- [Billing for Video Call](https://docs.agora.io/en/Video/billing_video?platform=All%20Platforms)
- [Billing for Video Broadcasting](https://docs.agora.io/en/Interactive%20Broadcast/billing_interactive_broadcast?platform=All%20Platforms)
- [Billing for Audio Broadcasting](https://docs.agora.io/en/Audio%20Broadcast/billing_audio_broadcast?platform=All%20Platforms) 
- [Billing for Cloud Recording](https://docs.agora.io/en/cloud-recording/billing_cloud_recording?platform=All%20Platforms)



For more information about billing, fee deduction, and account suspension, see [Billing, Fee Deduction, and Account Suspension](https://docs.agora.io/en/faq/billing_account).

## Calculate service minutes






Agora adds up the following two minutes used by the projects under your [Agora Account](https://console.agora.io/) on a monthly basis:

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

The aggregate recording resolution varies with the resolution of the video streams being recorded in real time. Agora adds up the corresponding recording video minutes with an accuracy of seconds.





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

If you deduct the time that the recording server records the video streams in the channel from the total time that the server stays in that channel, you get the recording audio minutes, regardless of whether the server records any audio stream.

<div class="alert note"><li>The recording audio minutes do not add up, even if the recording server subscribes to multiple audio streams.</li><li>See <a href="#billing">Pricing</a> for the pricing information of the recording audio minutes.</li></div>



## Pricing







| Service<a name="billing"></a> | Pricing （Dollars/1,000 minutes) |
| :---------------------------- | :------------------------------- |
| Recording audio               | 0.99                             |
| Recording HD video                  | 3.99                             |
| Recording HD+ video                 | 14.99                            |







## Q&A







<details>
	<summary><font color="#3ab7f8">Question: Does Agora charge differently if I use a different recording mode?</font></summary>

Your recording fee has nothing to do with the recording mode you choose. Regardless of whehter you use the individual mode or composite mode, your recording fee relates only to the number of the streams recorded, the recording time, and the aggregate recording resolutions. 

</details>




<details>
	<summary><font color="#3ab7f8">Question: Does Agora charge the screen capture function separately?</font></summary>

We do not charge the screen capture function separately. Screen capture is but a different form of the recording function. The recording service has to join the corresponding channel and subscribe to the specified video streams to capture screen at the specified interval. That said, even though you have not recorded the full-time video, we still charge you the full-time recording of the specified video streams.

</details>


## Agora's policy of 10,000 free-of-charge minutes

See [Agora's policy of 10,000 free-of-charge minutes](https://docs.agora.io/en/faq/billing_free).
