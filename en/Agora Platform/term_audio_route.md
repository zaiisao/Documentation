
---
title: audio route
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 12:02:17 GMT+0800 (CST)
---
# audio route
The audio route is the pathway audio data takes through audio hardware components during playback. Earpieces, headphones, speakerphones, and Bluetooth devices that output sound (such as Bluetooth earpieces), can all work as an audio route.

Mobile apps, once integrated with the Agora RTC SDK, use different default audio routes in different scenarios:

- Voice call: Earpiece
- Audio broadcast: Speakerphone
- Video call: Speakerphone
- Video broadcast: Speakerphone

The default audio route is subject to user behavior, such as plugging in or unplugging an external device. It is also subject to the restrictions of the operating system. To better control the audio route, developers need to have a clear picture of how audio route changes under various circumstances.

<a href="../../en/Agora%20Platform/terms.md"><button>Back to glossary</button></a>
