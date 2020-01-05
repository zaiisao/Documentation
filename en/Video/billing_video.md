
---
title: Billing for Video Call
description: 
platform: All Platforms
updatedAt: Thu Jan 02 2020 07:32:42 GMT+0800 (CST)
---
# Billing for Video Call
## Overview


At the beginning of each calendar month, Agora charges you for services used. This article describes how Agora calculates the billing for the services used of the Video SDK. You can see additional billing information pertaining to other Agora products and add-ons below:



- [Billing for On-premise Recording](https://docs.agora.io/en/Recording/billing_recording?platform=All%20Platforms)
- [Billing for Cloud Recording](https://docs.agora.io/en/cloud-recording/billing_cloud_recording?platform=All%20Platforms)



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









## Considerations



### Resolution calculation when dual-stream mode is enabled 

When the remote video stream being subscribed to enables [dual-stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode), the local subscriber can receive only one stream at a time: 

- If it is the high-video stream, then the local subscriber's composite resolution is calculated based on the high-video stream resolution that the remote user sets. 
- If it is the low-video stream, then the local subscriber's composite resolution is calculated based on the resolution of the received stream. 

### Resolution Calibration

When it comes to calculating a composite resolution, we take the resolution of all streams whose area is 225280 (640 x 352) as 640 x 360. 







## Q&A



<details>
	<summary><font color="#3ab7f8">Question: Are the audio minutes on my bill for a specific user?</font></summary>

No. The audio minutes that you see on your bill are the sum of the audio minutes used by all users under your Agora account. In other words, the audio minutes are <i>not</i> for a specific user or for users in a specific channel.  

</details>




<details>
	<summary><font color="#3ab7f8">Question: Why have I seen audio minutes in my bill even though all users subscribe to video streams?</font></summary>

Chances are: <ul><li>The user being subscribed to has not subscribed to any video stream.</li><li>After subscribing to a video stream, a user has not received any video stream due to poor network conditions or issues on the host side. </li></ul> If either of these conditions occurs, the corresponding user's aggregate resolution is 0 and the user's service time counts as the audio minutes. 

</details>
<details>
	<summary><font color="#3ab7f8">Question: Why have I been charged for HD+ video minutes e, even though all the users subscribe only to video streams with resolution of 360 x 640?</font></summary>

The aggregate resolution is a sum of all the resolutions of the video streams, to which a user subscribe. That said, the more video streams a user subscribe to, the more likely that user's aggregate resolution falls into the HD+ bracket ( > 1280 x 720). 
</details>







