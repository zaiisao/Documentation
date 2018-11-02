
---
title: 切换用户角色
description: web平台上设置或切换用户角色
platform: Web
updatedAt: Thu Nov 01 2018 08:13:31 GMT+0000 (UTC)
---
# 切换用户角色
# 切换用户角色
Web 平台通过如下 3 个接口控制用户角色的切换：

- `client.publish`：发布本地音视频流

	```javascript
	client.publish(localStream, function (err) {
		console.log("Publish local stream error: " + err);
	});

	client.on('stream-published', function (evt) {
		console.log("Publish local stream successfully");
	});
	```

- `client.unpublish`：取消发布本地音视频流

	```javascript
	client.unpublish(localStream, function (err) {
		console.log("Unpublish local stream error: " + err);
	});

	client.on('stream-unpublished', function (evt) {
		console.log("Unpublish local stream successfully");
	});
	```

- `client.subscribe`：订阅远端音视频流

	```javascript
	client.on('stream-added', function (evt) {
		var stream = evt.stream;
		console.log("New stream added: " + stream.getId());

		client.subscribe(stream, function (err) {
			console.log("Subscribe stream failed", err);
		});
	});
	client.on('stream-subscribed', function (evt) {
		var remoteStream = evt.stream;
		console.log("Subscribe remote stream successfully: " + stream.getId());
		stream.play('agora_remote' + stream.getId());
	})
	```

各接口与角色切换关系如下：

- 设置用户角色为主播：
 
   *  `client.publish`
   *  `client.subscribe`
 
- 用户角色由主播切换为观众：

  * `client.unpublish`

- 设置用户角色为观众：

  * `client.subscribe`
  
- 用户角色由观众切换为主播：

  *  `client.publish`


> 在调用 `client.publish` 发布本地音视频流前，需要先创建并初始化音视频流，详见 [发布或订阅音视频流](../../cn/Interactive%20Broadcast/publish_web.md) 中的描述。




