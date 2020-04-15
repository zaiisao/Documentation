
---
title: Why can't I play the background music using startAudioMixing on Android 10?
description: 
platform: Android
updatedAt: Tue Feb 25 2020 12:40:06 GMT+0800 (CST)
---
# Why can't I play the background music using startAudioMixing on Android 10?
## Issue description

Cannot play an mp3, mp4, or any other music format using `startAudioMixing` on Android 10.

## Reason

This is caused by an Android permission limit. If `targetSdkVersion` &ge; 29, you need to add relevant app privileges to play the a music file.

## Solution

For Android projects with `targetSdkVersion` &ge; 29, add the following line in the `application` zone of the AndroidManifest.xml file to play the music file:

```xml
<application
   Android:requestLegacyExternalStorage="true">
   …
</application>
```

## Relevant links

For more Android permission settings and considerations, see [Add project permissions](../../en/Interactive%20Broadcast/start_live_android.md).
