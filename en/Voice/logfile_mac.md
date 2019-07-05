
---
title: Set the Log File
description: 
platform: macOS
updatedAt: Fri Jul 05 2019 07:48:17 GMT+0800 (CST)
---
# Set the Log File
## Introduction
The Agora Native SDK provides API methods for you to generate an output log file that records all the log data of the SDK operation. You can use the log filters in the order of OFF, CRITICAL, ERROR, WARNING, INFO and DEBUG to get the logs that you need. Select a level, and all the logs in the levels preceding this level are generated. For example, if you set the log filter as OFF, no log is generated, and if you set the log filter as WARNING, you see the logs in the CRITICAL, ERROR and WARNING levels.

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Voice/android_video.md).

```Objective-C
// set log filter to debug
[engine setLogFilter: AgoraLogFilterDebug];

// get document path
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
// get file path
// get current timestamp to separate log files
NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
[formatter setDateFormat:@"ddMMyyyyHHmm"];
NSDate *currentDate = [NSDate date];
NSString *dateString = [formatter stringFromDate:currentDate];
NSString *logFilePath = [NSString stringWithFormat:@"%@/%@.log", [paths objectAtIndex:0], dateString];
// set log file path
[engine setLogFile:logFilePath]
```

### API Reference

- [`setLogFile`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html?transId=8d992290-01c1-11e9-a659-33e4b5b761ac#//api/name/setLogFile:)
- [`setLogFilter`](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html?transId=8d992290-01c1-11e9-a659-33e4b5b761ac#//api/name/setLogFilter:)

## Considerations
- On macOS, the default log file location is as follows:
	- Sandbox enabled: App Sandbox/Library/Logs/agorasdk.log, for example /Users/&lt;username&gt;/Library/Containers/&lt;App Bundle Identifier&gt;/Data/Library/Logs/agorasdk.log
	- Sandbox disabled: ～/Library/Logs/agorasdk.log
- Ensure that you call this method immediately after calling the [sharedEngineWithAppId](https://docs.agora.io/en/Voice/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/sharedEngineWithAppId:delegate:) method, otherwise the output log might not be complete.
