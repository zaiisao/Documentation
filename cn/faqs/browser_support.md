
---
title: Agora Web SDK 支持哪些浏览器？
description: 
platform: Web
updatedAt: Tue Jun 02 2020 11:51:55 GMT+0800 (CST)
---
# Agora Web SDK 支持哪些浏览器？
Agora Web SDK 支持所有主流浏览器，支持的浏览器及平台如下。

<table>
  <tr>
    <th>平台</th>
    <th>Chrome 58+</th>
    <th>Firefox 56+</th>
    <th>Safari 11+</th>
    <th>Opera 45+</th>
    <th>QQ 浏览器 10.5+</th>
    <th>360 安全浏览器</th>
    <th>微信浏览器</th>
  </tr>
  <tr>
    <td>Android 4.1+</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
		<td><b>N/A</b></td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>iOS 11+</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>macOS 10+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>Windows 7+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
		<td><b>N/A</b></td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
  </tr>
</table>

<div class="alert info">除上表浏览器外，还有以下支持：
	<li>Agora Web SDK 2.5 及以上版本支持 Windows XP 平台的 Chrome 49 版本浏览器（仅支持 VP8 编解码，不能与 Native SDK 互通）。</li>
	<li>Agora Web SDK 2.7 及以上版本支持 Windows 10 平台的 Edge 浏览器，详见 <a href="https://docs.agora.io/cn/faq/browser_support#a-nameedgeaedge">Edge 浏览器支持</a>。</li>
	<li>Agora Web SDK 理论上还支持 360 极速浏览器，但未经过验证，不保证全部功能正常工作。</li>
</div>
<div class="alert note">以下场景中请务必将 Agora Web SDK 升级至 2.6 或更高版本:
	<li>iOS 12.1.4 及以上版本使用 Safari 浏览器</li>
	<li>macOS 上使用 Safari 12.1 及以上版本</li>
	</div>

由于浏览器的差异，在不同浏览器及平台上支持的功能可能不同。下面列出浏览器和平台的已知限制。

## 使用限制

Chrome 81 及以上版本、Safari 和 Firefox 浏览器需要在获得媒体设备权限后才能获取设备 ID，详见[为什么在 Chrome 81 浏览器上无法获取设备 ID？](../../cn/faq/empty_deviceId.md)

### Chrome

Agora Web SDK 是基于 WebRTC 实现的采集和编解码，而 Chrome 又是第一批支持 WebRTC 的先行者，所以在 Chrome 上的限制最少，已知限制：

- Chrome 版本要求 58 及以上。
- 部分 Android 设备上，Chrome 不支持 H.264 编解码格式。
- 部分 API 需要 Chrome 更高版本支持，具体见 API 参考内的描述。
- 在所有使用 AMD 芯片和部分使用 Intel 芯片的 Windows 设备上，Chrome 使用 H.264 编码时，发送码率可能达不到设定值。你可以使用 VP8 编码或者尝试关闭硬件加速。

### Safari

- Safari 11 只支持 480P 及以上分辨率。
- Safari 12.1 及之前版本仅支持 H.264 编解码格式。
- 设备权限
  - Safari 无法获取输出设备信息，因此不支持 `getPlayoutDevices` 和 `setAudioOutput` 这两个方法。
  - 如果 Safari 浏览器没有打开自动播放，直接播放音视频流会听不到声音，必须在播放前调用 `navigator.mediaDevices.getUserMedia` 方法获取设备权限。
    ![](https://web-cdn.agora.io/docs-files/1591069399605)
- Safari 不支持 `addTrack` 和 `removeTrack`。
- Safari 不支持[双流模式](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#dual-stream)。
- iOS 端 Safari 不支持 `setAudioLevel` 方法。
- iOS 端 Safari 上存在语音路由问题：可能出现插着耳机但是仍然从扬声器出声，或者没有耳机却从听筒出声的情况。

### Firefox 

- 如果 Web 端使用 Firefox 浏览器，Native 端使用 iOS 设备，Firefox 看到的视频方向会发生旋转。
- Firefox 只支持视频帧率设为 30 fps。
- 在部分设备上 Firefox 设置视频编码配置不生效，目前已知有此问题的设备如下：
  - MacBook Pro (13-inch, 2016, Two Thunderbolt 3 ports)
  - Windows 10 (MI)

### <a name="edge"></a>Edge

Agora Web SDK 2.7 及以上版本支持 Edge 浏览器。受浏览器自身限制，仅支持以下功能：

- 与 Agora Native/Web SDK 音视频互通
- 调用 `getStats` 方法获取音视频流的连接数据（受浏览器更新的影响，可能存在部分字段缺失的情况）
- 调用 `getAudioLevel` 方法获取当前音量
- 调用 `muteAudio`/`unmuteAudio` 方法禁用/启用音频轨道
- 调用 `muteVideo`/`unmuteVideo` 方法禁用/启用视频轨道
- 调用 `setVideoProfile` 方法设置视频属性

## 相关链接
[移动端如何使用 Agora Web SDK？](https://docs.agora.io/cn/faq/web_on_mobile)
