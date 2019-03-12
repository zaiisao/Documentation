
---
title: 音乐混音
description: How to enable audio mixing on the Web
platform: Web
updatedAt: Tue Mar 12 2019 07:59:37 GMT+0000 (UTC)
---
# 音乐混音
## 功能描述
混音是指播放本地或者在线音乐文件，同时让频道内的其他人听到此音乐。混音方法主要用来播放比较长的背景音，比如直播的时候播放的音乐，同时只可以有一个文件播放。如果在混音播放第一个文件的过程中播放第二个文件，会自动停止第一个文件的播放。
Agora 混音功能支持如下设置：

- 混音或替换： 混音指的是音乐文件的音频流跟麦克风采集的音频流进行混音（叠加）并编码发送给对方；替换指的是麦克风采集的音频被音乐文件的音频流替换掉，对方只能听见音乐播放。
- 循环：可以设置是否循环播放混音文件，以及循环次数。

## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端 ](../../cn/Audio%20Broadcast/web_prepare.md)。

```javascript
// 设置混音选项
// filePath 必填，设置混音文件路径，仅支持在线文件。
// cycle 选填，设置音频文件循环播放的次数，仅支持 Chrome 65+。如不设置则默认播放一次。 
// replace 选填，设置是否用音频文件内容替换本地麦克风采集的音频流。默认为 false。
// playTime 必填，设置音频文件开始播放的位置，单位为 ms。设为 0 即从头开始播放。 
var options = {
      filePath: "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3", 
      cycle: 1, 
      replace: false, 
      playTime:0 
}

// 开始混音
localStream.startAudioMixing(options, function(err){
     if (err){
             console.log("Failed to start audio mixing. " + err);
      }
});

// 调整混音的音量。取值为 [0, 100]，100 代表维持原来混音的音量（默认）。
localStream.adjustAudioMixingVolume(volume);

// 获取当前播放的混音音乐的位置，返回值单位为 ms。
localStream.getAudioMixingCurrentPosition();

// 设置混音文件开始播放的位置，单位为 ms。
localStream.setAudioMixingPosition(pos);

// 暂停播放混音文件
localStream.pauseAudioMixing();

// 恢复播放混音文件
localStream.resumeAudioMixing();

// 获取当前混音的播放进度，返回值单位为 ms。
localStream.getAudioMixingCurrentPosition();

// 获取混音文件播放时长，返回值单位为 ms。
localStream.getAudioMixingDuration();

// 停止混音
localStream.stopAudioMixing();
```

### API 参考

- [`startAudioMixing`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing)
- [`stopAudioMixing`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#stopaudiomixing)
- [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#adjustaudiomixingvolume)
- [`pauseAudioMixing`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#pauseaudiomixing)
- [`resumeAudioMixing`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#resumeaudiomixing)
- [`getAudioMixingDuration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getaudiomixingduration)
- [`getAudioMixingCurrentPosition`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getaudiomixingcurrentposition)
- [`setAudioMixingPosition`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudiomixingposition)

## 开发注意事项

- 仅支持在线音频文件混音
- 混音方法支持以下浏览器：
  - Safari 12+
  - Chrome 65+
  - 最新版 Firefox
- 请在频道内调用该方法，如果在频道外调用该方法可能会出现问题。
- 部分参数选项只有特定浏览器支持，详见 [startAudioMixing](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing)。




