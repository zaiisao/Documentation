
---
title: 播放音效/音乐混音
description: How to play audio effect files and enable audio mixing 
platform: Windows
updatedAt: Sun Sep 29 2019 08:17:53 GMT+0800 (CST)
---
# 播放音效/音乐混音
## 功能描述
在通话或直播过程中，除了用户自己说话的声音，有时候需要播放自定义的声音或者音乐文件并且让频道内的其他人也听到，比如需要给游戏添加音效，或者需要播放背景音乐等，Agora 提供以下两组方法可以满足播放音效和音乐文件的需求。

开始前请确保已在你的项目中实现基本的实时音视频功能。 详见[开始音视频通话](../../cn/Video/start_call_windows.md)或[开始互动直播](../../cn/Video/start_live_windows.md)。

## 播放音效文件

音效通常指持续很短的音频。播放音效文件方法主要用来播放短小的音效，比如鼓掌、游戏子弹撞击声音等，可以多个音效叠加播放。
音效由音频文件路径指定，但在 SDK 内部使用 sound id 来识别和处理音效。SDK 并不强制如何定义 sound id，保证每个音效有唯一的识别即可。一般的做法有自增 id，使用音效文件名的 hashCode 等。

### 实现方法

```c++
// 初始化参数对象
RtcEngineParameters rep(*lpAgoraEngine);

// 预加载音效（推荐），需注意音效文件的大小，并在加入频道前完成加载
#ifdef UNICODE
  CHAR wdFilePath[MAX_PATH];
  ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
  int nRet = rep.preloadEffect(nSoundID, wdFilePath);
#else
  int nRet = rep.preloadEffect(nSoundID, filePath);
#endif

// 开始播放音效文件，如果设置了预加载，需要指定 nSoundID 
#ifdef UNICODE
  CHAR wdFilePath[MAX_PATH];
  ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
  int nRet = rep.playEffect(nSoundID, // 音效唯一标识
  wdFilePath, // 文件路径
  nLoopCount, // 重复播放次数
  dPitch, // 音效的音调
  dPan, // 音效的空间位置，0 表示正前方
  nGain, // 音效音量，取值 0 - 100， 100 代表原始音量
  TRUE // 是否令远端也能听到音效的声音
#else
  int nRet = rep.playEffect(nSoundID, filePath, nLoopCount, dPitch, dPan, nGain, TRUE);
#endif

// 暂停指定的音效播放
int nRet = rep.pauseEffect(nSoundID);

// 暂停所有音效播放
int nRet = rep.pauseAllEffects();

// 继续指定的已经暂停的音效播放
int nRet = rep.resumeEffect(nSoundID);

// 继续所有已经暂停的音效播放
int nRet = rep.resumeAllEffects();

// 停止指定的音效播放
int nRet = rep.stopEffect(nSoundID);

// 停止所有音效播放
int nRet = rep.stopAllEffects();

// 释放预加载的音效
int nRet = rep.unloadEffect(nSoundID);
```

### API 参考

* [`preloadEffect`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a61e4eac3b78f2774ef1b22d69bd4e166)
* [`playEffect`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a26307c09cbbaecee3bd662294a935821)
* [`pauseEffect`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a75fc09bdd0bd8b2bfe9c47770eb1e928)
* [`pauseAllEffects`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a98ff58bdd2b8683bd27a1f75694641dc)
* [`resumeEffect`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#adae083a10afd4b316a2071ba8d01ff80)
* [`resumeAllEffects`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a66dd1578478dd3ca163768d1314cd50a)
* [`stopEffect`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#ab0520529fe0ca4eb56d75ff4468e4a03)
* [`stopAllEffects`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a7f742bd2262899a90f4a36205995419e)
* [`unloadEffect`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#afd2cc4d59101cef1b5dc9296e604d047)

### 开发注意事项

- 预加载不是一个必须的步骤，一般来说为了提高性能或者需要反复播放某个特定的音效的时候，我们建议使用预加载。但如果音效文件较大，不建议预加载。
- 以上方法都有返回值，返回值小于 0 表示方法调用失败。

## 音乐混音

混音是指播放本地或者在线音乐文件，同时让频道内的其他人听到此音乐。混音方法主要用来播放比较长的背景音，比如直播的时候播放的音乐，同时只可以有一个文件播放。如果在混音播放第一个文件的过程中播放第二个文件，会自动停止第一个文件的播放。

Agora 混音功能支持如下设置：

- 混音或替换： 混音指的是音乐文件的音频流跟麦克风采集的音频流进行混音（叠加）并编码发送给对方；替换指的是麦克风采集的音频被音乐文件的音频流替换掉，对方只能听见音乐播放。
- 循环：可以设置是否循环播放混音文件，以及循环次数。

### 实现方法

```c++
LPCTSTR filePath = "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3";

// 初始化参数对象
RtcEngineParameters rep(*lpAgoraEngine);

// 开始播放混音
#ifdef UNICODE
 CHAR wdFilePath[MAX_PATH];
 ::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
int nRet = rep.startAudioMixing(wdFilePath, // 混音文件路径，支持网络文件，比如 http 协议的
 FALSE, // 只在本端播放
  TRUE, // 混音文件内容替换麦克风采集的声音
  1 // 混音文件重复播放次数
  );
#else
int nRet = rep.startAudioMixing(filePath, FALSE, TRUE, 1);
#endif

// 结束播放混音
int nRet = rep.stopAudioMixing();
```

### API 参考

- [`startAudioMixing`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a13106dd42b618ab9d1a03f7ea1bc4f2f)
- [`stopAudioMixng`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1e7955a19257fe8388f79213a1b7ad5b)

### 开发注意事项

- 在频道内调用混音方法，否则会有潜在问题。
- 以上方法都有返回值，返回值小于 0 表示方法调用失败。
