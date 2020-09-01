
---
title: 限定访问区域
description: 
platform: Windows CPP,Linux CPP
updatedAt: Tue Sep 01 2020 03:50:35 GMT+0800 (CST)
---
# 限定访问区域
## 功能描述

为适应不同国家或地区的法律法规，Agora RTM SDK 支持限定访问区域功能。你可以将 Agora RTM SDK 的数据传输限定在某一区域范围内。限定区域之后，RTM SDK 只能连接位于限定区域的 Agora RTM 服务器。

<div class="alert note">如果要设置印度为限定区域，你必须从印度的 IP 地址发起请求。否则会设置失败。</div>

## 实现方法

你需要在调用 `createClient` 初始化 RTM 客户端之前调用 `setRtmServiceContext` 设置限定区域。RTM SDK 支持以下区域：

- `AREA_CODE_GLOB`: （默认）全球。
- `AREA_CODE_CN`: 中国大陆。
- `AREA_CODE_NA`: 北美区域。
- `AREA_CODE_EU`: 欧洲区域。
- `AREA_CODE_AS`: 除中国大陆外的亚洲区域。
- `AREA_CODE_JP`: 日本。
- `AREA_CODE_IN`: 印度。


### 示例代码


```c++
// 设置限定区域
agora::rtm::RtmServiceContext rtmContext;
rtmContext.areaCode = agora::rtm::AREA_CODE_CN | agora::rtm::AREA_CODE_AS;
agora::rtm::SET_RTM_SERVICE_CONTEXT_ERR_CODE errorCode = setRtmServiceContext(rtmContext);
```


##  开发注意事项

### 防火墙要求

如果你的网络环境部署了防火墙，你需要根据你指定的区域将下表中对应的域名添加到防火墙白名单，不对 IP 地址设限，且开放相应端口。

- 域名白名单

| 区域                   | 域名                                                         |
| :--------------------- | :----------------------------------------------------------- |
| 中国大陆               | `ap.agoraio.cn` <br> `report-edge.agoraio.cn` <br> `service-agoraio.cn` |
| 北美区域               | `ap-america.agora.io`  <br> `report-america.agora.io` <br> `service-america.agora.io` |
| 欧洲区域               | `ap-europe.agora.io` <br> `report-europe.agora.io` <br> `service-europe.agora.io` |
| 日本                   | `ap-japan.agora.io` <br> `report-japan.agora.io` <br> `service-japan.agora.io` |
| 印度                   | `ap-india.agora.io` <br> `report-india.agora.io` <br> `service-india.agora.io` |
| 除中国大陆外的亚洲区域 | `ap-asia.agora.io`  <br> `report-asia.agora.io` <br> `service-asia.agora.io` |

- 端口

| 端口              | 协议 | 操作 |
| :---------------- | :--- | :--- |
| 9130; 9131; 9140  | TCP  | 允许 |
| 1080; 8000; 8130;  9120; 9121; 9700; 25000 | UDP  | 允许 |

## API 参考

[`setRtmServiceContext`](https://docs.agora.io/cn/Video/API%20Reference/RTM_cpp/group__get_rtm_sdk_version.html#ga55ed2d637b72bf2940872b8736a00bd3)
