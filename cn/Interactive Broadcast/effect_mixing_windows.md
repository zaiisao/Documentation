
---
title: 播放音效/音乐混音
description: How to play audio effect files and enable audio mixing 
platform: Windows
updatedAt: Wed Nov 21 2018 08:01:09 GMT+0000 (UTC)
---
# 播放音效/音乐混音
## 功能描述
在通话或直播过程中，除了用户自己说话的声音，有时候需要播放自定义的声音或者音乐文件并且让频道内的其他人也听到，比如需要给游戏添加音效，或者需要播放背景音乐等，Agora 提供以下两组方法可以满足播放音效和音乐文件的需求。
## 播放音效文件

音效通常指持续很短的音频。播放音效文件方法主要用来播放短小的音效，比如鼓掌、游戏子弹撞击声音等，可以多个音效叠加播放。
音效由音频文件路径指定，但在 SDK 内部使用 sound id 来识别和处理音效。SDK 并不强制如何定义 sound id，保证每个音效有唯一的识别即可。一般的做法有自增 id，使用音效文件名的 hashCode 等。

### 实现方法

```c++
RtcEngineParameters rep(*lpAgoraEngine);

// 1. 预加载音效文件，可选

#ifdef UNICODE
  CHAR wdFilePath[MAX_PATH];
  ::WideCharToMultiByte(CP_ACP, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
  int nRet = rep.preloadEffect(nSoundID, wdFilePath);
#else
  int nRet = rep.preloadEffect(nSoundID, filePath);
#endif

// 2. 播放音效文件。如果设置了预加载，需要指定 nSoundID 

#ifdef UNICODE
  CHAR wdFilePath[MAX_PATH];
  ::WideCharToMultiByte(CP_ACP, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
  int nRet = rep.playEffect(nSoundID, wdFilePath, nLoopCount, dPitch, dPan, nGain, TRUE /* publish */);
#else
  int nRet = rep.playEffect(nSoundID, filePath, nLoopCount, dPitch, dPan, nGain, TRUE /* publish */);
#endif

// 3. 暂停播放指定的音效文件

nRet = rep.pauseEffect(nSoundID);

// 4. 暂停播放所有的音效文件

nRet = rep.pauseAllEffects();

// 5. 恢复播放指定的音效文件

nRet = rep.resumeEffect(nSoundID);

// 6. 恢复播放所有的音效文件

nRet = rep.resumeAllEffects();

// 7. 停止播放音效文件

nRet = rep.unloadEffect(nSoundID);

// 8. 从内存释放某个预加载的音效文件

nRet = rep.unloadEffect(nSoundID);
```

### 开发注意事项

- 注意音频文件的路径，以及文件是否完整可用
- publish 设置为 `TRUE` ，频道内的其他用户也可以听见该音效

## 音乐混音

混音是指播放本地或者在线音乐文件，同时让频道内的其他人听到此音乐。混音方法主要用来播放比较长的背景音，比如直播的时候播放的音乐，同时只可以有一个文件播放。如果在混音播放第一个文件的过程中播放第二个文件，会自动停止第一个文件的播放。
Agora 混音功能支持如下设置：

- 混音或替换： 混音指的是音乐文件的音频流跟麦克风采集的音频流进行混音（叠加）并编码发送给对方；替换指的是麦克风采集的音频被音乐文件的音频流替换掉，对方只能听见音乐播放。
- 循环：可以设置是否循环播放混音文件，以及循环次数。

### 实现方法

```c++
  LPCTSTR filePath = "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3";
  
  int nRet = 0;
  RtcEngineParameters rep(*lpAgoraEngine);
  
  // 1. 开始混音
  
  #ifdef UNICODE
   CHAR wdFilePath[MAX_PATH];
   ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
   nRet = rep.startAudioMixing(wdFilePath, FALSE /* loopback */, TRUE /* replace */, 1 /* repeat times */);
  #else
   nRet = rep.startAudioMixing(filePath, FALSE /* loopback */, TRUE /* replace */, 1 /* repeat times */);
  #endif
  
  // 2. 停止混音
  
  nRet = rep.stopAudioMixing();
```

### 开发注意事项

- 注意混音文件的路径，以及文件是否完整可用
- loopback 设置为 `TRUE` ，只有本地可以听见混音文件的声音
- replace 设置为 `TRUE` ，混音文件的内容会替换麦克风采集的音频
- repeat times 表示混音文件播放次数，设为 `-1` 表示无限循环播放
