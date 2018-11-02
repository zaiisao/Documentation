
---
title: 推流到 CDN
description: 
platform: Windows
updatedAt: Sat Sep 29 2018 04:03:20 GMT+0000 (UTC)
---
# 推流到 CDN
# 推流到 CDN

## 简介

直播场景下，你有两种方式进行推流:

-   服务器端推流

-   客户端推流


本页仅介绍服务器端 CDN 推流。客户端推流场景需要由您自行实现或者采用第三方 SDK。您也可以联系 [sales@agora.io](mailto:sales@agora.io) 获取最新的示例代码。

## 服务器端推流

当启用服务器端推流服务后，当第一个主播加入频道时触发推流，当频道内没有任何主播流超过 30s 时，推流结束。

按照以下步骤在 Agora 服务器端推流:

1.  联系 [sales@agora.io](mailto:sales@agora.io) 开通推流功能。后续将在 Dashboard 提供自助服务。

2.  调用 `configPublisher` 在服务器端进行推流。


## 设置服务器端推流合图的布局

声网提供两种方式调整合图 \(画中画\) 布局:

-   自由调整布局

-   默认布局


### 自由调整布局

你通过调用 API 自由调整画中画布局，灵活地将任何主播视频以任何尺寸放置在屏幕的任何位置。

1.  联系 [sales@agora.io](mailto:sales@agora.io), 申请打开以下功能:

    1.  打开 Agora 服务器端推流的功能。

    2.  打开动态调整画中画布局功能。

2.  等待 agora.io 的确认消息，确认该功能已打开。

3.  根据实际需要，调用 `setVideoCompositingLayout` 调节画中画布局:

> -   请确保同一频道内仅有一位用户调用该 API，如有多人调用设置画中画布局，其他用户必须调用 `clearVideoCompositingLayout` 取消设置
> 
> -   如果该 API 没有正确调用，将导致旁路直播过程中以及录制的旁路直播内容只有声音，没有视频


#### 示例 1: 单主播全屏

如果你想显示以下布局:

<img alt="../_images/sei_1host.png" src="https://web-cdn.agora.io/docs-files/cn/sei_1host.png" style="width: 236.0px; height: 416.0px;"/>


设置参数如下:

```
Canvas:360*640, #FFFFFF
UserList
   User1 :
       userId: 123
       renderRegion: 0.0, 0.0, 1.0, 1.0
       alpha: 1.0
       zorder: 0
       renderMode: Cropped
```

#### 示例 2: 两人横向平铺

如果你想显示以下布局:

<img alt="../_images/sei_2host.png" src="https://web-cdn.agora.io/docs-files/cn/sei_2host.png" style="width: 416.0px; height: 240.0px;"/>


设置参数如下:

```
Canvas: 640 * 360,  #FFFFFF

  UserList
  User1 :
      userId: 123
      renderRegion: 0.0, 0.0, 0.5, 1.0
      alpha: 1.0
      zorder: 0
      renderMode: Cropped
  User2:
      userId: 456
      renderRegion: 0.5, 0.0, 0.5, 1.0
      alpha: 1.0
      zorder: 0
      renderMode: Cropped
```

#### 示例 3: 三人纵向平铺

如果你想显示以下布局:

<img alt="../_images/sei_3host.png" src="https://web-cdn.agora.io/docs-files/cn/sei_3host.png" style="width: 236.0px; height: 416.0px;"/>


设置参数如下:

```
Canvas: 360 * 640,  #FFFFFF

UserList
   User1 :
       userId: 123
       renderRegion: 0.0, 0.0, 1.0, 0.6
       alpha: 1.0
       zorder: 0
       renderMode: Cropped
   User2:
       userId: 456
       renderRegion: 0.0, 0.6, 0.5, 0.4
       alpha: 1.0
       zorder: 0
       renderMode: Cropped
   User3:
       userId: 789
       renderRegion: 0.5, 0.6, 0.5, 0.4
       alpha: 1.0
       zorder: 0
       renderMode: Cropped
```

#### 示例 4: 1 人全屏 + 多人任意悬浮小窗

如果你想显示以下布局:

<img alt="../_images/sei_random.png" src="https://web-cdn.agora.io/docs-files/cn/sei_random.png" style="width: 236.0px; height: 416.0px;"/>


设置参数如下:

```
Canvas: 360 * 640,  #FFFFFF
UserList

   User1:
       userId: 123
       renderRegion: 0.0, 0.0, 1.0, 1.0
       alpha: 1.0
       zorder: 0
       renderMode: Cropped

   User2:
       userId: 456
       renderRegion: 0.1, 0.5, 0.3, 0.4
       alpha: 1.0
       zorder: 100
       renderMode: Cropped

   User3:
       userId: 789
       renderRegion: 0.5, 0.5, 0.3, 0.4
       alpha: 1.0
       zorder: 100
       renderMode: Cropped
```

### 默认布局

#### 层叠窗口

悬浮视窗: 1 主窗 + N 悬浮小窗 \( N 最大为 6 \)

例如:

<img alt="../_images/sei_random.png" src="https://web-cdn.agora.io/docs-files/cn/sei_random.png" style="width: 236.0px; height: 416.0px;"/>


#### 横向平铺视窗

横向分割: 两窗横向平铺

例如:

<img alt="../_images/sei_2host.png" src="https://web-cdn.agora.io/docs-files/cn/sei_2host.png" style="width: 416.0px; height: 240.0px;"/>


#### 纵向平铺视窗

纵向分割: 1 上窗+ 2 下窗 \(上方占 60%，下方占 40% \(左右对半分\)）

例如:

<img alt="../_images/sei_3host.png" src="https://web-cdn.agora.io/docs-files/cn/sei_3host.png" style="width: 236.0px; height: 416.0px;"/>



