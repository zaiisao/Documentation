
---
title: Billing for audio broadcasting
description: 语音通话计费说明
platform: All Platforms
updatedAt: Thu Apr 02 2020 14:13:09 GMT+0800 (CST)
---
# Billing for audio broadcasting
## Calculating service minutes


Agora adds up the [audio minutes](#amin) used by the projects under your [Agora Account](https://console.agora.io/) on a monthly basis.








> 

### <a name="amin"></a>Audio minutes 

If you deduct the time that a user receives the video streams in the channel from the total time that the user stays in the channel, you get the audio minutes of that user, regardless of whether that user subscribes to any audio stream. 





Suppose a user is in a channel for 30 minutes, then Agora logs 30 audio minutes for that user regardless of whether the user is idle or has subscribed to other users (because there is no way for a user in a voice call or an audio broadcast to subscribe to a video stream).



<div class="alert note"><li>A user's audio minutes do not add up, even if that user subscribes to multiple audio streams at the same time. </li><li>See <a href="#billing">Pricing</a> for the pricing information of the audio minutes. </li></div>






## Pricing



| Service<a name="billing"></a> | Pricing （Dollars/1,000 minutes) |
| :---------------------------- | :------------------------------- |
| Audio                         | 0.99                             |













## Agora's policy of 10,000 free-of-charge minutes

See [Agora's policy of 10,000 free-of-charge minutes](https://docs.agora.io/en/faq/billing_free).
