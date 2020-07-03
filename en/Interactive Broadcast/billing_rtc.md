
---
title: Billing for Real-time Communication
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 04:27:44 GMT+0800 (CST)
---
# Billing for Real-time Communication
This article introduces the billing policy for the real-time communication (RTC) service provided by Agora.

<div class="alert note">Your billing details may differ if you have signed a contract with Agora.</div>

## Overview

Agora calculates the billing of all projects under your Agora account monthly.

Billing for RTC begins once you implement an RTC function, such as audio call, group video call, or video broadcast, using the Agora RTC SDK. On the first day of each month, Agora sends you the bill via email, and five days later deducts the payment from your account. For details, see [Billing, fee deduction, and account suspension](https://docs.agora.io/en/faq/billing_account).

<div class="alert note">
	<ul>
		<li>Agora gives each Agora account 10,000 charge-free minutes each month. For more information on the deduction sequence and applicable products, see<a href="https://docs.agora.io/cn/faq/billing_free"> Agora's free-of-charge policy for the first 10,000 minutes</a>.</li>
		<li>If your scenario involves the RTMP converter, expect additional transcoding charges.</li>
	</ul>
</div>

## Composition

Agora calculates your billing based on the sessions under each project. The billing for each session equals the total sum of charges for all users in the session.

For each user, Agora charges for the service according to their subscribing behavior in the session. Therefore, the charges for each user comprise the following:

- Video charges: Apply when the user successfully subscribes to video.
- Audio charges: Apply when the user does not subscribe to video, regardless of whether the user subscribes to audio.

**Cost for each user = Video charges + Audio charges = Video pricing × video service minutes + Audio pricing × audio service minutes**.

<div class="alert note">
	<ul>
		<li>If a user successfully subscribes to both audio and video at the same time, then Agora only charges for the video service.</li>
		<li>For sessions with only one user, the cost of the session equals the audio pricing × service minutes of the session.</li>
	</ul>
</div>

## Pricing

The pricing for audio and video are as follows:

| Service type | Pricing ($US/1,000 minutes) |
| ---------------- | ---------------- |
| Audio      | 0.99      |
| Video      |<ul><li>High-Definition (HD): 3.99</li><li>High-Definition Plus (HD+): 14.99</li></ul> |

<div class="alert note">The live video streaming pricing stated on <a href="https://www.agora.io/en/pricing/">the official website</a> does not apply by default. Please <a href="mailto:support.agora.io">contact our sales</a> to get this rate.</div>

### Video aggregate resolution

Agora adds up the resolution of all the video streams a user subscribes to at the same time to determine the user's **aggregate resolution**, which categorizes video as either HD or HD+:

- HD: The aggregate resolution is smaller than or equal to 921600 (1280 × 720).
- HD+: The aggregate resolution is greater than 921600 (1280 × 720).

Suppose users A, B, C, and D are in the same channel, and A subscribes to the video of B, C, and D.

**Example 1**

Suppose the resolution of the videos A subscribes to are as follows:

- Video from B: 640 × 360
- Video from C: 640 × 360
- Video from D: 640 × 360

The aggregate resolution: (640 × 360) + (640 × 360) + (640 × 360) = 691200.

Since 691,200 is smaller than 921,600, the aggregate resolution of A falls into the category of HD, and is charged $3.99/1,000 minutes.

**Example 2**

Suppose the resolution of the videos A subscribes to change during the session:

- Video from B: 640 × 360
- Video from C:
  - The initial 10 minutes: 640 × 360
  - The subsequent 10 minutes: 240 × 180
- Video from D:
  - The initial 10 minutes: 640 × 360
  - The subsequent 10 minutes: 1280 × 720

The aggregate resolution of A is calculated as follows:

| Time | Video from B | Video from C | Video from D | The aggregate resolution |
| ---------------- | ---------------- | ---------------- | ---------------- | ---------------- |
| The initial 10 minutes | 640 × 360      | 240 × 180      | 1280 × 720 | 1195200 |
| The subsequent 10 minutes | 640 × 360      | 640 × 360      | 640 × 360   | 691200 |

As shown in the table:

- In the initial 10 minutes, the aggregate resolution that A subscribes to is 691200 (< 921600), and should be charged as HD, at the rate of $3.99/1,000 minutes.
- In the subsequent 10 minutes, the aggregate resolution that A subscribes to is 1195200 (> 921600), and should be charged as HD+, at the rate of $14.99/1,000 minutes.

## Service minutes

Service minutes (accurate to seconds), are calculated from a user joining a channel to the user leaving the channel.

Depending on the subscribing behavior of the user, service minutes comprises the following:

- Video minutes: The total duration of the user receiving video.
- Audio minutes: If you deduct the video minutes from the total duration when the user is in the channel, you get the audio minutes of that user, regardless of whether that user subscribes to any audio.

<div class="alert note">If the user subscribes to multiple audio and video streams at the same time, the total service minutes do not add up by streams. For example:
<ul>
	<li>If User A subscribes to the video streams of both B and C for the same 10 minutes, the billing for A is the billing for 10 minutes of video.</li>
	<li>If User A subscribes to the audio stream of B, and video stream of C, both for the same 10 minutes, the billing for A is also the billing for 10 minutes of video.</li>
</ul>
</div>

## Examples

<div class="alert note">Use the following examples to better understand Agora's RTC billing policy:</div>

### Video call with two users

**Scenario**: Users A and B join the channel at the same time and have a video call for 20 minutes.

**Cost**: In this session, both A and B use the video service. 
- Billing for A: Charges for subscribing to the video of B = Video pricing × 20 minutes/1000 minutes.
- Billing for B: Charges for subscribing to the video of A = Video pricing × 20 minutes/1000 minutes.

### Voice call with three users

**Scenario**: Users A, B, and C join the channel at the same time and have a voice call for 20 minutes.

**Cost**: In this session, A, B, and C use the audio service. Billing = (Audio pricing × 20 minutes/1000 minutes) + (Audio pricing × 20 minutes/1000 minutes) + (Audio pricing × 20 minutes /1000 minutes).

### Video call with four users

**Scenario**: Users A, B, and C join the channel at the same time and have a voice call. 10 minutes later, user D joins the channel, and has a video call with A, B, and C for 10 minutes.

**Cost**: In this session, A, B, C, and D use either the audio or video service.
- Billing for A:
  - The initial 10 minutes: Charges for subscribing to the audio of B and C = Audio pricing × 10 minutes /1000 minutes.
  - The subsequent 10 minutes: Charges for subscribing to the video of B, C, and D = Video pricing × 10 minutes/1000 minutes.
- Billings for B and C are the same as the billing for A.
- Billing for D:
  - The initial 10 minutes: No charges occur since D is not in the channel.
  - The subsequent 10 minutes: Charges for subscribing to the video of A, B, and C = Video pricing × 10 minutes/1000 minutes.

### Single-hosted video broadcast

**Scenario**: User A joins a video broadcast channel and hosts for 20 minutes. Six people watch the broadcast, three subscribing to audio, and three subscribing to video.

**Cost**: Since user A does not subscribe to any stream in the channel, A is charged for the audio service only. For the audience, three people are charged for the video service, and three for the audio service.
- Billing for A: Audio billing = Audio pricing × 20 minutes/1000 minutes.
- Billing for the 3 audience members subscribing to the video: 3 × Video pricing × 20 minutes/1000 minutes.
- Billing for the 3 audience members subscribing to the audio: 3 × Audio pricing × 20 minutes/1000 minutes.

### Co-hosted video broadcast

**Scenario**: User A hosts in a video broadcast channel, with six audience members. 10 minutes later, user B changes the user role and co-hosts with A for 10 minutes. The remaining audience continues watching the video broadcast.

**Cost**: 
- Billing for A:
  - The initial 10 minutes: Since A does not subscribe to any stream in the channel, billing = Audio charges = Audio pricing × 10 minutes/1000 minutes.
  - The subsequent 10 minutes: Video charges for subscribing to the video of B = Video pricing × 10 minutes/1000 minutes.
- Billing for B: Billing for subscribing to the video of A for 20 minutes = Video pricing × 20 minutes/1000 minutes.
- The remaining five audience members:
  - The initial 10 minutes: Billing for subscribing to the video of A = 5 × Video pricing × 10 minutes/1000 minutes.
  - The initial 10 minutes: Billing for subscribing to the video of both A and B = 5 × Video pricing × 10 minutes/1000 minutes.

## Considerations

### Video resolution in the dual-stream scenario

In dual-stream mode, the aggregate video resolution is calculated as follows:

- If the user subscribes to the high-quality video stream, the aggregate resolution is calculated based on the resolution of the high-quality video.
- If the user subscribes to the low-quality video stream, the aggregate resolution is calculated based on the resolution of the video received by the user.

### Resolution calibration

When calculating the aggregate resolution, Agora counts the resolution of 225280 (640 × 352) as 640 × 360.

## FAQ

<details><summary><font color="#3ab7f8">Why do I see audio minutes in my bill even though all users subscribe only to video streams?</font></summary>
Chances are:
	<ul>
	<li>The user being subscribed to has not subscribed to any video stream.</li>
	<li>After subscribing to a video stream, a user has not received any video stream due to poor network conditions.</li>
</ul>
If either of these conditions occurs, the corresponding user's aggregate resolution is 0 and the user's service time counts as audio minutes.
</details>
<details><summary><font color="#3ab7f8">Why am I charged for HD+ video, even though all users subscribe only to video streams with the resolution of 360 × 640?</font></summary>
Video streams are categorized as HD or HD+ by <b>aggregate resolution</b>, which is the sum of all the resolutions of the video streams a user subscribes to. That said, the more video streams a user subscirbes to, the more likely that user's aggregate resolution falls into HD+ (1280 × 720).
</details>
<details><summary><font color="#3ab7f8">Are the audio minutes on my bill for a specific user?</font></summary>
No. The audio minutes that you see on your bill are the sum of the audio minutes used by all users under your Agora account.
</details>

## Reference

- [Agora's free-of-charge policy for the first 10,000 minutes](https://docs.agora.io/en/faq/billing_free)
- [Billing, free deduction, and account suspension](https://docs.agora.io/en/faq/billing_account)
- [How can I bill individual?](https://docs.agora.io/en/faq/business_billing)
