
---
title: 水印
description: 
platform: Linux Java
updatedAt: Thu Feb 20 2020 07:42:26 GMT+0800 (CST)
---
# 水印
## 功能描述

在使用合流录制模式时，你可在录制视频文件中添加水印，如公司 logo、时间戳或特定文字信息，以实现防伪、版权声明、宣传或记录等目的。

本地服务端录制 SDK 支持文字、动态时间戳和静态图片三种水印，你可按需选择。

- 文字水印：使用一段文字作为水印，可指定字体和字号。
- 动态时间戳水印：使用录制服务器的当前时间作为水印，显示形式为“2019:06:18 14:30:35"。
- 静态图片水印：使用一张本地 PNG 图片作为水印。

> 本地服务端录制 SDK 自 v3.0.0 起支持水印功能。

## 设置水印

### 文字水印

通过 `literalWms` 参数设置文字水印。`literalWms` 是由多个 `LiteraWatermarkConfig` 组成的数组，一个 `LiteraWatermarkConfig` 对应一个文字水印。

`LiteraWatermarkConfig` 中包含以下设置：

- 设置 `wmLitera` 参数添加文字内容。
- 设置 `fontFilePath` 参数指定字体文件路径。支持 ttf，otf 等字体格式。如不设置，使用默认字体 NotoSansMonoCJKsc-Regular。
- 设置 `fontSize` 参数指定文字的字号。如不设置，默认值为 10 （相当于 144 DPI 设备上的 10 x 15 磅）。
- 通过 `offsetX`，`offsetY`，`wmWidth`，`wmHeight` 四个参数设置水印的水平位置、垂直位置、宽度、高度，详见[设置水印大小和位置](#size)。

> - 最多可添加 10 个文字水印。
> - 仅支持 **UTF-8** 编码。
> - 支持字符取决于字体。默认字体为 NotoSansMonoCJKsc-Regular，支持的语言和字符详见[字体介绍](https://www.google.com/get/noto/help/cjk/)。
> - 字符串长度没有限制。文字内容的最终显示受字号和水印框大小的影响，超出水印框的部分不显示。
> - 支持自动换行，当文字内容长度超过水印框宽度时，会自动换行。也支持换行符 `\n`。
> - 文字颜色为黑色或白色，会根据视频画布的颜色自动调整。若视频背景较暗，则颜色为白色；若视频背景较亮，则颜色为黑色。当视频背景不均匀时，颜色为黑白相间。

### 时间戳水印

通过 `timestampWms` 参数设置时间戳水印。`timestampWms` 是由 `TimestampWatermarkConfig` 组成的数组，一个 `TimestampWatermarkConfig` 对应一个时间戳水印。

`TimestampWatermarkConfig` 中包含以下设置：

- 设置 `fontSize` 参数指定时间戳的字号。如不设置，默认值为 10 （相当于 144 DPI 设备上的 10 x 15 磅）。
- 通过 `offsetX`，`offsetY`，`wmWidth`，`wmHeight` 四个参数设置水印的水平位置、垂直位置、宽度、高度，详见[设置水印大小和位置](#size)。

> - 只能添加 1 个时间戳水印。
> - 时间戳显示录制服务器的当前时间，是动态变化的，显示形式为 “2019:06:18 14:30:35"。
> - 时间戳颜色为黑色或白色，会根据视频画布的颜色自动调整。若视频背景较暗，则颜色为白色；若视频背景较亮，则颜色为黑色。当视频背景不均匀时，颜色为黑白相间。

### 图片水印

通过 `imageWms` 参数设置图片水印。`imageWms` 是由多个 `ImageWatermarkConfig` 组成的数组，一个 `ImageWatermarkConfig` 对应一个图片水印。
`ImageWatermarkConfig` 中包含以下设置：

- 设置 `imagePath` 参数指定图片路径，仅支持本地 PNG 图片。
- 通过 `offsetX`，`offsetY`，`wmWidth`，`wmHeight` 四个参数设置水印的水平位置、垂直位置、宽度、高度，详见[设置水印大小和位置](#size)。

> - 最多可添加 4 个图片水印。
> - 仅支持本地 PNG 图片。
> - 建议图片分辨率不超过 480p。
> - 如果图片小于水印框，图片居中显示，不拉伸；如果图片大于水印框，图片等比缩小至合适水平，再居中显示。 

### <a name= "size"></a>设置水印大小和位置

无论添加哪种水印，你都必须通过 `offsetX`，`offsetY`，`wmWidth`，`wmHeight` 四个参数设置水印的水平位置、垂直位置、宽度、高度。

![](https://web-cdn.agora.io/docs-files/1564544792118)

如上图所示，以**视频画布左上角**为原点，`offsetX` 和 `offsetY` 分别代表**水印框左上角**到原点的水平距离和垂直距离，`wmWidth` 和 `wmHeight` 分别代表水印框的**宽度**和**高度**。这四个参数均为绝对值，单位为像素，默认值为 0。

> - 水印框完全透明，无颜色。
> - 如果水印框超出画布范围，则水印框与画布的交集区域会作为显示区域。

## 添加水印

你可通过以下方法添加水印：

- 调用 `setVideoMixingLayout` 方法设置合流布局时，设置 `literalWms`、`timestampWms` 和 `imageWms` 参数分别添加文字水印、时间戳水印或图片水印；
- 直接调用 `updateWatermarkConfigs` 方法设置 `literaWms` 、`timestampWms` 和 `imgWms` 参数分别添加文字水印、时间戳水印或图片水印。

## 更新和删除水印

调用 `setVideoMixingLayout` 或 `updateWatermarkConfigs` 方法均可更改水印的个数和具体设置，也可删除已添加的水印。

举例来说，假设视频中已有 1 个图片水印和 1 个文字水印。如想删除文字水印，可以调用  `setVideoMixingLayout` 或 `updateWatermarkConfigs` 方法只传入原先的图片水印的设置。

注意：如想删除全部已添加的水印，只能调用 `updateWatermarkConfigs` 方法。调用 `setVideoMixingLayout` 更新合流布局时，如果没有水印信息， 即不对水印进行任何修改，因此不能删除所有已添加的水印。

## 示例代码

以下 Java 示例代码演示了如何调用 `setVideoMixingLayout` 方法设置 `literalWms`、`timestampWms` 和 `imageWms` 参数添加 3 个水印：1 个图片水印，1 个时间戳水印，1 个文字水印。

```java
/*
 * RecordingSDKInstance is the instance of the RecordingSDK class.
 */
Common ei = new Common();
Common.VideoMixingLayout layout = ei.new VideoMixingLayout();
layout.timestampWms = new TimestampWatermarkConfig[1];
layout.timestampWms[0] = ei.new TimestampWatermarkConfig();
layout.timestampWms[0].fontSize = 10;
layout.timestampWms[0].offsetX = 10;
layout.timestampWms[0].offsetY = 110;
layout.timestampWms[0].wmWidth = 200;
layout.timestampWms[0].wmHeight = 100;
layout.literalWms = new LiteraWatermarkConfig[1];
layout.literalWms[0] = ei.new LiteraWatermarkConfig();
layout.literalWms[0].wmLitera = "test watermark";
layout.literalWms[0].fontSize = 10;
layout.literalWms[0].fontFilePath = "path-to-font-file.otf";
layout.literalWms[0].offsetX = 10;
layout.literalWms[0].offsetY = 10;
layout.literalWms[0].wmWidth = 300;
layout.literalWms[0].wmHeight = 100;
layout.imageWms = new ImageWatermarkConfig[1];
layout.imageWms[0] = ei.new ImageWatermarkConfig();
layout.imageWms[0].imagePath = "path-to-image.png";
layout.imageWms[0].offsetX = 10;
layout.imageWms[0].offsetY = 220;
layout.imageWms[0].wmWidth = 400;
layout.imageWms[0].wmHeight = 400;
RecordingSDKInstance.setVideoMixingLayout(layout);
```

## API 参考

- [`setVideoMixingLayout`](https://docs.agora.io/cn/Recording/API%20Reference/recording_java/classio_1_1agora_1_1recording_1_1_recording_s_d_k.html?transId=2.8.0#a5834d23933d66ff7a5555b0de22c4313)
- [`updateWatermarkConfigs`](https://docs.agora.io/cn/Recording/API%20Reference/recording_java/classio_1_1agora_1_1recording_1_1_recording_s_d_k.html?transId=2.8.0#a88eb63ddbae307480770c4376444d473)
