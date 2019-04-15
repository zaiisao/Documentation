
---
title: Set the Log File
description: 
platform: Windows
updatedAt: Mon Apr 15 2019 10:03:24 GMT+0800 (CST)
---
# Set the Log File
## Introduction
The Agora Native SDK provides API methods for you to generate an output log file that records all the log data of the SDK operation. You can use the log filters in the order of OFF, CRITICAL, ERROR, WARNING, INFO and DEBUG to get the logs that you need. Select a level, and all the logs in the levels preceding this level are generated. For example, if you set the log filter as OFF, no log is generated, and if you set the log filter as WARNING, you see the logs in the CRITICAL, ERROR and WARNING levels.

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Audio%20Broadcast/android_video.md).

```C++

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

### API Reference

- [`setLogFile`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html?transId=534dd520-0344-11e9-bbd0-251679929d6b#a0e11f89f5b900279ed82a9d4fa9eb18a)
- [`setLogFilter`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html?transId=534dd520-0344-11e9-bbd0-251679929d6b#a169cd86502290529b02eaf6748a63f2a)
