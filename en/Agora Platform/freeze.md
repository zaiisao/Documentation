
---
title: freeze
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 12:47:15 GMT+0800 (CST)
---
# freeze
Freeze refers to choppy audio or video playback caused by a poor network connection or limited device performance during real-time audio and video communication.

To highlight transmission qualities and help developers locate issues, the Agora RTC SDK provides callbacks that report audio and video freeze statistics during a call. Developers can also track freezes with [Agora Analytics](../../en/Agora%20Platform/terms.md).

The Agora RTC SDK and Agora Analytics use different algorithms to decide when a freeze occurs:

| Product | Video freeze | Audio freeze |
| ---------------- | ---------------- | ---------------- |
| RTC SDK      | In a video session where the frame rate is set to 5 fps or higher, video freeze occurs when the time interval between two adjacent video frames is more than 500 ms.      | In the reported interval, audio freeze occurs when the audio frame loss rate reaches 4%.      |
| Agora Analytics   | Occurs when the video freezes for 600 ms. or more. | Occurs when the audio freezes for 200 ms. or more. |

<div class="alert info">See also: <a href="https://docs.agora.io/en/Agora%20Platform/aa_data_insight?platform=All%20Platforms#quality">Data Insight quality overview</a>
</div>

<a href="../../en/Agora%20Platform/terms.md"><button>Back to glossary</button></a>
