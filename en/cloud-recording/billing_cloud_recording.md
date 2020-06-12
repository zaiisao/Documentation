
---
title: Billing for Cloud Recording
description: 
platform: All Platforms
updatedAt: Thu Jan 02 2020 07:50:01 GMT+0800 (CST)
---
# Billing for Cloud Recording
## Overview


At the beginning of each calendar month, Agora charges you for services used. This article describes how Agora calculates the billing for the services used of the Cloud Recording SDK. You can see additional billing information pertaining to other Agora products and add-ons below:



- [Billing for Voice Call](https://docs.agora.io/en/Voice/billing_audio?platform=All%20Platforms)
- [Billing for Video Call](https://docs.agora.io/en/Video/billing_video?platform=All%20Platforms)
- [Billing for Video Broadcasting](https://docs.agora.io/en/Interactive%20Broadcast/billing_interactive_broadcast?platform=All%20Platforms)
- [Billing for Audio Broadcasting](https://docs.agora.io/en/Audio%20Broadcast/billing_audio_broadcast?platform=All%20Platforms) 
- [Billing for On-premise Recording](https://docs.agora.io/en/Recording/billing_recording?platform=All%20Platforms)



For more information about billing, fee deduction, and account suspension, see [Billing, Fee Deduction, and Account Suspension](https://docs.agora.io/en/faq/billing_account).

## Agora's policy of 10,000 free-of-charge minutes

Agora gives each [Agora Account](https://console.agora.io/) 10,000 charge-free minutes each month, and deducts the minutes in the following sequence: 

1. Audio minutes
2. On-premise recording audio minutes
3. Cloud recording audio minutes 
4. HD video minutes
5. On-premise recording HD video minutes
6. Cloud recording HD video minutes
7. HD+ video minutes
8. On-premise recording HD+ video minutes
9. Cloud recording HD+ video minutes

If your total service minutes do not exceed 10,000 minutes, the service is free of charge. After the 10,000 charge-free minutes are fully deducted, Agora charges you for the additional service minutes.

<div class="alert note">The remaining free-of-charge minutes will be cleared at the end of each calendar month.</div>

## Calculate service minutes






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
| Recording audio               | 1.49                             |
| Recording HD video                 | 5.99                             |
| Recording HD+ video                | 22.49                            |





## Considerations





### Video resolution in the dual-stream scenario

When the user being recorded enables [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode), the recording service can receive only one stream at a time:

- If the recording server records the high-quality video stream, the aggregate video resolution is calculated based on the resolution of the high-quality video.
- If the recording server records the low-quality video stream, the aggregate video resolution is calculated based on the resolution of the video received by the server.

### Resolution calibration

When calculating the aggregate resolution, Agora counts the resolution of 225,280 (640 × 352) as 640 × 360.





## Q&A







<details>
	<summary><font color="#3ab7f8">Question: How does Agora charge if I use different recording modes?</font></summary>

Your recording fee has nothing to do with the recording mode you choose. Regardless of whether you use the individual mode or composite mode, your recording fee relates only to the number of the streams recorded, the recording time, and the aggregate recording resolutions. The number of the streams recorded does not affect the recording duration, but affects the aggregate recording resolution.

</details>




