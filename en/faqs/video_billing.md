
---
title: Billing for the video call
description: 视频通话计费说明
platform: All Platforms
updatedAt: Mon Dec 16 2019 20:31:53 GMT+0800 (CST)
---
# Billing for the video call
## Calculating service minutes




Agora will add up the following two minutes used by the project corresponding to your [App ID](https://console.agora.io/) on a monthly basis:

- [Video minutes](#vmin)
- [Audio minutes](#amin)
  







### <a name="vmin"></a>Video minutes 

If a user successfully receives video stream(s) after joining an RTC channel, the corresponding time counts as the video minutes. 

Agora adds up the resolutions of the video streams, to which a specific user subscribes at a time, to get the aggregate resolution. The aggregate resolution can be classified into two brackets, and Agora will charge you accordingly: 



| Video Bracket         | Aggregate Resolution |
| :-------------------- | :------------------- |
| High Definition (HD)  | ≤ 1280 x 720         |
| Super High Definition | > 1280 x 720         |





<div class="alert note">The aggregate resolution varies with the resolution of the subscribed video stream(s) in real time. Agora adds up the corresponding video minutes with an accuracy of seconds.</div>

**Calculate the aggregate resolution**

Suppose that user A has been in an RTC channel for 45 straight minutes, subscribing to the video streams of user B, C, and D. And the following table shows the resolutions of B, C, and D during this period:

|                       | B Resolution | C Resolution | D Resolution | Aggregate Resolution of A |
| --------------------- | ------------ | ------------ | ------------ | ------------------------- |
| **First 30 min**      | 640 x 360    | 640 x 360    | 640 x 360    | 691200 < 1280 x 720       |
| **Subsequent 15 min** | 640 x 360    | 240 x 180    | 1280 x 720   | 1195200 > 1280 x 720      |

As you can see from the above table: 

- Aggregate resolution of A for the first 30 min = Area B +Area C + Area D = 691200 < 1280 x 720, falling into the HD bracket. 
- Aggregate resolution of A for the subsequent 15 min = Area B +Area C + Area D = 1195200 > 1280 x 720, falling into the HD+ bracket. 

Total fee for user A = Unit price (video minutes HD) x 30 min + Unit price (video minutes HD+) x 15 min

> See [Pricing](#billing) for the pricing of the video minutes.
> 
> 

### <a name="amin"></a>Audio minutes 

If you deduct the time that a user receives the video stream(s) in the channel from the total time that the user stays in the channel, you get the audio minutes of that user, regardless of whether that user subscribes to any audio stream. 

<div class="alert note"><li>Your audio minutes will not add up, if you subscribe to multiple audio streams. </li><li>The way by which the audio minutes are calculated applies to the mini app. </li><li>See <a href="#billing">Pricing</a> for the pricing information of the audio minutes. </li></div>






## Pricing





| Service<a name="billing"></a> | Pricing （Dollars/1,000 minutes) |
| :---------------------------- | :------------------------------- |
| Audio                         | 0.99                             |
| Mini app audio                | 1.42                             |
| Video HD                      | 3.99                             |
| Mini app video                | 4.28                             |
| Video HD+                     | 14.99                            |









## Agora's policy of 10,000 free-of-charge minutes

See [Agora's policy of 10,000 free-of-charge minutes](https://docs.agora.io/en/faq/billing_free).
