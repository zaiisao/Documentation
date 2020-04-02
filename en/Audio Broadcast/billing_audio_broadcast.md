
---
title: Billing for Audio broadcasting
description: 
platform: All Platforms
updatedAt: Thu Jan 02 2020 07:44:22 GMT+0800 (CST)
---
# Billing for Audio broadcasting
## Overview


At the beginning of each calendar month, Agora charges you for services used. This article describes how Agora calculates the billing for the services used of the Audio Broadcasting SDK. You can see additional billing information pertaining to other Agora products and add-ons below:



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


Agora adds up the [audio minutes](#amin) used by the projects under your [Agora Account](https://console.agora.io/) on a monthly basis.








> 

### <a name="amin"></a>Audio minutes 

If you deduct the time that a user receives the video streams in the channel from the total time that the user stays in the channel, you get the audio minutes of that user, regardless of whether that user subscribes to any audio stream. 





Suppose a user is in a channel for 30 minutes, then Agora logs 30 audio minutes for that user regardless whether the user is idle or has subscribed to other users (because there is no way for a user in a voice call to subscribe to a video stream).



<div class="alert note"><li>A user's audio minutes do not add up, even if that user subscribes to multiple audio streams at the same time. </li><li>See <a href="#billing">Pricing</a> for the pricing information of the audio minutes. </li></div>






## Pricing



| Service<a name="billing"></a> | Pricing （Dollars/1,000 minutes) |
| :---------------------------- | :------------------------------- |
| Audio                         | 0.99                             |











## Q&A



<details>
	<summary><font color="#3ab7f8">Question: Are the audio minutes on my bill for a specific user?</font></summary>

No. The audio minutes that you see on your bill are the sum of the audio minutes used by all users under your Agora account. In other words, the audio minutes are <i>not</i> for a specific user or for users in a specific channel.  

</details>









