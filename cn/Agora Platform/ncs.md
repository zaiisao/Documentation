
---
title: 消息通知服务（Beta）
description: 
platform: All Platforms
updatedAt: Fri Feb 05 2021 06:58:15 GMT+0800 (CST)
---
# 消息通知服务（Beta）
<div class="alert note">Agora 消息通知服务目前处于 Beta 阶段，不建议你的核心业务依赖该服务。</div>

消息通知服务可以将 Agora 各业务下的**事件**，以 HTTP/HTTPS 请求的形式通知到你的服务器。消息通知服务已集成实时通信（Real-Time Communication）、推流到 CDN（Push Stream to CDN 3.0）和云端录制（Cloud Recording）业务下的一些**事件**。你可以向 Agora 提供相关的配置信息来开通该服务。

<div class="alert note"> 如果你想开通推流到 CDN 的消息通知服务，请使用 v2.4.1 及以上的 Native RTC SDK 或 v2.9.0 及以上的 Web RTC SDK。</div>

## 用户配置

你需要[提交工单](https://agora-ticket.agora.io)联系技术支持开通消息通知服务，并提供以下配置信息：

1. 接收消息通知的 HTTP/HTTPS 服务器地址。
2. 开通消息通知服务的项目所对应的 App ID。
3. 你想使用通知服务的[业务](#pro)。
4. 你想得到通知的[事件](#event)。
5. 选择是否开启消息通知**重试**。消息通知服务是实时通知，开启通知重试后，消息通知服务器就会在第一次通知失败后立即重试，再次发送消息通知，最多会尝试三次通知。消息通知服务器在以下任意一种情况下会认为通知失败：
	*  发送消息通知后，10 秒内没有收到你的服务器的响应。
	*  收到你的服务器响应的 HTTP 状态码不是 200。

配置消息通知服务后，你需要注意以下事项：

- 5 分钟后，该服务即会生效。你的服务器需要尽快响应消息通知服务器，响应的 body 格式必须为 JSON。
- 我们系统将自动生成 `secret`。`secret` 是字符串类型的密钥，密钥可以用来生成签名，你需要保存这个密钥以备后续验签，详见[验证签名](#signature)。


## 消息通知回调格式
消息通知回调以 HTTP/HTTPS POST 请求发送给你的服务器，请求的 `body` 格式为 JSON。字符编码为 UTF-8。

消息通知回调的 `header` 中包含以下字段：

| 字段名            | 值                 |
| ----------------- | ------------------ |
| `Content-Type`    | `application/json` |
| `Agora-Signature` | 签名值             |

消息通知回调的 `body` 中包含以下字段：

| 字段名      | 类型   | 含义                                                         |
| ----------- | ------ | ------------------------------------------------------------ |
| `noticeId`  | String | 通知 ID，标识来自业务服务器的一次事件通知。            |
|<a name="pro"></a> `productId` | Number | 业务 ID: <li>`1`：Real-Time Communication（实时通信） </li><li>`2`：Push Stream to CDN 3.0（推流到 CDN）</li><li>`3`：Cloud Recording and Extension Service（云端录制及扩展服务）</li> |
|<a name="event"></a> `eventType`    |    Number  | 通知的事件类型：<li>如果你使用实时通信业务，请参考[实时通信事件类型](https://docs-preview.agoralab.co/cn/Agora%20Platform/rtc_eventtype?platform=All%20Platforms#事件类型)。</li><li>如果你使用推流到 CDN 业务，请参考[推流到 CDN 事件类型](https://docs-preview.agoralab.co/cn/Agora%20Platform/rtc_eventtype?platform=All%20Platforms#a-namecdna-事件类型)。</li><li>如果你使用云端录制及扩展服务，请参考[云端录制事件类型](https://docs.agora.io/cn/cloud-recording/cloud_recording_callback_rest?platform=All%20Platforms#a-nameeventa回调事件)。</li>|
|   `notifyMs`   |   Number | 消息通知服务器向你的服务发出回调请求的 Unix 时间戳，单位为毫秒。<br> Note：通知重试时会更新该时间。|
|  `payload`       |      JSON Object       | 事件内容：<li>如果你使用实时通信业务，请参考[实时通信 payload 字段](https://docs-preview.agoralab.co/cn/Agora%20Platform/rtc_eventtype?platform=All%20Platforms#payload-字段)。</li><li>如果你使用推流到 CDN 业务，请参考[推流到 CDN payload 字段](https://docs-preview.agoralab.co/cn/Agora%20Platform/rtc_eventtype?platform=All%20Platforms#payload-字段-1)。</li><li>如果你使用云端录制及扩展服务，请参考[云端录制 payload 字段](https://docs.agora.io/cn/cloud-recording/cloud_recording_callback_rest?platform=All%20Platforms#a-namepayloadapayload-字段)。</li>|



<a name="signature"></a>
## 验证签名

我们系统直接生成了密钥（`secret`），并通过 HMAC/SHA1 算法最终生成 `Agora-Signature` 签名。
你可以使用已经保存了的这个密钥（`secret`）来验证 `Agora-Signature` 签名，验证签名时你可以参考以下代码：

* Python 

```python
#!/usr/bin/env python3
# !-*- coding: utf-8 -*-
import hashlib
import hmac
  
# 拿到消息通知的 raw request body 并对其计算签名，也就是说下面代码中的 request_body 是反序列化之前的 binary byte array，不是反序列化之后的 dictionary
request_body = '{"eventMs":1560408533119,"eventType":10,"noticeId":"4eb720f0-8da7-11e9-a43e-53f411c2761f","notifyMs":1560408533119,"payload":{"a":"1","b":2},"productId":1}'
  
secret = 'secret'
  
signature = hmac.new(secret, request_body, hashlib.sha1).hexdigest()
  
print(signature) # 033c62f40f687675f17f0f41f91a40c71c0f134c
```

* Node.js 

```javascript
const crypto = require('crypto')
  
// 拿到消息通知的 raw request body 并对其计算签名，也就是说下面代码中的 requestBody 是反序列化之前的 binary byte array，不是反序列化之后的 object
const requestBody = '{"eventMs":1560408533119,"eventType":10,"noticeId":"4eb720f0-8da7-11e9-a43e-53f411c2761f","notifyMs":1560408533119,"payload":{"a":"1","b":2},"productId":1}'
  
const secret = 'secret'
  
const signature = crypto.createHmac('sha1', secret).update(requestBody, 'utf8').digest('hex')
  
console.log(signature) // 033c62f40f687675f17f0f41f91a40c71c0f134c
```

* Java
	
```java
import javax.crypto.Mac;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
 
public class HmacSha {
    // 将加密后的字节数组转换成字符串
    public static String bytesToHex(byte[] bytes) {
        StringBuffer sb = new StringBuffer();
 
        for (int i = 0; i < bytes.length; i++) {
            String hex = Integer.toHexString(bytes[i] & 0xFF);
 
            if (hex.length() < 2) {
                sb.append(0);
            }
 
            sb.append(hex);
        }
 
        return sb.toString();
    }
 
    //HMAC/SHA1 加密，返回加密后的字符串
    public static String hmacSha1(String message, String secret) {
        try {
            SecretKeySpec signingKey = new SecretKeySpec(secret.getBytes(
                        "utf-8"), "HmacSHA1");
            Mac mac = Mac.getInstance("HmacSHA1");
            mac.init(signingKey);
 
            byte[] rawHmac = mac.doFinal(message.getBytes("utf-8"));
 
            return bytesToHex(rawHmac);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
 
    public static void main(String[] args) {
        //拿到消息通知的 raw request body 并对其计算签名，也就是说下面代码中的 request_body 是反序列化之前的 binary byte array，不是反序列化之后的 object
        String request_body = "{\"eventMs\":1560408533119,\"eventType\":10,\"noticeId\":\"4eb720f0-8da7-11e9-a43e-53f411c2761f\",\"notifyMs\":1560408533119,\"payload\":{\"a\":\"1\",\"b\":2},\"productId\":1}";
        String secret = "secret";
        System.out.println(hmacSha1(request_body, secret)); //033c62f40f687675f17f0f41f91a40c71c0f134c
    }
}
	
```

## 注意事项
	
- 你的服务器收到的通知顺序和事件发生的顺序不一定完全一致。
- 为确保消息通知的可靠性，每次事件可能会有不止一次消息通知，你的服务器需要能处理重复消息。
- 如果需要配置 QPS（Query Per Second）比较高的事件，如实时通信业务中的 105 或 106 事件，请确保你的服务器有足够的处理能力。



