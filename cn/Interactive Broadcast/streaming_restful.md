
---
title: 推流 RESTful API
description: 
platform: All Platforms
updatedAt: Tue Mar 30 2021 02:46:35 GMT+0800 (CST)
---
# 推流 RESTful API
<div class="alert note"><p>本服务在 2021 年 5 月 1 日前免费。</p>
本服务目前处于 Beta 阶段，可能存在一定程度的可用性风险。为保障服务质量，Agora 强烈推荐你在中国大陆地区使用本服务：
<li>请确保发起 HTTP 请求的业务服务器部署在中国大陆。</li>
<li>请确保创建的 Converter 位于中国大陆。</li></div>

## 简介

你可以使用推流 RESTful API 将 Agora 频道内的音视频流由 Agora 私有协议转换为标准协议（如 RTMP），然后推到 CDN（Content Delivery Network）。这个过程相当于 Agora 通过一个 Converter 将音视频流处理后输出并推到 CDN。

## 工作原理

你可以通过如下方法控制 Converter：

- `Create`：为指定的项目创建一个 Converter，并设置相关配置。频道内用户音视频流会按照指定的配置经过 Converter 处理后输出推到 CDN。
- `Delete`：销毁指定的 Converter。销毁该 Converter 后，此频道内用户音视频流将不再经过 Converter 处理并输出推到 CDN。
- `Update`：更新指定的 Converter 配置。

在推流 RESTful API 中，Converter 通过 `converter` 字段设置，字段结构如图所示：
字段含义详见下表：

| 字段 | 类型 | 描述  |
|---|----|---|
|id   | String | Converter 的 ID。它是 Agora 服务器生成的一个 UUID（通用唯一识别码），标识一个已创建的 Converter。|
|name  | String，可选| Converter 的名字。长度必须在 64 个字符以内，支持的字符集范围为：<li>所有小写英文字母（a-z）</li><li>所有大写英文字母（A-Z）</li><li>数字 0-9</li><li>"-", "_"</li>不传值时该字段为空。同一时间，一个项目下可以有多个 `name` 为空的 Converter。同一时间，一个项目下不允许出现重名的 Converter，否则在尝试创建同名的 Converter 时会收到响应状态码 409(Conflict)。<div class="alert note">为避免重复创建多个 Converter 重复推流，请使用 name 字段管理指定项目下的 Converter。Agora 推荐你以“频道名（rtcChannel）”和 “Converter 特性”组合来给 `name` 赋值。如 `show68_horizontal` 和 `show68_vertical`，分别代表在频道 `show68` 中创建的用户画面为水平布局和和垂直布局的 Converter。</div>|
|transcodeOptions| JSON Object| Converter 的转码配置。|
|transcodeOptions.rtcChannel|String|Agora 频道名称。|
|transcodeOptions.audioOptions|JSON Object|Converter 的音频转码配置。|
|transcodeOptions.audioOptions.codecProfile|String|Converter 输出的音频编解码器。|
|transcodeOptions.audioOptions.sampleRate|Number|Converter 输出的音频编码采样率 (Hz)。|
|transcodeOptions.audioOptions.bitrate|Number|Converter 输出的音频编码码率 (Kbps)。|
|transcodeOptions.audioOptions.audioChannels|Number| Converter 输出的音频声道数。|
|transcodeOptions.audioOptions.rtcStreamUids|JSON Array|参与混音的用户 UID/Account。|
|transcodeOptions.videoOptions|JSON Object|Converter 的视频转码配置。|
|transcodeOptions.videoOptions.canvas|JSON Object |视频画布。|
|transcodeOptions.videoOptions.canvas.width|Number|画布的宽度 (pixel)。|
|transcodeOptions.videoOptions.canvas.height|Number|画布的高度 (pixel)。|
|transcodeOptions.videoOptions.canvas.color|Number|画布的背景色。RGB 颜色值，以十进制数表示。如 255 代表蓝色。|
|transcodeOptions.videoOptions.layout|JSON Array|画布上视频画面的内容描述。支持 RtcStreamView 和 ImageView 两种元素。</li>|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素|无|画布上各用户的视频画面。|
|`transcodeOptions.videoOptions.layout.RtcStreamView 元素.rtcStreamUid|Number|视频流所属用户的 UID。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.rtcStreamAccount|String|视频流所属用户的 Account。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region|JSON Object|用户视频画面在画布上的显示区域。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region.xPos|Number|画面在画布上的 x 坐标 (pixel)。以画布左上角为原点，x 坐标为画面左上角相对于原点的横向位移。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region.yPos|Number|画面在画布上的 y 坐标 (pixel)。以画布左上角为原点，y 坐标为画面左上角相对于原点的纵向位移。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region.zIndex|Number|画面的图层编号。取值范围为 [0,100]。`0` 代表最下层的图层。`100` 代表最上层的图层。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region.width|Number|画面的宽度 (pixel)。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region.height|Number| 画面的高度 (pixel)。 |
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.placeholderImageUrl|String|替代图片的 HTTP(s) URL。|
|transcodeOptions.videoOptions.layout.ImageView 元素|无| 画布上的视频图片，可用于当水印。 |
|transcodeOptions.videoOptions.layout.ImageView 元素.imageUrl|String| 图片的 HTTP(S) URL。 |
|transcodeOptions.videoOptions.layout.ImageView 元素.region|JSON Object| 图片在画布上的显示区域。|
|transcodeOptions.videoOptions.layout.ImageView 元素.region.xPos|Number|图片在画布上的 x 坐标 (pixel)。以画布左上角为原点，x 坐标为图片左上角相对于原点的横向位移。|
|transcodeOptions.videoOptions.layout.ImageView 元素.region.yPos|Number|图片在画布上的 y 坐标 (pixel)。以画布左上角为原点，y 坐标为图片左上角相对于原点的纵向位移。|
|transcodeOptions.videoOptions.layout.ImageView 元素.region.zIndex|Number|图片的图层编号。取值范围为 [0,100]。`0` 代表最下层的图层。`100` 代表最上层的图层。|
|transcodeOptions.videoOptions.layout.ImageView 元素.region.width|Number|图片的宽度 (pixel)。|
|transcodeOptions.videoOptions.layout.ImageView 元素.region.height|Number|图片的高度 (pixel)。|
|transcodeOptions.videoOptions.bitrate|Number|输出视频的编码码率 (Kbps)。|
|transcodeOptions.videoOptions.frameRate|Number| 输出视频的编码帧率 (fps)。|
|transcodeOptions.videoOptions.codecProfile|String|输出视频的编码规格。|
|transcodeOptions.videoOptions.seiExtraInfo|String|输出视频中携带的用户 SEI 信息。用于向 CDN 发送用户自定义的 SEI 信息。长度限制 4096 字节。默认为空。|
|rtmpUrl|String|CDN 推流地址。|
|idleTimeOut|Number|Converter 处于空闲状态的最大时长（秒）。空闲指 Converter 处理的音视频流所对应的所有用户均已离开频道。取值范围为 5 到 600。默认值为 300。取值超出范围会报错。|
|createTs|Number|创建 Converter 时的 Unix 时间戳（秒）。|
|updateTs|Number|最近一次更新 Converter 配置时的 Unix 时间戳（秒）。|
|state|String|Converter 的运行状态：<li>`idle`: 推流未开始或已结束。</li><li>`connecting`: 正在连接 Agora 推流服务器和 CDN 服务器。</li><li>`running`: 正在进行推流。</li><li>`recovering`: 正在恢复推流。</li><li>`failure`: 推流失败。</li>|


一个完整的 Converter 示例如下：

```json
{
    "id": "4c014467d647bb87b60b719f6fa57686",
    "name": "show68_vertical",
    "transcodeOptions": {
        "rtcChannel": "show68",
        "audioOptions": {
            "codecProfile": "HE-AAC",
            "sampleRate": 48000,
            "bitrate": 128,
            "audioChannels": 1,
            "rtcStreamUids": [
                201
            ]
        },
        "videoOptions": {
            "canvas": {
                "width": 360,
                "height": 640,
                "color": 0
            },
            "layout": [
                {   "rtcStreamUid": 201,
                    "region": {
                        "xPos": 0,
                        "yPos": 0,
                        "zIndex": 1,
                        "width": 360,
                        "height": 320
                    },
                    "placeholderImageUrl": "http://example.agora.io/host_placeholder.jpg"
                },
                {   "rtcStreamUid": 202,
                    "region": {
                        "xPos": 0,
                        "yPos": 320,
                        "zIndex": 1,
                        "width": 360,
                        "height": 320
                    }
                },
                {   "imageUrl": "http://example.agora.io/watchmark.jpg",
                    "region": {
                        "xPos": 0,
                        "yPos": 0,
                        "zIndex": 2,
                        "width": 36,
                        "height": 64
                    }
                }
            ],
            "codecProfile": "High",
            "frameRate": 15,
            "bitrate": 400,
            "seiExtraInfo": "xxxyyyzzz"
        }
    },
    "rtmpUrl": "rtmp://example.agora.io/live/show68",
    "idleTimeout": 300,
    "createTs": 1591786766,
    "updateTs": 1591786835,
    "state": "connecting"
}
```

### 前提条件

联系我们开通 **RTMP Converter** 服务。

## Create：创建一个 Converter

### HTTP 请求

```http
POST https://api.agora.io/v1/projects/<appId>/rtmp-converters
```

#### 路径参数

`appId`: String 型必填参数。Agora 为每个开发者提供的 **App ID**。在 Agora 控制台创建一个项目后即可得到一个 App ID。一个 App ID 是一个项目的唯一标识。

#### Query Parameters

`regionHintIp`: String 型可选参数。CDN 源站 IP 地址，必须为有效的 IPv4 地址。该参数可以保障 RTMP 流的稳定性。

```http
POST https://api.agora.io/v1/projects/<appId>/rtmp-converters?regionHintIp={regionHintIp}
```

#### 请求报文的 Header 字段

- `Content-Type`: `application/json`
- `Authorization`: 该字段的值需参考[认证说明](https://docs.agora.io/cn/faq/restful_authentication)。
- `X-Request-ID`: UUID（通用唯一识别码），标识本次**请求**。传入该字段后，Agora 服务器会在响应 header 中返回该字段。

   > Agora 推荐你对 `X-Request-ID` 赋值。如果不赋值，Agora 服务器会自动生成一个 UUID 传入。

#### 请求报文的 Body

该示例展示将 `show68` 频道内指定的两个用户合图后推到 CDN，并且频道内所有用户音频进行混音。

```json
{
    "converter": {
        "name": "show68_vertical",
        "transcodeOptions": {
            "rtcChannel": "show68",
            "audioOptions":{
                "codecProfile": "LC-AAC",
                "sampleRate": 48000,
                "bitrate": 48,
                "audioChannels": 1
            }
            "videoOptions": {
                "canvas": {
                    "width": 360,
                    "height": 640
                },
                "layout": [
                    {   "rtcStreamUid": 201,
                        "region": {
                            "xPos": 0,
                            "yPos": 0,
                            "zIndex": 1,
                            "width": 360,
                            "height": 320
                        },
                        "placeholderImageUrl": "http://example.agora.io/user_placeholder.jpg"
                    },
                    {   "rtcStreamUid": 202,
                        "region": {
                            "xPos": 0,
                            "yPos": 320,
                            "zIndex": 1,
                            "width": 360,
                            "height": 320
                        }
                    }
                ],
                "bitrate": 800
            }
        },
        "rtmpUrl": "rtmp://example.agora.io/live/show68"
    }
}
```

<a name="mean"></a>
请求 Body 为 JSON Object 类型的 `converter` 字段，字段结构如图所示：
字段含义详见下表：
<style> table th:first-of-type {     width: 80px; } th:third-of-type {     width: 80px; }</style>
| 字段 | 类型 | 描述  |
|---|----|---|
|name | String（可选） | Converter 的名字。长度必须在 64 个字符以内，支持的字符集范围为：<li>所有小写英文字母（a-z）</li><li>所有大写英文字母（A-Z）</li><li>数字 0-9</li><li>"-", "_"</li>不传值时该字段为空。同一时间，一个项目下可以有多个 `name` 为空的 Converter。同一时间，一个项目下不允许出现重名的 Converter，否则在尝试创建同名的 Converter 时会收到响应状态码 `409(Conflict)`。<div class="alert note">为避免重复创建多个 Converter 重复推流，请使用 `name` 字段管理指定项目下的 Converter。Agora 推荐你以“频道名（`rtcChannel`）”和 “Converter 特性”组合来给 `name` 赋值。如 `show68_horizontal` 和 `show68_vertical`，分别代表在频道 `show68` 中创建的用户画面为水平布局和和垂直布局的 Converter。</div>|
|transcodeOptions| JSON Object（必填）| Converter 的转码配置。<div class="alert note"><li>当 `converter.transcodeOptions` 中无 `audioOptions` 字段且有 `videoOptions` 字段时，Converter 输出纯视频流。</li><li>当 `converter.transcodeOptions.audioOptions` 字段中无 `rtcStreamUids` 字段时，Agora 会将频道内所有用户的音频流进行混音并通过 Converter 输出混音后的音频流。</li><li>当 `converter.transcodeOptions.audioOptions` 字段中有 `rtcStreamUids` 字段时，Agora 会将指定用户的音频流进行混音并通过 Converter 输出混音后的音频流。</li></div>|
|transcodeOptions.rtcChannel|String（必填）|Agora 频道名称。即 Converter 处理的流所属的频道。字符串长度必须在 64 字节以内，支持以下字符集（共 89 个字符）：<li>所有小写英文字母（a-z）</li><li>所有大写英文字母（A-Z）</li><li>数字 0-9</li><li>空格</li><li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ","</li>|
|transcodeOptions.audioOptions|JSON Object（可选）|Converter 的音频转码配置。|
|transcodeOptions.audioOptions.codecProfile|String（可选）|Converter 输出的音频编解码器。支持如下值：<li>`LC-AAC`（默认值）: MPEG-4 AAC LC。</li><li>`HE-AAC`: High-Efficiency AAC。</li>|
|transcodeOptions.audioOptions.sampleRate|Number（可选）|Converter 输出的音频编码采样率 (Hz)，可填 `32000`，`44100` 或 `48000`（默认值）。|
|transcodeOptions.audioOptions.bitrate|Number（可选）|Converter 输出的音频编码码率 (Kbps)取值范围为 [32,128]。默认值为 48。如果音频编解码器为 LC-AAC，推荐音频编码码率取值范围为 [32, 112]。如果音频编解码器为 HE-AAC，推荐音频编码码率取值范围为 [40, 96]。|
|transcodeOptions.audioOptions.audioChannels|Number（可选）| Converter 输出的音频声道数，可填 `1`（默认值） 或 `2`。|
|transcodeOptions.audioOptions.rtcStreamUids|JSON Array（可选）|参与混音的用户 UID/Account。默认值为空数组，代表 Agora 不对任何用户的音频流进行混音。|
|transcodeOptions.videoOptions|JSON Object（可选）|Converter 的视频转码配置。|
|transcodeOptions.videoOptions.canvas|JSON Object（必填）|视频画布。|
|transcodeOptions.videoOptions.canvas.width|Number（必填）|画布的宽度 (pixel)。取值范围为 [66,1920]。|
|transcodeOptions.videoOptions.canvas.height|Number（必填）|画布的高度 (pixel)。取值范围为 [66,1920]。|
|transcodeOptions.videoOptions.canvas.color|Number（可选）|画布的背景色。RGB 颜色值，以十进制数表示。如 255 代表蓝色。取值范围为 [0,16777215]。默认值为 0，即黑色。|
|transcodeOptions.videoOptions.layout|JSON Array（必填）|画布上视频画面的内容描述。支持 RtcStreamView 和 ImageView 两种元素。</li>|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素|无|画布上各用户的视频画面。<div class="alert note">Agora 支持频道内用户名为 Number 型的 UID 或 String 型的 Account。如果使用 String 型 Account，请确保已经阅读[如何使用 String 型用户名](https://docs.agora.io/cn/faqs/string)。</div>|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.rtcStreamUid|Number|视频流所属用户的 UID。请在 `rtcStreamUid` 和 `rtcStreamAccount` 中选择一个使用。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.rtcStreamAccount|String|视频流所属用户的 Account。请在 `rtcStreamUid` 和 `rtcStreamAccount` 中选择一个使用。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region|JSON Object（必填）|用户视频画面在画布上的显示区域。超出画布的视频画面会被裁剪，无法显示。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region.xPos|Number（必填）|画面在画布上的 x 坐标 (pixel)。以画布左上角为原点，x 坐标为画面左上角相对于原点的横向位移。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region.yPos|Number（必填）|画面在画布上的 y 坐标 (pixel)。以画布左上角为原点，y 坐标为画面左上角相对于原点的纵向位移。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region.zIndex|Number（必填）|画面的图层编号。取值范围为 [0,100]。`0` 代表最下层的图层。`100` 代表最上层的图层。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region.width|Number（必填）|画面的宽度 (pixel)。|
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.region.height|Number（必填）| 画面的高度 (pixel)。 |
|transcodeOptions.videoOptions.layout.RtcStreamView 元素.placeholderImageUrl|String（必填）|替代图片的 HTTP(s) URL。支持 JPG 和 PNG 格式的图片。当频道内用户不发布视频流时，如果设置了该字段，替代图片会替代该用户的视频画面，否则会显示该用户的最后一帧视频。|
|transcodeOptions.videoOptions.layout.ImageView 元素|无| 画布上的视频图片，可用于当水印。 |
|transcodeOptions.videoOptions.layout.ImageView 元素.imageUrl|String（必填）| 图片的 HTTP(S) URL。支持 JPG 和 PNG 格式的图片。 |
|transcodeOptions.videoOptions.layout.ImageView 元素.region|JSON Object（必填）| 图片在画布上的显示区域。超出画布的图片会被裁剪，无法显示。|
|transcodeOptions.videoOptions.layout.ImageView 元素.region.xPos|Number（必填）|图片在画布上的 x 坐标 (pixel)。以画布左上角为原点，x 坐标为图片左上角相对于原点的横向位移。|
|transcodeOptions.videoOptions.layout.ImageView 元素.region.yPos|Number（必填）|图片在画布上的 y 坐标 (pixel)。以画布左上角为原点，y 坐标为图片左上角相对于原点的纵向位移。|
|transcodeOptions.videoOptions.layout.ImageView 元素.region.zIndex|Number（必填）|图片的图层编号。取值范围为 [0,100]。`0` 代表最下层的图层。`100` 代表最上层的图层。|
|transcodeOptions.videoOptions.layout.ImageView 元素.region.width|Number（必填）|图片的宽度 (pixel)。|
|transcodeOptions.videoOptions.layout.ImageView 元素.region.height|Number（必填）|图片的高度 (pixel)。|
|transcodeOptions.videoOptions.bitrate|Number（必填）|输出视频的编码码率 (Kbps)。取值范围为 [1,10000]。详见[常用视频属性](https://docs-preview.agoralab.co/cn/rtmp-converter/rtmp_push_cdn_android?platform=Android#常用视频属性)。|
|transcodeOptions.videoOptions.frameRate|Number（可选）| 输出视频的编码帧率 (fps)。取值范围为 [1,30]。默认值为 15。详见[常用视频属性](https://docs-preview.agoralab.co/cn/rtmp-converter/rtmp_push_cdn_android?platform=Android#常用视频属性)。|
|transcodeOptions.videoOptions.codecProfile|String（可选）|输出视频的编码规格。输出视频的编码规格。支持如下值：<li>`high`（默认值）: High 级别的视频编码规格，一般用于广播及视频碟片存储，高清电视。</li><li>`baseline`: Baseline 级别的视频编码规格，一般用于低阶或需要额外容错的应用，比如视频通话、手机视频等。</li><li>`main`: Main 级别的视频编码规格，一般用于主流消费类电子产品，如 mp4、便携的视频播放器、PSP 和 iPad 等。</li>|
|transcodeOptions.videoOptions.seiExtraInfo|String（可选）|输出视频中携带的用户 SEI 信息。用于向 CDN 发送用户自定义的 SEI 信息。长度限制 4096 字节。默认为空。详见[声网 SEI 规范](https://docs.agora.io/cn/faq/sei?_ga=2.96778972.2136046418.1596424964-2007697402.1585210498#声网-sei-规范)。|
|rtmpUrl|String（必填）|CDN 推流地址。必须为有效的 RTMP 地址，且长度在 1024 个字符以内。|
|idleTimeOut|Number（可选）|Converter 处于空闲状态的最大时长（秒）。空闲指 Converter 处理的音视频流所对应的所有用户均已离开频道。空闲状态超过设置的 `idleTimeOut` 后，Converter 会自动销毁。|

### HTTP 响应

所有可能的响应状态码详见[状态码汇总表](#code)。

#### 响应报文的 Header 字段

- `X-Request-ID`：UUID（通用唯一识别码），标识本次**请求**。该值为本次请求 header 中 `X-Request-ID`。如果请求出错，请在日志中打印出该值，排查问题。

    > 如果本次请求的响应状态码不是 2XX，那么响应 header 中可能无该字段。

- `X-Resource-ID`：UUID（通用唯一识别码），标识本次请求创建的 Converter 的 **ID**。

#### 响应报文的 Body

```
{
    "converter": {
      "id": "4c014467d647bb87b60b719f6fa57686",
      "createTs": 1591786766,
      "updateTs": 1591786766,
      "state": "connecting"
    },
    "fields": "id,createTs,updateTs,state"
}
```

如果状态码为 2XX，请求成功。Body 中包含字段结构如图所示：
字段含义详见下表：

| 字段 | 类型 | 描述  |
|---|----|---|
|converter.id| String| Converter 的 ID。它是 Agora 服务器生成的一个 UUID（通用唯一识别码），标识一个已创建的 Converter。|
|converter.createTs| Number| 创建 Converter 时的 Unix 时间戳（秒）。|
|converter.updateTs| Number| 最近一次更新 Converter 配置时的 Unix 时间戳（秒）。|
|converter.state| String| Converter 的运行状态：<li>`idle`: 推流未开始或已结束。</li><li>`connecting`: 正在连接 Agora 推流服务器和 CDN 服务器。</li><li>`running`: 正在进行推流。</li><li>`recovering`: 正在恢复推流。</li><li>`failure`: 推流失败。</li>|
|fields| String| JSON 编码方式的字段掩码，详见[谷歌 protobuf FieldMask 文档](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#fieldmask)。用于描述返回的 `converter` 中包含的字段集合。在本示例中，`fields` 指定了 Agora 服务器返回 `converter` 字段中的 `id`，`createTs`，`updateTs` 和 `state` 字段子集。|


如果状态码不为 2XX，请求失败。Body 中包含 String 类型的 `message` 字段，描述失败的具体原因。

```json
{
    "message": "Invalid authentication credentials."
}
```

## Delete: 销毁指定的 Converter

### HTTP 请求

```http
DELETE https://api.agora.io/v1/projects/<appId>/rtmp-converters/<converterId>
```

#### 路径参数

- `appId`: String 型必填参数。Agora 为每个开发者提供的 **App ID**。在 Agora 控制台创建一个项目后即可得到一个 App ID。一个 App ID 是一个项目的唯一标识。
- `converterId`: String 型必填参数。Converter 的 **ID**。


#### Query Parameters

无

#### 请求报文的 Header 字段

- `Authorization`: 该字段的值需参考[认证说明](https://docs.agora.io/cn/faq/restful_authentication)。
- `X-Request-ID`: UUID（通用唯一识别码），标识本次**请求**。传入该字段后，Agora 服务器会在响应 header 中返回该字段。

   > Agora 推荐你对 `X-Request-ID` 赋值。如果不赋值，Agora 服务器会自动生成一个 UUID 传入。

#### 请求报文的 Body

空

### HTTP 响应

所有可能的响应状态码详见[状态码汇总表](#code)。

#### 响应报文的 Header 字段

- `X-Request-ID`：UUID（通用唯一识别码），标识本次**请求**。该值为本次请求 header 中 `X-Request-ID`。如果请求出错，请在日志中打印出该值，排查问题。

    > 如果本次请求的响应状态码不是 2XX，那么响应 header 中可能无该字段。

- `X-Resource-ID`：UUID（通用唯一识别码），标识本次请求删除的 Converter 的 **ID**。

#### 响应报文的 Body

- 如果状态码为 2XX，则请求成功。Body 为空。
- 如果状态码不为 2XX，请求失败。Body 中包含 String 类型的 `message` 字段，描述失败的具体原因。


## Update：更新指定的 Converter

### HTTP 请求

```http
PATCH https://api.agora.io/v1/projects/<appId>/rtmp-converters/<converterId>
```

#### 路径参数

- `appId`: String 型必填参数。Agora 为每个开发者提供的 **App ID**。在 Agora 控制台创建一个项目后即可得到一个 App ID。一个 App ID 是一个项目的唯一标识。
- `converterId`: String 型必填参数。Converter 的 **ID**。

#### Query Parameters

`sequence`：Number 型**必填**参数。Update 请求的序列号。取值需要大于或等于 0。请确保后一次 Update 请求的序列号大于前一次 Update 请求的序列号。序列号可以确保 Agora 服务器按照你指定的最新配置来更新 Converter。

> Agora 推荐你在第一次调用 Update 时，将 `sequence` 填 `0`。在第二次调用 Update 时，将 `sequence` 填 `1`。在第三次调用 Update 时，将 `sequence` 填 `2`。依次类推。Agora 服务器会按照最新 Update 请求（即最大的序列号）更新 Converter。

```http
PATCH https://api.agora.io/v1/projects/<appId>/rtmp-converters/<converterId>?sequence={sequence}
```

#### 请求报文的 Header 字段

- `Content-Type`: `application/json`
- `Authorization`: 该字段的值需参考[认证说明](https://docs.agora.io/cn/faq/restful_authentication)。
- `X-Request-ID`: UUID（通用唯一识别码），标识本次**请求**。传入该字段后，Agora 服务器会在响应 header 中返回该字段。

   > Agora 推荐你对 `X-Request-ID` 赋值。如果不赋值，Agora 服务器会自动生成一个 UUID 传入。

#### 请求报文的 Body

- 更新 `converter.transcodeOptions.videoOptions.layout` 配置。请务必参考如下示例代码：
    ```
    {
        "converter": {
            "transcodeOptions": {
                "videoOptions": {
                    "layout": [
                        {   "rtcStreamUid": 201,
                            "region": {
                                "xPos": 0,
                                "yPos": 0,
                                "zIndex": 1,
                                "width": 360,
                                "height": 320
                            },
                            "placeholderImageUrl": "http://example.agora.io/host_placeholder.jpg"
                        },
                        {   "rtcStreamUid": 703,
                            "region": {
                                "xPos": 0,
                                "yPos": 320,
                                "zIndex": 1,
                                "width": 360,
                                "height": 320
                            },
                            "placeholderImageUrl": "http://example.agora.io/guest_placehoder.jpg"
                        }
                    ]
                }
            }
        },
        "fields": "transcodeOptions.videoOptions.layout"
    }
    ```

- 更新 `converter.transcodeOptions.audioOptions.rtcStreamUids` 配置。请务必参考如下示例代码：

    ```json
    {
        "converter":{
            "transcodeOptions":{
                "audioOptions":{
                    "rtcStreamUids":[
                        201,
                        202
                    ]
                }
            },
            "fields": "transcodeOptions.audioOptions.rtcStreamUids"
        }
    }
    ```
		
- 更新 converter 的其他配置，如 `converter.rtmpUrl`:
    ```
    {
        "converter": {
            "rtmpUrl": "rtmp://example.agora.io/live/show68_backup"
        },
        "fields": "rtmpUrl"
    }
    ```

字段含义详见 [Create 请求 body](#mean)。

> Update 不支持更新 Converter 的如下配置：
> - `name`
> - `idleTimeOut`
> - `transcodeOptions.rtcChannel`
> - `transcodeOptions.audioOptions`
> - `transcodeOptions.audioOptions.codecProfile`
> - `transcodeOptions.audioOptions.sampleRate`
> - `transcodeOptions.audioOptions.bitrate`
> -  `transcodeOptions.audioOptions.audioChannels`
> - `transcodeOptions.videoOptions.codecProfile`
>
> 调用 Create 方法创建一个只输出纯视频/音频流的 Converter 后，你无法通过 Update 方法将其更新为只输出纯音频/视频流的 Converter。


### HTTP 响应

所有可能的响应状态码详见[状态码汇总表](#code)。

#### 响应报文的 Header 字段

- `X-Request-ID`：UUID（通用唯一识别码），标识本次**请求**。该值为本次请求 header 中 `X-Request-ID`。如果请求出错，请在日志中打印出该值，排查问题。

    > 如果本次请求的响应状态码不是 2XX，那么响应 header 中可能无该字段。

- `X-Resource-ID`：UUID（通用唯一识别码），标识本次请求更新的 Converter 的 **ID**。

#### 响应报文的 Body

```
{
    "converter": {
      "id": "4c014467d647bb87b60b719f6fa57686",
      "createTs": 1591786766,
      "updateTs": 1591788746,
      "state": "running"
    },
    "fields": "id,createTs,updateTs,state"
}
```

如果状态码为 2XX，请求成功。Body 中包含字段结构如图所示：
字段含义详见下表：

| 字段 | 类型 | 描述  |
|---|----|---|
|converter.id| String| Converter 的 ID。它是 Agora 服务器生成的一个 UUID（通用唯一识别码），标识一个已创建的 Converter。|
|converter.createTs| Number| 创建 Converter 时的 Unix 时间戳（秒）。|
|converter.updateTs| Number| 最近一次更新 Converter 配置时的 Unix 时间戳（秒）。|
|converter.state| String| Converter 的运行状态：<li>`idle`: 推流未开始或已结束。</li><li>`connecting`: 正在连接 Agora 推流服务器和 CDN 服务器。</li><li>`running`: 正在进行推流。</li><li>`recovering`: 正在恢复推流。</li><li>`failure`: 推流失败。</li>|
|fields| String| JSON 编码方式的字段掩码，详见[谷歌 protobuf FieldMask 文档](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#fieldmask)。用于描述返回的 `converter` 中包含的字段集合。在本示例中，`fields` 指定了 Agora 服务器返回 `converter` 字段中的 `id`，`createTs`，`updateTs` 和 `state` 字段子集。|


如果状态码不为 2XX，请求失败。Body 中包含 String 类型的 `message` 字段，描述失败的具体原因。


## API 限流说明

Agora 服务器限制用户 API 调用速率，超出限制速率时会返回状态码 `429(Too Many Requests)`。如果你有更高调用速率需求，请联系技术支持。

| API      | 限流说明                                                     |
| :------- | :----------------------------------------------------------- |
| `Create` | 一个项目中，创建 Converter 的速率上限为 50 次/秒。       |
| `Delete` | 一个项目中，销毁 Converter 的速率上限为 100 次/秒。   |
| `Update` | 一个项目中，更新一个指定的 Converter 的速率上限为 2 次/秒。    |


<a name="code"></a>
## 状态码汇总表

- 如果状态码为 2XX，则请求成功。
- 如果状态码不为 2XX，则请求失败。请根据对应的响应报文 Body 中的 `message` 字段内容排查问题。

| 状态码                  | 可能的 message 字段内容                                      |
| ----------------------- | ------------------------------------------------------------ |
| 200 OK                  | /                                                            |
| 400 Bad Request         | <li>Invalid parameter: rtmpUrl. Replace it and retry.</li><li>Invalid parameter: idleTimeout. Replace it and retry.</li> |
| 401 Unauthorized        | Invalid authentication credentials.             |
| 403 Forbidden           |No invalid permission to use this function. Contact us.|
| 404 Not Found          |Resource is not found and destroyed.|
| 409 Conflict            | Resource with the same name has already existed. Use the existing resource, otherwise delete it and create a new resource.|
| 429 Too Many Requests   |<li>Request rate limit exceeded.</li><li>Resource quota limit exceeded.</li><li>No available resources.</li> |
| 500 Unknown             |  Internal errors. Contact us for troubleshooting.  |
|501 Not Implemented |The method requested has not been implemented.|
| 503 Service Unavailable | <li>The server is temporarily overloading. Retry with a backoff strategy and contact us for troubleshooting.</li><li>The server is temporarily down. Retry with a backoff strategy.</li> |
| 504 Gateway Timeout     | Gateway timeout. Check whether the resource is created, if not, re-create a new resource. |

## 回调通知

使用推流 RESTful API 后，Agora 消息通知服务器会通过 HTTP 或 HTTPS 请求给你的服务器回调通知，详情请参考[消息通知服务](https://docs-preview.agoralab.co/cn/Agora%20Platform/nscstreaming?platform=All%20Platforms)。


## 注意事项

本节总结使用推流 RESTful API 的重要注意事项。

|注意事项|影响程度|
|------|--------|
|请确保 Agora 频道场景为直播。|★★★☆☆ |
| 如果你需要同一个频道内仅有一个 Converter，请你务必使用 `name` 参数，详见 `name` 参数解释。    |   ★★★☆☆     |
| Agora 默认在你的业务服务器所在区域创建 Converter。如果你使用的 CDN 服务和业务服务器不在一个区域，请使用 `regionHintIp` 参考，使 Agora 在 CDN 服务所在区域创建 Converter，以保障 RTMP 流的稳定性。详见 `regionHintIp` 参数解释。 | ★★★★★ |
| 如果 Converter 因为服务故障等原因自动销毁，Agora 推荐你重新创建一个 Converter。|★★★★★ |
|创建 Converter 后，如果 CDN 侧拉流异常。Agora 推荐你 `Delete` 该 Converter，再重新创建一个 Converter。|★★★★★ |
|请确保对同一个 Converter 进行多次 `Update` 时，`sequence` 值递增，详见 `sequence` 参数解释。| ★★★★★ |
| 请求出错时，请务必在日志中打印出响应 Header 中的 `X-Request-ID` 和 `X-Resource-ID` 字段值，以方便排查问题。 | ★★★☆☆   |
