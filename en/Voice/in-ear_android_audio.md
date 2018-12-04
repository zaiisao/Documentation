
---
title: In-ear Monitoring
description: How to enable in-ear monitoring and adjust the volume
platform: Android
updatedAt: Tue Dec 04 2018 19:03:26 GMT+0000 (UTC)
---
# In-ear Monitoring
In-ear monitoring can provide a mix of audio sources (for example the vocal and the music) to the host with a low latency, frequently used in professional scenarios such as concerts.
Agora SDK supports the in-ear monitoring function and adjusting the volume of the in-ear monitor.

## Implementation
Before proceeding, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Voice/android_audio.md) for more information.

```java
// Enables in-ear monitoring. The default value is false.
rtcEngine.enableInEarMonitoring(true);
  
// Sets the volume of the in-ear monitor. The value range is 0 to 100, and the default is 100 which represents the original volume captured by the microphone.
int volume = 80;
rtcEngine.setInEarMonitoringVolume(volume);
```
	 

## Considerations

The API methods have return values. If the method fails, the return value is < 0.
