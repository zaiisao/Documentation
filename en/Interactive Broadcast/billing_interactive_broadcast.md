
---
title: Billing for Video Broadcasting
description: 
platform: All Platforms
updatedAt: Thu Jan 02 2020 07:48:03 GMT+0800 (CST)
---
# Billing for Video Broadcasting
## Overview


Each month, Agora charges you by the services you use. This article describes how Agora calculates the services you use and provides the billing information of the Video Broadcasting SDK. If you have used other products or add-ons, you can also see the billing information below:



- [Billing for On-premise Recording](https://docs.agora.io/en/Recording/billing_recording?platform=All%20Platforms)
- [Billing for Cloud Recording](https://docs.agora.io/en/cloud-recording/billing_cloud_recording?platform=All%20Platforms)



For more information about billing, fee deduction, and account suspension, see [Billing, Fee Deduction, and Account Suspension](https://docs.agora.io/en/faq/billing_account).

## Calculate service minutes




Agora adds up the following two minutes used by the projects under your [Agora Account](https://console.agora.io/) on a monthly basis:

- [Video minutes](#vmin)
- [Audio minutes](#amin)
  







### <a name="vmin"></a>Video minutes 

If a user successfully receives video streams after joining an RTC channel, the corresponding time counts as the video minutes. 

Agora adds up the resolutions of the video streams, to which a specific user subscribes at a time, to get that user's aggregate resolution. The aggregate resolution can be classified into two brackets, and Agora charges you accordingly: 



| Video Bracket         | Aggregate Resolution |
| :-------------------- | :------------------- |
| High Definition (HD)  | ≤ 1280 x 720         |
| Super High Definition (HD+) | > 1280 x 720         |





<div class="alert note">The aggregate resolution varies with the resolution of the subscribed video streams in real time. Agora adds up the corresponding video minutes with an accuracy of seconds.</div>

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

If you deduct the time that a user receives the video streams in the channel from the total time that the user stays in the channel, you get the audio minutes of that user, regardless of whether that user subscribes to any audio stream. 

<div class="alert note"><li>Your audio minutes do not add up, even if you subscribe to multiple audio streams. </li><li>See <a href="#billing">Pricing</a> for the pricing information of the audio minutes. </li></div>






## Pricing





| Service<a name="billing"></a> | Pricing （Dollars/1,000 minutes) |
| :---------------------------- | :------------------------------- |
| Audio                         | 0.99                             |
| Video HD                      | 3.99                             |
| Video HD+                     | 14.99                            |









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






## Agora's policy of 10,000 free-of-charge minutes

See [Agora's policy of 10,000 free-of-charge minutes](https://docs.agora.io/en/faq/billing_free).
