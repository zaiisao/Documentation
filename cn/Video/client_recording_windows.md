
---
title: 客户端通话录制
description: 
platform: Windows
updatedAt: Wed Feb 20 2019 06:40:09 GMT+0000 (UTC)
---
# 客户端通话录制
## 功能描述

在通话的过程中，将通话各方的声音录制下来，存放在本地，相当于手机上面的通话录音功能，录制下来的声音可用于回放。

Agora SDK 支持通话过程中在客户端进行录音。该方法录制频道内所有用户的音频，并生成一个包含所有用户声音的录音文件，录音文件格式可以为：

- WAV：文件大，音质保真度高
- AAC：文件小，有一定的音质损失

## 实现方法

````C++
// 初始化参数对象
RtcEngineParameters rep(*lpRtcEngine);

// 开始本地音频文件录制
#ifdef UNICODE
 CHAR aFilePath[MAX_PATH];
 ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, aFilePath, MAX_PATH, NULL, NULL);
int nRet = rep.startAudioRecording(aFilePath, // 本地合法文件路径
	AUDIO_RECORDING_QUALITY_TYPE::AUDIO_RECORDING_QUALITY_HIGH // 录音音质 AUDIO_RECORDING_QUALITY_HIGH|MEDIUM|LOW
	);
#else
int nRet = rep.startAudioRecording(filePath, AUDIO_RECORDING_QUALITY_TYPE::AUDIO_RECORDING_QUALITY_HIGH);
#endif

// 结束音频文件录制
int nRet = rep.stopAudioRecording();
```

### API 参考

* [`startAudioRecording`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#acb567614081900eaaf94d02b7c809af5)
* [`stopAudioRecording`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#ac5f5a19d5f32d7f7d7d2765caafcdaec)

## 开发注意事项

- 开启录音须在进入频道之后调用
- 离开频道会自动停止录音

