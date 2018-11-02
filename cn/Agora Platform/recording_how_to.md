
---
title: 录制相关
description: 
platform: 录制相关
updatedAt: Fri Nov 02 2018 04:06:32 GMT+0000 (UTC)
---
# 录制相关
### 录制完成动作如何捕获

自动模式录制：如果频道内没有人，当idle时间到，则停止录制，然后leaveChannel()离开频道。应用侧监控leaveChannel()则表示录制完成，可以转入下一步处理逻辑：例如录制完成后把录制文件上传到其他服务器上等。

### 录制文件保存目录及文件如何自定义

可以自定义，需要单独配置cfg文件。SDK包中Agora_Recording_SDK_for_Linux_FULL/samples/cpp 下面有一个隐藏文件.cfg.json，可以参考设置输出文件路径，文件名的方法。
具体链接：https://docs.agora.io/cn/Recording/recording_cpp?platform=C++

例如：目录文件名需要按照“根目录/年月日/channelId/uid.aac”方式保存： 按照如下方式配置/home/Agora_Recording_SDK_for_Linux_FULL/samples/recording_outpu。
