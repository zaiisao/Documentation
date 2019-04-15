
---
title: Set the Log File
description: 
platform: Android
updatedAt: Wed Feb 20 2019 07:51:46 GMT+0800 (CST)
---
# Set the Log File
## Introduction
The Agora Native SDK provides API methods for you to generate an output log file that records all the log data of the SDK operation. You can use the log filters in the order of OFF, CRITICAL, ERROR, WARNING, INFO and DEBUG to get the logs that you need. Select a level, and all the logs in the levels preceding this level are generated. For example, if you set the log filter as OFF, no log is generated, and if you set the log filter as WARNING, you see the logs in the CRITICAL, ERROR and WARNING levels.

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Audio%20Broadcast/android_audio.md).

```java
// set log filter to debug
engine.setLogFilter(LOG_FILTER_DEBUG);

// get document path and save to sdcard
// get current timestamp to separate log files
String ts = new SimpleDateFormat("yyyyMMdd").format(new Date());
String filepath = "/sdcard/" + ts + ".log";
File file = new File(filepath);
engine.setLogFile(filepath);
```

### API Reference

- [`setLogFile`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab25d55c7f95903ff09280e308a977c08)
- [`setLogFilter`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#abb16ab61cebb6c676e1aab61030c3181)
