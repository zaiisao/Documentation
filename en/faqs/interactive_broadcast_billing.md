
---
title: Billing for the video broadcasting
description: 视频通话计费说明
platform: All Platforms
updatedAt: Mon Dec 16 2019 20:54:51 GMT+0800 (CST)
---
# Billing for the video broadcasting
## Calculating service minutes




Agora adds up the following two types of minutes used by the projects under your [Agora Account](https://console.agora.io/) on a monthly basis:

- [Video minutes](#vmin)
- [Audio minutes](#amin)
  







### <a name="vmin"></a>Video minutes 

If a user successfully receives video streams after joining an Agora RTC channel, the corresponding time counts as video minutes. 

Agora adds up the resolutions of the video streams, to which a specific user subscribes at a time, to determine that user's aggregate resolution. This aggregate resolution is classified into two brackets, and Agora charges you accordingly: 



| Video Bracket         | Aggregate Resolution |
| :-------------------- | :------------------- |
| High Definition (HD)  | ≤ 1280 x 720         |
| Super High Definition (HD+) | > 1280 x 720         |





<div class="alert note">The aggregate resolution varies with the resolution of the subscribed video streams in real time. Agora adds up the corresponding video minutes down to the accurary of a few seconds.</div>

**Calculate the aggregate resolution**

Suppose that user A has been in an RTC channel for 45 continuous minutes, subscribing to the video streams of users B, C, and D. The following table shows the resolutions of B, C, and D during this period:

|                       | B's Resolution | C's Resolution | D's Resolution | A's Aggregate Resolution |
| --------------------- | ------------ | ------------ | ------------ | ------------------------- |
| **Initial 30 min**      | 640 x 360    | 640 x 360    | 640 x 360    | 691200 < 1280 x 720       |
| **Subsequent 15 min** | 640 x 360    | 240 x 180    | 1280 x 720   | 1195200 > 1280 x 720      |

As you can see from the above table: 

- A's Aggregate resolution for the initial 30 min = B's resolution + C's resolution + D's resolution = 691200 < 1280 x 720, falling into the HD bracket. 
- Aggregate resolution of A for the subsequent 15 min = B's resolution +C's resolution + D's resolution = 1195200 > 1280 x 720, falling into the HD+ bracket. 

Total fee for user A = Unit price (video minutes HD) x 30 min + Unit price (video minutes HD+) x 15 min

> See [Pricing](#billing) for the pricing of the video minutes.
> 
> 

### <a name="amin"></a>Audio minutes 

If you deduct the time that a user receives the video streams in the channel from the total time that the user stays in the channel, you get the audio minutes of that user, regardless of whether that user subscribes to any audio stream. 

For example, let's say a user is in a channel for 30 minutes. This user subscribs to a video stream for 20 minutes, and is idle for the rest 10 minutes. In this case, Agora records 20 video minutes and 10 audio minutes for this specific user.

<div class="alert note"><li>A user's audio minutes do not add up, even if that user subscribes to multiple audio streams at the same time. </li><li>See <a href="#billing">Pricing</a> for the pricing information of the audio minutes. </li></div>






## Pricing





| Service<a name="billing"></a> | Pricing （Dollars/1,000 minutes) |
| :---------------------------- | :------------------------------- |
| Audio                         | 0.99                             |
| Video HD                      | 3.99                             |
| Video HD+                     | 14.99                            |









## Agora's policy of 10,000 free-of-charge minutes

See [Agora's policy of 10,000 free-of-charge minutes](https://docs.agora.io/en/faq/billing_free).
