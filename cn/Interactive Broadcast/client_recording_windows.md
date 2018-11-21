
---
title: 客户端通话录制
description: 
platform: Windows
updatedAt: Wed Nov 21 2018 10:34:25 GMT+0000 (UTC)
---
# 客户端通话录制
## 功能描述

在通话的过程中，将通话各方的声音录制下来，存放在本地，相当于手机上面的通话录音功能，录制下来的声音可用于回放。

Agora SDK 支持通话过程中在客户端进行录音。该方法录制频道内所有用户的音频，并生成一个包含所有用户声音的录音文件，录音文件格式可以为：

- wav：文件大，音质保真度高
- aac：文件小，有一定的音质损失

## 实现方法

```C++
RtcEngineParameters rep(*lpRtcEngine);
int nRet;

// start recording audio to local storage

#ifdef UNICODE
 CHAR aFilePath[MAX_PATH];
 ::WideCharToMultiByte(CP_ACP, 0, filePath, -1, aFilePath, MAX_PATH, NULL, NULL);
 nRet = rep.startAudioRecording(aFilePath, AUDIO_RECORDING_QUALITY_TYPE::AUDIO_RECORDING_QUALITY_HIGH);
#else
 nRet = rep.startAudioRecording(filePath);
#endif

// stop recording audio file
ret = rep.stopAudioRecording();

// constants for quality of audio file saved

/** Audio recording quality.
*/
enum AUDIO_RECORDING_QUALITY_TYPE
{
/** 0: Low audio recording quality.
*/
AUDIO_RECORDING_QUALITY_LOW = 0,
/** 1: Medium audio recording quality.
*/
AUDIO_RECORDING_QUALITY_MEDIUM = 1,
/** 2: High audio recording quality.
*/
AUDIO_RECORDING_QUALITY_HIGH = 2,
};

```

