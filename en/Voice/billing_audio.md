
---
title: Billing for Voice Call
description: 
platform: All Platforms
updatedAt: Thu Dec 26 2019 08:49:51 GMT+0800 (CST)
---
# Billing for Voice Call
## Calculating service minutes


Agora will add up the [audio minutes](#amin) used by the project corresponding to your [App ID](https://console.agora.io/) on a monthly basis.








> 

### <a name="amin"></a>Audio minutes 

If you deduct the time that a user receives the video streams in the channel from the total time that the user stays in the channel, you get the audio minutes of that user, regardless of whether that user subscribes to any audio stream. 

<div class="alert note"><li>Your audio minutes do not add up, even if you subscribe to multiple audio streams. </li><li>See <a href="#billing">Pricing</a> for the pricing information of the audio minutes. </li></div>






## Pricing



| Service<a name="billing"></a> | Pricing （Dollars/1,000 minutes) |
| :---------------------------- | :------------------------------- |
| Audio                         | 0.99                             |











## FAQ



<details>
	<summary><font color="#3ab7f8">Question: Are the audio minutes from my bill for a specific user?</font></summary>

No. The audio minutes that you see from your bill are the sum of the audio minutes used by all users under your Agora account. In other words, the audio minutes are <i>not</i> for a specific user or users in a specific channel.  

</details>








## Agora's policy of 10,000 free-of-charge minutes

See [Agora's policy of 10,000 free-of-charge minutes](https://docs.agora.io/en/faq/billing_free).
