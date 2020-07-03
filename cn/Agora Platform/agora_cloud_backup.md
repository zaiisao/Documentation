
---
title: Agora 云备份 (Agora Cloud Backup)
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 10:11:54 GMT+0800 (CST)
---
# Agora 云备份 (Agora Cloud Backup)
Agora 云备份是云端录制使用的备份云存储服务，当录制服务无法将录制文件上传至开发者指定的第三方云存储时，会自动将录制文件暂存至备份云。当上传恢复后，录制服务会将录制文件转存至第三方云存储，并从备份云中删除。

录制文件在备份云中最多存储 10 天，10 天后会被自动删除。开发者可以通过云端录制 RESTful API 回调获知录制文件是否被上传至 Agora 云备份。

<div class="alert info">相关链接：<a href="https://docs.agora.io/cn/cloud-recording/cloud_recording_callback_rest?platform=All%20Platforms">云端录制 RESTful API 回调服务</a>
</div>

<a href="../../cn/Agora%20Platform/terms.md"><button>返回术语库</button></a>
