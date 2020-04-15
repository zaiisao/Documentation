
---
title: How can I set the log file?
description: 
platform: All Platforms
updatedAt: Wed Mar 25 2020 22:45:32 GMT+0800 (CST)
---
# How can I set the log file?
The Agora SDK provides API methods for you to generate an output log file that records all the log data of the SDK operation.

### Native

The native platform includes Android, iOS, macOS, and Windows.

### Set the log file

To ensure that the output log is complete, we recommend calling the `setLogFile` method to set the log file immediately after you create and initialize RtcEngine. The default output log file path for each platfom is as follows:

- Android：`/sdcard/{Package name of the app}/agorasdk.log`
- iOS：`App Sandbox/Library/caches/agorasdk.log`
- macOS：`～/Library/Logs/agorasdk.log`
- Windows：`C:\Users\{user_name}\AppData\Local\Agora\{process_name}`

### Set the log output level

You can call the `setLogFilter` method to set the output lof level. Select a level, and all the logs in the levels preceding this level are generated. 

- DEBUG: Outputs all API logs. Set your log filter as DEBUG if you want to get the most complete log file.
- INFO: Outputs logs of the CRITICAL, ERROR, WARNING, and INFO level. We recommend setting your log filter as this level.
- WARNING:  Outputs logs of the CRITICAL, ERROR, and WARNING level.
- ERROR: Outputs logs of the CRITICAL and ERROR level.
- CRITICAL: Outputs logs of the CRITICAL level.
- OFF: Outputs no log.

### Set the log file size

The Agora SDK has two log files, each with a default size of 512 KB. Call the `setLogFileSize` method if you want to increase the log file size

### Sample code

```java
// Java
// set log filter to debug
engine.setLogFilter(LOG_FILTER_DEBUG);

// get document path and save to sdcard
// get current timestamp to separate log files
String ts = new SimpleDateFormat("yyyyMMdd").format(new Date());
String filepath = "/sdcard/" + ts + ".log";
File file = new File(filepath);
engine.setLogFile(filepath);
```

```objective-c
// Objective-C
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

```C++
// C++
TCHAR szAppFolder[MAX_PATH] = { 0 };
SHGetFolderPath(NULL, CSIDL_APPDATA, NULL, 0, szAppFolder);
_tcscat(szAppFolder, _T("\\AppName\\"));

if (!PathFileExists(szAppFolder)){
    // create directory if not exists
    CreateDirectory(szAppFolder, NULL);
}

if (PathFileExists(szAppFolder)){
    // create file
    TCHAR szFile[MAX_PATH] = { 0 };
    SYSTEMTIME st = { 0 };
    GetLocalTime(&st);
    // Use timestamp to separate log files
    _stprintf_s(szFile, _T("%s%d%02d%02d_%02d%02d%02d.log"), szAppFolder, st.wYear, st.wMonth, st.wDay, st.wHour, st.wMinute, st.wSecond);
    HANDLE hFile = CreateFile(szFile, GENERIC_WRITE, 0, NULL, CREATE_ALWAYS, 0, NULL);
    if (INVALID_HANDLE_VALUE != hFile){
        CloseHandle(hFile);
        char logFullPath[MAX_PATH] = { 0 };
        ::WideCharToMultiByte(CP_UTF8, 0, szFile, -1, logFullPath, MAX_PATH, NULL, NULL);
        RtcEngineParameters rep(*engine);
        rep.setLogFile(logFullPath);
    }
}
```

### Get the stack information

You can also get the stack information when crashes occur:

- Android: Run the `adb bugreport` command
- iOS: Xcode → window → Devices → Select a device → view device logs → View logs of a certain app at a certain time → Right click export log
- macOS: ~/Library/Logs/DiagnosticReports/

On Android and iOS, if you have integrated Bugly in your app, you cal also use Bugly to get the stack information.

### API reference

- Android
	- [`setLogFile`](https://docs.agora.io/en/faqs/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab25d55c7f95903ff09280e308a977c08)
	- [`setLogFilter`](https://docs.agora.io/en/faqs/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#abb16ab61cebb6c676e1aab61030c3181)
	- [`setLogFileSize`](https://docs.agora.io/en/faqs/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a50fd37c6f5b8fc144b18ed4620aee6fc)

- iOS/macOS
	- [`setLogFile`](https://docs.agora.io/en/faqs/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLogFile:)
	- [`setLogFilter`](https://docs.agora.io/en/faqs/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLogFilter:)
	- [`setLogFileSize`](https://docs.agora.io/en/faqs/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:)

- Windows
	- [`setLogFile`](https://docs.agora.io/en/faqs/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ae5a8ef2082a0ac196ecc128ee408def3)
	- [`setLogFilter`](https://docs.agora.io/en/faqs/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acfc056b4c693d134bece8e7c0f05e69f)
	- [`setLogFileSize`](https://docs.agora.io/en/faqs/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a1fa35cd4f874109a26895a95096a873f)

## Web

### Enable or disable log upload

You can use the `enableLogUpload` method to upload logs on to the Agora Server, or call the disableLogUpload method to stop the upload.

To ensure that the output log is complete, we recommend calling the `setLogFile` method to set the log file before initializing the Client object.

<div class="alert note">If you fail to join the channel, the logs are unavailable on the Agora Server.</div>

### Set the log output level

Call  the `setLogLevel` method to set the log output level. Select a level, and you can see the logs in the preceding levels.

- DEBUG: Outputs all logs.
- INFO: Outputs logs of the INFO, WARNING and ERROR levels.
- WARNING: Outputs logs of the WARNING and ERROR levels.
- ERROR: Outputs logs of the ERROR level.
- NONE: Outputs no log.

### Sample code

```javascript
// Javascript
// Enable log upload
AgoraRTC.Logger.enableLogUpload();
// Set the log output level as INFO
AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.INFO);
```

### API reference

- [`enableLogUpload`](https://docs.agora.io/en/faqs/API%20Reference/web/modules/agorartc.logger.html#enablelogupload)
- [`disableLogUpload`](https://docs.agora.io/en/faqs/API%20Reference/web/modules/agorartc.logger.html#disablelogupload)
- [`setLogLevel`](https://docs.agora.io/en/faqs/API%20Reference/web/modules/agorartc.logger.html#setloglevel)



