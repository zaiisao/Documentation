
---
title: 云端录制常见错误
description: 
platform: All Platforms
updatedAt: Fri Oct 23 2020 09:03:03 GMT+0800 (CST)
---
# 云端录制常见错误
本文仅列出使用云端录制 RESTful API 过程中常见的错误码或错误信息，如果遇到其他错误，请联系 Agora 技术支持。

- `2`：参数不合法，请确保参数类型、大小写和取值范围正确，且必填的参数均已填写。
- `7`：录制已经在进行中 ，请勿用同一个 resource ID 重复 `start` 请求。
- `8`：HTTP 请求头部字段错误，有以下几种情况：
  - `Content-type` 错误，请确保 `Content-type` 为 `application/json;charset=utf-8`。
  - 请求 URL 中缺少 `cloud_recording` 字段。
  - 使用了错误的 HTTP 方法。	
  - 请求包体不是合法的 JSON 格式。
- `49`：使用同一个 resource ID 和录制 ID（sid）重复 `stop` 请求。
- `53`：录制已经在进行中。当采用相同参数再次调用 `acquire` 获得新的 resource ID，并用于 `start` 请求时，会发生该错误。如需发起多路录制，需要在 `acquire` 方法中填入不同的 UID。
- `62`：调用 `Acquire` 请求时，如果出现该错误，表示你填入的 App ID 没有[开通云端录制权限](https://docs.agora.io/cn/cloud-recording/cloud_recording_rest?platform=All%20Platforms#%E5%BC%80%E9%80%9A%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6%E6%9C%8D%E5%8A%A1)。
- `65`：多为网络抖动引起。当调用 `start` 方法收到该错误码时，需要使用同一 resource ID 再次调用 `start`。建议使用退避策略重试两次，如第一次等待 3 秒后重试、第二次等待 6 秒后重试。
- `432`：请求参数错误。请求参数不合法，或请求中的 App ID，频道名或用户 ID 与 resource ID 不匹配。
- `433`：resource ID 过期。获得 resource ID 后必须在 5 分钟内开始云端录制。请重新调用 `acquire` 获取新的 resource ID。
- `435`：没有录制文件产生。频道内没有用户发流，无录制对象。
- `501`：录制服务正在退出。该错误可能在调用了 `stop` 方法后再调用 `query` 时发生。
- `1001`：resource ID 解密失败。请重新调用 `acquire` 获取新的 resource ID。
- `1003`：App ID 或者录制 ID（sid）与 resource ID 不匹配。请确保在一个录制周期内 resource ID、App ID 和录制 ID 一一对应。
- `1013`：频道名不合法。频道名必须为长度在 64 字节以内的字符串。以下为支持的字符集范围（共 89 个字符）：
  - 26 个小写英文字母 a-z
  - 26 个大写英文字母 A-Z
  - 10 个数字 0-9
  - 空格
  - "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","

- `1028`：`updateLayout` 方法的请求包体中参数错误。
- `"invalid appid"`：无效的 App ID。请确保 App ID 填写正确。如果你已经确认 App ID 填写正确，但仍出现该错误，请[提交工单](https://agora-ticket.agora.io/)。
- `"no Route matched with those values`": 该错误可能由 HTTP 方法填写错误导致，例如将 GET 方法填写为 POST；也可能由请求 URL 填写错误导致。
- `"Invalid authentication credentials"`: 该错误可能由以下原因导致。 如果你已经排除以下原因，但仍出现该错误，请联系 support@agora.io。
  - Customer ID 或 Customer Certificate 填写错误。
  - App ID 没有[开通云端录制权限](https://docs.agora.io/cn/cloud-recording/cloud_recording_rest?platform=All%20Platforms#%E5%BC%80%E9%80%9A%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6%E6%9C%8D%E5%8A%A1)。
  - HTTP 请求头的认证信息有误，如 `Authorization` 字段的值 `Basic <Authorization>` 缺少 `Basic`。
  - HTTP 请求头的格式不正确，如 `Content-type` 字段的值 `application/json;charset=utf-8` 大小写不正确或包含空格。
