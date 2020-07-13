
---
title: 处理浏览器的自动播放策略
description: 
platform: Web
updatedAt: Fri Jul 10 2020 07:38:48 GMT+0800 (CST)
---
# 处理浏览器的自动播放策略
## 概览

浏览器为了防止网页在用户非自愿的情况下主动播放声音，对网页上的自动播放（Autoplay）功能做了限制：浏览器在**没有用户交互操作之前不允许有声音的媒体播放**。

这个限制是出于用户体验的考虑，因为通常情况下用户访问网页后突然自动播放音频可能是违背用户意愿的。

结合 Agora Web SDK 的具体使用来说，只要调用 `Stream.play` 播放的流对象中包含音频且未静音，都会受到浏览器自动播放策略的限制。

为了解决由于浏览器限制导致播放失败的问题，本文将分两种情况介绍绕过 Autoplay 限制的方案：

- 播放失败时绕过 Autoplay 限制。
- 直接绕过 Autoplay 限制。

<div class="alert note">本文仅适用于 Agora <a href="https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#rtc-sdk">RTC SDK</a> 的 Web 平台。</div>

## 播放失败时绕过 Autoplay 限制

SDK 的 `Stream.play("ID")` 方法在检测到因为 Autoplay 限制导致无法播放时会在错误回调中携带错误。我们可以利用这个方法，当检测到播放失败时执行绕过 Autoplay 限制的逻辑。因为页面不是 100% 被 Autoplay 限制，随着用户使用这个页面的次数增加，浏览器会把这个页面加入自己的 Autoplay 白名单列表中。

当检测到播放失败时，引导用户点击页面上的某个位置来恢复播放。

```javascript
stream.play("agora_remote"+ stream.getId(), function(err){
        if (err && err.status !== "aborted"){
               // 播放失败，一般为浏览器策略阻止。引导用户用手势触发恢复播放            
               document.querySelector("#agora_remote"+ stream.getId()).onclick=function(){
                        stream.resume().then(
                        function (result) {
                              console.log('恢复成功：' + result);
                        }).catch(
                        function (reason) {
                              console.log('恢复失败：' + reason);
                       });
               }      
        }
});
```

一般来说本地流不会有 Autoplay 限制（因为不会播放声音），所以只需要对远端流处理即可。

## 直接绕过 Autoplay 限制

如果不希望手动执行 `Stream.resume` 这个操作，想直接绕过 Autoplay 限制主要有两种方案：

- 方案一：通过 `Stream.play("ID", { muted: true })` 播放。如果媒体不包含声音，则不会被 Autoplay 限制。

- 方案二：通过 UI 和产品设计让用户和页面发生交互操作（点击/触摸），在用户操作后再调用 `Stream.play` 进行播放。

如果你的产品设计要求在没有用户操作的前提下自动播放媒体，我们推荐使用方案一。

如果你的产品设计允许在播放媒体之前用户和页面产生某些交互，比如用户在进入直播间后需要点击某些按钮才会开始订阅主播，我们推荐使用方案二。

<div class="alert note">无论使用何种方案，在自动播放策略限制下，没有用户操作之前自动播放有声媒体都是不可能的。虽然浏览器会在本地维护一个白名单来决定对哪些网站解除自动播放限制，但这部分是无法用 Javascript 探测到的。</div>

### 方案一：通过 `muted: true` 绕过 Autoplay 限制

使用这个方案，媒体会首先通过静音的方式自动播放，等页面上出现任何交互操作时，再自动切成有声媒体的播放。

主要步骤如下：

1. 在代码的开始，我们在页面上注册一个全局的事件：

    ```javascript
    document.body.addEventListener("touchstart") // 监听触摸操作
    ```
    或者
    ```javascript
    document.body.addEventListener("mousedown") // 监听点击操作
    ```
    
2. 通过 `createStream` 或者订阅远端的方式获取到可以播放的 `Stream` 对象后，立刻调用 `Stream.play("ID", { muted: true })` 自动以静音的方式播放这些流。同时将这些自动播放的 `Stream` 对象保存到一个内部列表对象 `playingStreamList` 中。

3. 在第一步注册的事件回调中，将 `playingStreamList` 中的 `Stream` 对象再次播放，这次就无须设置 `muted` 了。

    ```javascript
    Stream.stop()
    Stream.play("ID", { muted: false })
    ```

### 方案二：通过提前产生交互行为绕过 Autoplay 限制

简单来说，只要确保在调用 `Stream.play` 之前用户和页面发生过交互行为即可。对于桌面端的浏览器，这个方案可以绕过大部分 Autoplay 限制，但是在 iOS Safari/Webview 上，自动播放策略会更为严格。

### iOS Safari/WebView 的特殊处理

<div class="alert note">iOS Safari 只允许通过用户交互来<b>触发</b>有声媒体的播放，而不是在用户交互后就打开 Autoplay 限制。</div> 

如果你的应用需要兼容 iOS Safari/WebView，我们推荐对 iOS Safari/WebView 做特殊处理。

- 和方案一一样，所有的流自动播放时都设置 `muted: true。`
- 在每个 `Stream.play` 时指定的 `HTMLElement` 容器绑定交互事件（点击/触摸）。
- 在播放界面上通过图标显示当前流被静音，引导用户点击。
- 当用户点击某个流的播放界面（刚刚绑定的容器）时，在事件的回调中将这个流再次播放，此时无需设置静音，同时更改静音图标。   

    ```javascript
    Stream.stop()
    Stream.play("ID", { muted: false })
    ```
