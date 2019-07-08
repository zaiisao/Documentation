
---
title: 设置日志文件
description: 设置日志文件
platform: Windows
updatedAt: Fri Jul 05 2019 04:14:57 GMT+0800 (CST)
---
# 设置日志文件
## 功能简介
Agora Native SDK 提供设置 SDK 的输出日志文件的功能，SDK 运行时产生的所有 log 将写入该文件。

在调试你的应用时，你也可以设置日志的输出等级，顺序依次为 OFF、CRITICAL、ERROR、WARNING、INFO 和 DEBUG。选择一个级别后，会输出该级别及之前所有级别的日志信息。例如，选择 OFF 级别表示不输出任何日志；选择 WARNING 级别，表示输出 CRITICAL、ERROR 和 WARNING 级别上的所有日志信息。

## 实现方法

开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/windows_video.md)。

```C++

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

## API 参考
- [`setLogFile`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html?transId=ac204940-0343-11e9-bbd0-251679929d6b#a0e11f89f5b900279ed82a9d4fa9eb18a)
- [`setLogFilter`](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html?transId=ac204940-0343-11e9-bbd0-251679929d6b#a169cd86502290529b02eaf6748a63f2a)

## 注意事项
如需调用本方法，请在调用 [initialize](https://docs.agora.io/cn/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac71db65e66942e4e0a0550e95c16890f) 方法初始化 IRtcEngine 对象后立即调用，否则可能造成输出日志不完整。
