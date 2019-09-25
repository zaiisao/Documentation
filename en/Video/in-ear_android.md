
---
title: In-ear Monitoring
description: How to enable in-ear monitoring and adjust the volume
platform: Android
updatedAt: Wed Sep 25 2019 04:19:24 GMT+0800 (CST)
---
# In-ear Monitoring
In-ear monitoring provides a mix of the audio sources (for example, a mix of the vocals and music) to the host with low latency, commonly used in professional scenarios, such as in concerts.
The Agora SDK supports the in-ear monitoring function and volume adjustment of the in-ear monitor.

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Video/android_video.md).

```java
// Enables in-ear monitoring. The default value is false.
rtcEngine.enableInEarMonitoring(true);
  
// Sets the volume of the in-ear monitor. The value ranges between 0 and 100. The default value is 100, which represents the original volume captured by the microphone.
int volume = 80;
rtcEngine.setInEarMonitoringVolume(volume);
```
	 

## Considerations

The API methods have return values. If the method call fails, the return value is < 0.
