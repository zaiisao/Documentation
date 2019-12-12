
---
title: 如何设置日志文件
description: 
platform: All Platforms
updatedAt: Thu Oct 31 2019 18:45:33 GMT+0800 (CST)
---
# 如何设置日志文件
Agora SDK 提供设置 SDK 的输出日志文件的功能，SDK 运行时产生的所有 log 将写入该文件。

## Native 平台

Native 平台包括 Android、iOS、macOS 和 Windows 平台。

### 设置日志文件

为保证输出的日志完整，我们建议在创建并初始化 RtcEngine 后，就调用 `setLogFile` 方法设置日志文件。各平台默认的日志文件输出地址如下：

- Android：`/sdcard/{App 的包名}/agorasdk.log`
- iOS：`App Sandbox/Library/caches/agorasdk.log`
- macOS：`～/Library/Logs/agorasdk.log`
- Windows：`C:\Users\{user_name}\AppData\Local\Agora\{process_name}`

### 设置日志输出等级

调用 `setLogFilter` 方法设置日志的输出等级。选择一个级别，你就可以看到在该级别及之前所有级别的日志信息。

按照输出日志最全到最少排列：

- DEBUG 级别：输出所有的 API 日志。如果你想获取最完整的日志，可将日志级别设为该等级
- INFO 级别：输出 CRITICAL、ERROR、WARNING、INFO 级别的日志。我们推荐你将日志级别设为该等级
- WARNING 级别：仅输出 CRITICAL、ERROR、WARNING 级别的日志
- ERROR 级别：仅输出 CRITICAL、ERROR 级别的日志
- CRITICAL 级别：仅输出 CRITICAL 级别的日志
- OFF：不输出任何日志

### 设置日志文件大小

Agora SDK 设有 2 个日志文件，每个文件默认大小为 512 KB。如果希望增加日志文件大小，还可以调用 `setLogFileSize` 方法设置日志文件大小。

### 示例代码

```java
// Java
// 将日志过滤器等级设置为 LOG_FILTER_DEBUG
engine.setLogFilter(LOG_FILTER_DEBUG);

// 获取在 SD 卡中的文件路径
// 获取时间戳
String ts = new SimpleDateFormat("yyyyMMdd").format(new Date());
String filepath = "/sdcard/" + ts + ".log";
File file = new File(filepath);
engine.setLogFile(filepath);
```

```objective-c
// Objective-C
// 将日志输出等级设置为 AgoraLogFilterDebug
[engine setLogFilter: AgoraLogFilterDebug];

// 获取当前目录
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
// 获取文件路径
// 获取时间戳
NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
[formatter setDateFormat:@"ddMMyyyyHHmm"];
NSDate *currentDate = [NSDate date];
NSString *dateString = [formatter stringFromDate:currentDate];
NSString *logFilePath = [NSString stringWithFormat:@"%@/%@.log", [paths objectAtIndex:0], dateString];
// 设置日志文件的默认地址
[engine setLogFile:logFilePath]
```

```C++
// C++
TCHAR szAppFolder[MAX_PATH] = { 0 };
SHGetFolderPath(NULL, CSIDL_APPDATA, NULL, 0, szAppFolder);
_tcscat(szAppFolder, _T("\\AppName\\"));

if (!PathFileExists(szAppFolder)){
    // 如果没有目录，创建一个新目录
    CreateDirectory(szAppFolder, NULL);
}

if (PathFileExists(szAppFolder)){
    // 创建日志文件
    TCHAR szFile[MAX_PATH] = { 0 };
    SYSTEMTIME st = { 0 };
    GetLocalTime(&st);
    // 获取时间戳
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

### 获取堆栈信息

发生 crash 时会告知产生 crash 的堆栈信息。各平台获取堆栈信息的方法如下：

- Android 平台：运行 `adb bugreport` 命令
- iOS：Xcode → window → Devices → 选中相关设备 → view device logs → 相关程序，相关时间的 log → 右键 export log
- macOS：~/Library/Logs/DiagnosticReports/

在 Android 和 iOS 平台，如果你在 app 中集成了 Bugly，可以直接通过 Bugly 获取。

### API 参考

- Android
	- [`setLogFile`](https://docs.agora.io/cn/faqs/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab25d55c7f95903ff09280e308a977c08)
	- [`setLogFilter`](https://docs.agora.io/cn/faqs/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#abb16ab61cebb6c676e1aab61030c3181)
	- [`setLogFileSize`](https://docs.agora.io/cn/faqs/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a50fd37c6f5b8fc144b18ed4620aee6fc)

- iOS/macOS
	- [`setLogFile`](https://docs.agora.io/cn/faqs/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLogFile:)
	- [`setLogFilter`](https://docs.agora.io/cn/faqs/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLogFilter:)
	- [`setLogFileSize`](https://docs.agora.io/cn/faqs/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:)

- Windows
	- [`setLogFile`](https://docs.agora.io/cn/faqs/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ae5a8ef2082a0ac196ecc128ee408def3)
	- [`setLogFilter`](https://docs.agora.io/cn/faqs/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#acfc056b4c693d134bece8e7c0f05e69f)
	- [`setLogFileSize`](https://docs.agora.io/cn/faqs/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a1fa35cd4f874109a26895a95096a873f)


## Web 平台

### 开启/关闭日志上传

Agora Web SDK 通过 `enableLogUpload` 方法将 SDK 的日志上传到 Agora 的服务器，通过 `disableLogUpload` 方法停止上传。

为保证输出的日志完整，我们建议在创建 Client 前就调用 `enableLogUpload` 方法开启日志上传服务。

<div class="alert note">如果没有成功加入频道，则服务器上无法查看日志信息。</div>

### 设置日志输出等级

你可以调用 `setLogLevel` 方法设置日志的输出等级。选择一个级别，你就可以看到在该级别及之前所有级别的日志信息。

按照输出日志最全到最少排列：

- DEBUG：输出所有 API 日志信息
- INFO：输出 INFO、WARNING 和 ERROR 级别的日志信息
- WARNING：输出 WARNING 和 ERROR 级别的日志信息
- ERROR：输出 ERROR  级别的日志信息
- NONE：不输出日志信息

### 示例代码

```javascript
// Javascript
// 开启日志上传功能
AgoraRTC.Logger.enableLogUpload();
// 将日志输出级别设置为 INFO
AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.INFO);
```

### API 参考

- [`enableLogUpload`](https://docs.agora.io/cn/faqs/API%20Reference/web/modules/agorartc.logger.html#enablelogupload)
- [`disableLogUpload`](https://docs.agora.io/cn/faqs/API%20Reference/web/modules/agorartc.logger.html#disablelogupload)
- [`setLogLevel`](https://docs.agora.io/cn/faqs/API%20Reference/web/modules/agorartc.logger.html#setloglevel)

