
---
title: In-ear Monitoring
description: How to enable in-ear monitoring and adjust the volume
platform: Android
updatedAt: Mon Jul 06 2020 10:35:02 GMT+0800 (CST)
---
# In-ear Monitoring
In-ear monitoring provides a mix of the audio sources (for example, a mix of the vocals and music) to the host with low latency, commonly used in professional scenarios, such as in concerts.
The Agora SDK supports the in-ear monitoring function and volume adjustment of the in-ear monitor.

## Implementation
Before using the in-ear monitoring function, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Interactive%20Broadcast/start_call_android.md) or [Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_android.md).

The Agora SDK provides the `enableInEarMonitoring` and `setInEarMonitoringVolume` methods to set the in-ear monitoring according to the scenario, you can use these methods no matter before or after `joinChannel`.

### Sample code

```java
// Enables in-ear monitoring. The default value is false.
rtcEngine.enableInEarMonitoring(true);
  
// Sets the volume of the in-ear monitor. The value ranges between 0 and 100. The default value is 100, which represents the original volume captured by the microphone.
int volume = 80;
rtcEngine.setInEarMonitoringVolume(volume);
```
	 
### API reference

- [`enableInEarMonitoring`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aeb014fcf7ec84291b9b39621e09772ea)
- [`setInEarMonitoringVolume`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af71afdf140660b10c4fb0c40029c432d)

## Considerations

- Call `enableInEarMonitoring` before `setInEarMonitoringVolume`.
- The API methods have return values. If the method call fails, the return value is < 0.
