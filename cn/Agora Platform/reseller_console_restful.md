
---
title: 代理商控制台 RESTful API
description: 
platform: All Platforms
updatedAt: Tue Oct 08 2019 06:29:35 GMT+0800 (CST)
---
# 代理商控制台 RESTful API
## 签名

<div class="alert note">在使用本文 RESTful API 提供的功能前，请确认你已注册代理商控制台账号。若无账号，联系 sales@agora.io 或在 Agora Dashboard <a href="https://dashboard.agora.io/support">提交工单</a >申请账号。</div>

RESTful API 仅支持 HTTPS。在使用 RESTful API 前，你需要在 HTTP 请求头部（header）填入 `Authorization` 字段，进行签名。

### 1. 创建待签字符串

通过 UTC 时间和 UUID 创建代签字符串，伪代码及参数描述如下：

| 参数名           | 类型   | 描述                                                         |
| :--------------- | :----- | :----------------------------------------------------------- |
| **x-date**       | string | HTTP 1.1 协议中规定的 UTC 时间。例如：Wed, 11 Sep 2019 14:28:28 UTC。<br>**Note**<br>需确保机器中设置的时间为 UTC 时间。 |
| **x-request-id** | string | UUID。包含32个16进制数字，指以连字号分为五段且形式为8-4-4-4-12的32个字符。例如：4c18781f-16b2-43c5-9281-2eddfd313bd8。 |

```
 StringToSign = "x-date: " + {gmtTime} + "\n" + "x-request-id: " + {uuid} 
```

### 2. 计算签名

通过 Access Key Secret 和代签字符串计算签名。登录[代理商控制台](https://reseller.agora.io/)，点击右上角账户名，进入下拉菜单设置页面，在 **API 密钥** 中即可生成 Access Key Secret 和 Access Key ID（用于步骤 3）。

此签名使用加密哈希函数进行加密，提供了更高程度的保护，伪代码及参数描述如下：

| 参数名              | 类型   | 描述                                                         |
| :------------------ | :----- | :----------------------------------------------------------- |
| **accessKeySecret** | string | 在代理商控制台中获取的 Access Key Secret，由 a-z、A-Z 和 0-9 组成的字符串，长度为 40。 |
| **StringToSign**    | string | 步骤 1 中创建的代签字符串。                                    |


```
 Signature = HexEncode(HAMC(accessKeySecret, StringToSign)) 
```

### 3. 向 HTTPS 请求添加签名

通过 Access Key ID 和签名向 HTTPS 请求添加签名，将签名信息添加到名为 Authorization 的 HTTP 请求头部。

确保签名时 header 的顺序和此次请求中 headers 指定的顺序一致。Access Key ID 仅用于访问 RESTful API。

伪代码及参数描述如下：

| 参数名          | 类型   | 描述                                                         |
| :-------------- | :----- | :----------------------------------------------------------- |
| **accessKeyId** | string | 在代理商控制台中获取的 Access Key ID，由 a-z、A-Z 和 0-9 组成的字符串，长度为 40。 |
| **Signature**   | string | 步骤 2 中计算得到的签名信息。                                  |

```
 Authorization: hmac username="{accessKeyId}", algorithm="hmac-sha256", headers="x-date x-request-id", signature="{Signature}" 
```

###  示例代码

本文提供 Java 语言生成 `Authorization` 字段的示例代码，请参考代码获取相应的 `Authorization` 值。

<details>
	<summary><font color="#3ab7f8">cURL 示例代码</font></summary>
	
```
curl -i -X GET https://localhost:8443/benchmark \
      -H "X-Date: Wed, 11 Sep 2019 16:21:09 GMT" \
      -H "X-Request-ID: 70a0a14b-f38c-4aaf-aa46-04d8fae3e38c" \
      -H 'Authorization: hmac username="ccc", algorithm="hmac-sha256", headers="x-date x-request-id", signature="nu/RSu8rOCm8Y6kbr7iqHEq6rukF+MtXmz8Ds26GABI="'
```
</details>

<details>
	<summary><font color="#3ab7f8">Java 示例代码</font></summary>
	
```java
package io.agora.demo.restfulapi.reseller;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpUriRequest;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import sun.misc.BASE64Encoder;
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.io.IOException;
import java.time.ZoneId;
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;
import java.util.UUID;
public class AgoraResellerRestfulApiDemo {
    private static final String GMT_ZONE_ID = "GMT+00";
    private static final String HMAC_SHA256 = "HmacSHA256";
    private static final String X_DATE_HEADER = "x-date";
    private static final String X_REQUEST_ID_HEADER = "x-request-id";
    private static final String AUTH_HEADER = "authorization";
    private static final String TO_SIGNED_FORMAT = "%s: %s\n%s: %s";
    private static final String AUTH_FORMAT = "hmac username=\"%s\", algorithm=\"hmac-sha256\", headers=\"x-date x-request-id\", signature=\"%s\"";
    private static final String ACCESS_KEY_ID = "";
    private static final String ACCESS_KEY_SECRET = "";
    private static final String URL = "https://api.agora.io/v1/resellers/companies";
    public static void main(String[] args) {
        CloseableHttpClient httpClient = HttpClients.createDefault();
        HttpGet httpGet = new HttpGet(URL);
        buildAuth(httpGet);
        CloseableHttpResponse httpResponse = null;
        try {
            httpResponse = httpClient.execute(httpGet);
            String result = EntityUtils.toString(httpResponse.getEntity());
            System.out.println(result);
        } catch (IOException e) {
            throw new RuntimeException(e.getMessage(), e);
        } finally {
            if(httpResponse != null) {
                try {
                    httpResponse.close();
                } catch (IOException e) {
                    throw new RuntimeException(e.getMessage(), e);
                }
            }
        }
    }
    public static String gmtDateStrOfNow() {
        ZonedDateTime now = ZonedDateTime.now(ZoneId.of(GMT_ZONE_ID));
        return DateTimeFormatter.RFC_1123_DATE_TIME.format(now);
    }
    public static String hmacSha256(String key, String data) {
        try {
            SecretKeySpec signingKey = new SecretKeySpec(key.getBytes(), HMAC_SHA256);
            Mac mac = Mac.getInstance(HMAC_SHA256);
            mac.init(signingKey);
            byte[] rawHmac = mac.doFinal(data.getBytes());
            return (new BASE64Encoder()).encode(rawHmac);
        } catch (Exception e) {
            throw new RuntimeException(e.getMessage(), e);
        }
    }
    private static void buildAuth(HttpUriRequest request) {
        String xDateStr = gmtDateStrOfNow();
        String xRequestIdStr = UUID.randomUUID().toString();
        String toSigned = String.format(TO_SIGNED_FORMAT, X_DATE_HEADER, xDateStr, X_REQUEST_ID_HEADER, xRequestIdStr);
        String signature = hmacSha256(ACCESS_KEY_SECRET, toSigned);
        String authStr = String.format(AUTH_FORMAT, ACCESS_KEY_ID, signature);
        request.addHeader(X_DATE_HEADER, xDateStr);
        request.addHeader(X_REQUEST_ID_HEADER, xRequestIdStr);
        request.addHeader(AUTH_HEADER, authStr);
    }
}
```
</details>

## 接入点

所有请求都发送给 BaseUrl：[https://api.agora.io](https://api.agora.io/)

- 请求：参数格式必须为 JSON ，内容类型: application/json。

- 响应：响应内容的格式为 JSON。以下为定义的响应状态：

  | 状态码  | 描述             |
  | :------ | :--------------- |
  | 200/201 | 请求处理成功     |
  | 400     | 邮箱已被使用     |
  | 401     | 签名不匹配       |
  | 403     | 无权限访问       |
  | 404     | API 调用错误     |
  | 429     | API 调用过于频繁 |
  | 500     | 服务器内部错误   |

  

## 创建公司（POST）

**基本信息**

| 请求基本信息 | 描述                           |
| :----------- | :----------------------------- |
| 方法         | POST                           |
| 请求 URL     | BaseUrl/v1/resellers/companies |

**请求参数**

#### Body 参数

<table>
<tr>
<th>参数名</th>
<th>类型</th>
<th>描述</th>
</tr>
<tr>
<td>companyName </td>
<td>string</td>
<td>（必填）公司名称，长度为 [1,20]。</td>
</tr>
<tr>
<td>email</td>
<td>string</td>
<td>（必填）邮箱，用于创建公司的 Dashboard 账号。</td>
</tr>
<tr>
<td>firstName</td>
<td>string</td>
<td>（必填）用户名字，长度为 [1,255]。</td>
</tr>
<tr>
<td>lastName</td>
<td>string</td>
<td>（必填）用户姓氏，长度为 [1,255]。</td>
</tr>
<tr>
<td>country</td>
<td>string</td>
<td>（必填）国家。需输入符合 ISO 3166-1 国际标准的二位字母代码。</td>
</tr>
<tr>
<td>area</td>
<td>string</td>
<td>（必填）地区。<br><li>CN：中国</li><li>Non-CN：非中国</td>
</tr>
<tr>
<td>appLimit</td>
<td>number</td>
<td>应用数量上限。默认值为 10，最小值为 1。</td>
<td>
<tr>
<td>memberLimit</td>
<td>number</td>
<td>子账号数量上限。默认值为 10，最小值为 1。</td>
</tr>
<tr>
<td>industry</td>
<td>string</td>
<td>行业。<br><li>1：客户服务<br><li>2：电商<br><li>3：教育<br><li>4：企业<br><li>5：金融<br><li>6：游戏<br><li>7：智能硬件 IOT 机器人<br><li>8：医疗健康<br><li>9：人力资源<br><li>10：社交/泛娱乐<br><li>11：系统集成<br><li>12：（默认）其它<br><li>13：直播<br><li>14：企业协作/联络中心<br><li>15：家政服务 / O2O<br><li>16：软件外包<br><li>17：个人开发者<br><li>18：呼叫中心<br><li>19：AR/VR/AI</td>
</tr>
<tr>
<td>interest</td>
<td>string</td>
<td>感兴趣的产品。<br><li>1：（默认）其他<br><li>2：视频<br><li>3：音频<br><li>4：直播<br><li>5：游戏</td>
</tr>
<tr>
<td>environment</td>
<td>string</td>
<td>开发平台。<br><li>1：（默认）其他<br><li>2：Web<br><li>3：Android<br><li>4：iOS<br><li>5：Windows<br><li>6：Mac<br><li>7：小程序<br><li>8：Linux</td>
</tr>
<tr>
<td>sendPasswordResetEmail</td>
<td>boolean</td>
<td>是否发送重置密码邮件。<br><li>true：发送。<br><li>false：（默认）不发送。</td>
</tr>
</table>

**请求示例**

```
{
  "name": "Agora",
  "email": "test@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "country": "CN",
  "area": "CN",
  "appLimit": 10,
  "memberLimit": 10,
  "industry": 12,
  "interest": 1,
  "environment": 1,
  "sendPasswordResetEmail": false
}
```

<a name="CreateCompanyReturn"></a>
**响应参数**

<table>
<tr>
<th>参数名</th>
<th>类型</th>
<th>描述</th>
</tr>
<tr>
<td>id </td>
<td>number</td>
<td>公司 ID。</td>
</tr>
<tr>
<td>name </td>
<td>string</td>
<td>公司名称。</td>
</tr>
<tr>
<td>country</td>
<td>string</td>
<td>国家，使用符合 ISO 3166-1 国际标准的二位字母代码。</td>
</tr>
<tr>
<td>area</td>
<td>string</td>
<td>地区。<br><li>CN：中国</li><li>Non-CN：非中国</td>
</tr>
<tr>
<td>appLimit</td>
<td>number</td>
<td>应用数量上限。</td>
<td>
<tr>
<td>memberLimit</td>
<td>number</td>
<td>子账号数量上限。</td>
</tr>
<tr>
<td>industry</td>
<td>string</td>
<td>行业。<br><li>1：客户服务<br><li>2：电商<br><li>3：教育<br><li>4：企业<br><li>5：金融<br><li>6：游戏<br><li>7：智能硬件 IOT 机器人<br><li>8：医疗健康<br><li>9：人力资源<br><li>10：社交/泛娱乐<br><li>11：系统集成<br><li>12：其它<br><li>13：直播<br><li>14：企业协作/联络中心<br><li>15：家政服务 / O2O<br><li>16：软件外包<br><li>17：个人开发者<br><li>18：呼叫中心<br><li>19：AR/VR/AI</td>
</tr>
<tr>
<td>interest</td>
<td>string</td>
<td>感兴趣的产品。<br><li>1：其他<br><li>2：视频<br><li>3：音频<br><li>4：直播<br><li>5：游戏</td>
</tr>
<tr>
<td>environment</td>
<td>string</td>
<td>开发平台。<br><li>1：其他<br><li>2：Web<br><li>3：Android<br><li>4：iOS<br><li>5：Windows<br><li>6：Mac<br><li>7：小程序<br><li>8：Linux</td>
</tr>
<tr>
<td>status</td>
<td>number</td>
<td>公司状态。<br><li> 0：正常<br><li>1：余额不足<br><li>2：自动停用<br><li>3：手动停用</td>
</tr>
</table>

**响应示例**

```
{
  "id": 1,
  "name": "Agora",
  "country": "CN",
  "area": "CN",
  "appLimit": 10,
  "memberLimit": 10,
  "industry": 12,
  "interest": 1,
  "environment": 1,
  "status": 0
}
```

## 获取公司列表（GET）

**基本信息**

| 请求基本信息 | 描述                           |
| :----------- | :----------------------------- |
| 方法         | GET                            |
| 请求 URL     | BaseUrl/v1/resellers/companies |

**请求参数**

查询参数

| 参数名 | 类型   | 描述                                        |
| :----- | :----- | :------------------------------------------ |
| limit  | number | 查询结果中每页显示的公司个数。默认值为 20。 |
| offset | number | 从查询结果的第几项开始显示。默认值为 0。    |

**请求示例**

```
https://api.agora.io/v1/resellers/companies?limit=20&offset=0
```

**响应参数**

#### 整体结构

| 参数名 | 类型      | 描述       |
| :----- | :-------- | :--------- |
| rows   | Company[] | 公司信息。 |
| count  | number    | 公司数量。 |

#### Company 结构

| 参数名      | 类型   | 描述                                                       |
| :---------- | :----- | :--------------------------------------------------------- |
| id          | number | 公司 ID。                                                  |
| name        | string | 公司名称。                                                 |
| country     | string | 国家。需输入符合 ISO 3166-1 国际标准的二位字母代码。       |
| area        | string | 地区。<br><li>CN：中国<br><li>Non-CN：非中国                       |
| appLimit    | number | 应用数量上限。                    |
| memberLimit | number | 子账号数量上限。                  |
| industry    | string | 行业。详细响应参数描述见[创建公司响应参数](#CreateCompanyReturn)。         |
| interest    | string | 感兴趣的产品。详细响应参数描述见[创建公司响应参数](#CreateCompanyReturn)。 |
| environment | string | 开发平台。详细响应参数描述见[创建公司响应参数](#CreateCompanyReturn)。     |
| status      | number | 公司状态。<br><li>0：正常<br><li>1：余额不足<br><li>2：自动停用<br><li>3：手动停用         |

**响应示例**

```
{
  "rows": [
    {
      "id": 1,
      "name": "Agora",
      "country": "CN",
      "area": "CN",
      "appLimit": 10,
      "memberLimit": 10,
      "industry": 12,
      "interest": 1,
      "environment": 1,
      "status": 0
    }
  ],
  "count": 1
}
```

## 创建新项目（POST）

**基本信息**

| 请求基本信息 | 描述                                                |
| :----------- | :-------------------------------------------------- |
| 方法         | POST                                                |
| 请求 URL     | BaseUrl/v1/resellers/companies/{companyId}/projects |

<div class="alert note">调用此方法创建新项目后，新创建的项目无法直接在线上使用，需经过 3 至 5 分钟同步。</div>

**请求参数**

#### Body 参数

| 参数名 | 类型   | 描述                               |
| :----- | :----- | :--------------------------------- |
| name   | string | （必填）项目名称，长度为 [1,255]。 |

#### 路径参数

| 参数名    | 类型   | 描述              |
| :-------- | :----- | :---------------- |
| companyId | number | （必填）公司 ID。 |

**请求示例**

```
{ 
	 "name": "ProjectX" 
}
```

**响应参数**

| 参数名  | 类型   | 描述                                                         |
| :------ | :----- | :----------------------------------------------------------- |
| id      | string | 项目 ID。                                                    |
| name    | string | 项目名称。                                                   |
| status  | number | 项目状态。<br><li>-1：删除<br><li>0：禁用<br><li>1：激活                             |
| created | string | 项目创建时间。例如：2019-09-24T08:37:38.502Z，表示该项目于2019年9月24日8时37分38.502秒创建。 |

**响应示例**

```
{
  "id": "u_gRV4g0",
  "name": "ProjectX",
  "status": 1,
  "created": "2019-09-25T04:30:41.529Z"
}
```

## 获取项目列表（GET）

**基本信息**

| 请求基本信息 | 描述                                                |
| :----------- | :-------------------------------------------------- |
| 方法         | GET                                                 |
| 请求 URL     | BaseUrl/v1/resellers/companies/{companyId}/projects |

**请求参数**

#### 路径参数

| 参数名    | 类型   | 描述              |
| :-------- | :----- | :---------------- |
| companyId | string | （必填）公司 ID。 |

#### 查询参数

| 参数名 | 类型   | 描述                                        |
| :----- | :----- | :------------------------------------------ |
| limit  | number | 查询结果中每页显示的公司个数。默认值为 20。 |
| offset | number | 从查询结果的第几项开始显示。默认值为 0。    |

**请求示例**

```
https://api.agora.io/v1/resellers/companies/1/projects?limit=20&offset=0
```

**响应参数**

#### 整体结构

| 参数名 | 类型      | 描述       |
| :----- | :-------- | :--------- |
| rows   | Project[] | 项目信息。 |
| count  | number    | 项目数量。 |

#### Project 结构

| 参数名  | 类型   | 描述                                                         |
| :------ | :----- | :----------------------------------------------------------- |
| id      | string | 项目 ID。                                                    |
| name    | string | 项目名称。                                                   |
| status  | number | 项目状态。<br><li>-1：删除<br><li>0：禁用<br><li>1：激活                              |
| created | string | 项目创建时间。例如：2019-09-24T08:37:38.502Z，表示该项目于2019年9月24日8时37分38.502秒创建。 |


**响应示例**

```
{
  "rows": [
    {
      "id": "u_gRV4g0",
      "name": "ProjectX",
      "status": 1,
      "created": "2019-09-25T04:30:41.529Z"
    }
  ],
  "count": 1
}
```

## 开启 UAP 自服务（POST）

**基本信息**

| 请求基本信息 | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| 方法         | POST                                                         |
| 请求 URL     | BaseUrl/v1/resellers/companies/{companyId}/projects/{projectId}/uap |

**请求参数**

#### Body 参数

| 参数名 | 类型   | 描述                                        |
| :----- | :----- | :------------------------------------------ |
| typeId | number | （必填）产品类型。<br><li>1：小程序<br><li>3：推流<br><li>7：云录制 |
| region | number | （必填）地区。<br><li>1：中国<br><li>2：非中国             |

#### 路径参数

| 参数名    | 类型   | 描述              |
| :-------- | :----- | :---------------- |
| companyId | number | （必填）公司 ID。 |
| projectId | string | （必填）项目 ID。 |

**请求示例**

```
{
  "typeId": 1,
  "region": 1
}
```

**响应参数**

| 参数名      | 类型   | 描述                                |
| :---------- | :----- | :---------------------------------- |
| id          | number | 项目 ID。                           |
| typeId      | number | 产品类型。<br><li>1：小程序<br><li>3：推流<br><li>7：云录制 |
| projectName | string | 项目名称。                          |
| status      | number | UAP 自服务状态。<br><li>0：关闭<br><li>1：开启      |
| region      | number | 地区。<br><li>1：中国<br><li>2：非中国              |

**响应示例**

```
{
  "id": 1,
  "typeId": 1,
  "projectName": "ProjectX",
  "status": 0,
  "region": 1
}
```

## 查询公司用量（GET）

**基本信息**

| 请求基本信息 | 描述                                             |
| :----------- | :----------------------------------------------- |
| 方法         | GET                                              |
| 请求 URL     | BaseUrl/v1/resellers/companies/{companyId}/usage |

**请求参数**

#### 路径参数

| 参数名    | 类型   | 描述              |
| :-------- | :----- | :---------------- |
| companyId | number | （必填）公司 ID。 |

#### 查询参数

| 参数     | 类型   | 描述                                                         |
| :------- | :----- | :----------------------------------------------------------- |
| model    | string | （必填）计量模型。可设为：<br><li>duration：时长模型<br><li>transcoding：转码模型<br><li>counter：计数模型<br>如何设置请参考[业务类型和计量模型对照表](#Model)。 |
| business | string | （必填）业务类型。可设为：<br><li>default：默认业务，为 [RTC](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#rtc-sdk)<br><li>recording：本地服务端录制<br><li>cloudRecording：云端录制<br><li>miniapp：小程序<br><li>rtm：实时消息（RTM）<br>如何设置请参考[业务类型和计量模型对照表](#Model)。 |
| fromTs   | number | （必填）开始时间（秒），使用 UTC 时间和 Unix 时间戳。例如：1569196800。 |
| endTs    | number | （必填）结束时间（秒），使用 UTC 时间和 Unix 时间戳。例如：1569801599。 |
| interval | string | （必填）时间间隔。<br><li>daily：天<br><li>monthly：月                       |

<a name="Model"></a>
#### 业务类型和计量模型对照表

| 业务名称       | duration 时长模型    | transcoding 转码模型 | counter 计数模型   |
| :------------- | :------------------- | :------------------- | :----------------- |
| default        | RTC 音视频的时长     | RTC 音视频转码的时长 | N/A                |
| recording      | 本地服务端录制的时长 | N/A                  | N/A                |
| cloudRecording | 云端录制的时长       | N/A                  | N/A                |
| miniapp        | 小程序的时长         | N/A                  | N/A                |
| rtm            | N/A                  | N/A                  | RTM 的日活跃用户数 |

**请求示例**

```
https://api.agora.io/v1/resellers/companies/1/usage?model=duration&business=default&fromTs=1569196800&endTs=1569801599&interval=daily
```

<a name="CompanyUsage"></a>
**响应参数**

#### 整体结构

| 参数名     | 类型   | 描述       |
| :--------- | :----- | :--------- |
| UsageDatum | UsageDatum[] | 用量结构。 |

#### UsageDatum 结构

| 参数名   | 类型    | 描述                      |
| :------- | :------ | :------------------------ |
| dateTime | string  | 查询时间，使用 UTC 时间。 |
| usage    | Usage[] | 用量数据。                |

#### Usage 结构

| 参数名      | 类型   | 描述                                                         |
| :---------- | :----- | :----------------------------------------------------------- |
| duration    | string | 该公司中各产品的时长用量。<ul><li>default（RTC 音视频）</li><ul><li>audioAll：主播和观众音频时长<br><li>videoAll：主播和观众视频总时长<br><li>sdAll：主播和观众 SD 视频总时长<br><li>hdAll：主播和观众 HD 视频总时长<br><li>hdpAll：主播和观众 HDP 视频总时长</ul><li>miniapp（小程序）：</li><ul><li>audioAll：主播和观众音频时长<br><li>videoAll：主播和观众视频总时长</ul><li>recording（本地服务端录制）：</li><ul><li>audioAudience：观众音频时长<br><li>sdAudience：观众 SD 视频时长<br><li>hdAudience：观众 HD 视频时长<br><li>hdpAudience：观众 HDP 视频时长</ul><li>cloudRecording（云端录制）：</li><ul><li>audioAudience：观众音频时长<br><li>sdAudience：观众 SD 视频时长<br><li>hdAudience：观众 HD 视频时长<br><li>hdpAudience：观众 HDP 视频时长</li></ul> |
| transcoding | string | 该公司中 RTC 音视频的转码用量。<ul><li>transcodeDuration（转码时长）：</li><ul><li>audioHostin：音频[连麦](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#连麦)总时长<br><li>avcAudio：音频总时长。<br><li>videoSingleHost：H.264 单主播视频时长。<br><li>avcSd：H.264 SD 视频连麦时长<br><li>avcHd：H.264 HD 视频连麦时长<br><li>avcHdp：H.264 HDP 视频连麦时长<br><li>hevcAudio：H.265 音频连麦时长<br><li>hevcSd：H.265 SD 视频连麦时长<br><li>hevcHd：H.265 HD 视频连麦时长<br><li>hevcHdp：H.265 HDP 视频连麦时长</ul><li>concurrentChannel（转码并发频道数）：</li><ul><li>avcTotal：H.264 视频连麦的转码并发频道数。<br><li>avcTotalHostin：H.264 音视频连麦的转码并发频道数总和。<br><li>avcTotalVideoHostin：H.264 视频连麦的转码并发频道数。<br><li>avcAudio：H.264 音频连麦的转码并发频道数。<br><li>audioHostin：H.264 和 H.265 音频连麦的转码并发数总和。<br><li>videoSingleHost：H.264 单主播视频的转码并发数。<br><li>avcSd：H.264 SD 视频连麦的转码并发频道数。<br><li>avcHd：H.264 HD 视频连麦的转码并发频道数。<br><li>avcHdp：H.264 HDP 视频连麦的转码并发频道数。<br><li>hevcAudio：H.265 音频连麦的转码并发频道数。<br><li>hevcSd：H.265 SD 视频连麦的转码并发频道数。<br><li>hevcHd：H.265 HD 视频连麦的转码并发频道数。<br><li>hevcHdp：H.265 HDP 视频连麦的转码并发频道数。</ul><li>counter（日活跃用户数）：</li><ul><li>rtm（实时消息）：</li><ul><li>dau：日活跃用户数 |

**响应示例**

```
[
  {
    "dateTime": "2019-09-23 00:00:00",
    "usage": {
      "audioAll": 0,
      "videoAll": 0,
      "sdAll": 0,
      "hdAll": 0,
      "hdpAll": 0
    }
  }
]
```

##  查询项目用量（GET）

**基本信息**

| 请求基本信息 | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| 方法         | GET                                                          |
| 请求 URL     | BaseUrl/v1/resellers/companies/{companyId}/projects/{projectId}/usage |

**请求参数**

#### 路径参数

| 参数名    | 类型   | 描述              |
| :-------- | :----- | :---------------- |
| companyId | number | （必填）公司 ID。 |
| projectId | string | （必填）项目 ID。 |

#### 查询参数

| 参数     | 类型   | 描述                                                         |
| :------- | :----- | :----------------------------------------------------------- |
| model    | string | （必填）计量模型。可设为：<br><li>duration：时长模型<br><li>transcoding：转码模型<br><li>counter：计数模型<br>如何设置请参考[业务类型和计量模型对照表](#Model2)。 |
| business | string | （必填）业务类型。可设为：<br><li>default：默认业务，为 [RTC](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#rtc-sdk<br><li>)recording：本地服务端录制<br><li>cloudRecording：云端录制<br><li>miniapp：小程序<br><li>rtm：实时消息（RTM）<br>如何设置请参考[业务类型和计量模型对照表](#Model2)。 |
| fromTs   | number | （必填）开始时间（秒），使用 UTC 时间和 Unix 时间戳。例如：1569196800。 |
| endTs    | number | （必填）结束时间（秒），使用 UTC 时间和 Unix 时间戳。例如：1569801599。 |
| interval | string | （必填）时间间隔。<br><li>daily：天<br><li>monthly：月                       |

<a name="Model2"></a>
#### 业务类型和计量模型对照表

| 业务名称       | duration 时长模型    | transcoding 转码模型 | counter 计数模型   |
| :------------- | :------------------- | :------------------- | :----------------- |
| default        | RTC 音视频的时长     | RTC 音视频转码的时长 | N/A                |
| recording      | 本地服务端录制的时长 | N/A                  | N/A                |
| cloudRecording | 云端录制的时长       | N/A                  | N/A                |
| miniapp        | 小程序的时长         | N/A                  | N/A                |
| rtm            | N/A                  | N/A                  | RTM 的日活跃用户数 |

**请求示例**

```
https://api.agora.io/v1/resellers/companies/1/projects/1/usage?model=duration&business=default&fromTs=1569196800&endTs=1569801599&interval=daily
```

**响应参数**

#### 整体结构

| 参数名     | 类型   | 描述       |
| :--------- | :----- | :--------- |
| UsageDatum | Object | 用量结构。 |

#### UsageDatum 结构

| 参数名   | 类型    | 描述                      |
| :------- | :------ | :------------------------ |
| dateTime | string  | 查询时间，使用 UTC 时间。 |
| usage    | Usage[] | 用量数据。                |

#### Usage 结构

| 参数名      | 类型   | 描述                                                         |
| :---------- | :----- | :----------------------------------------------------------- |
| duration    | string | 该公司中各产品的时长用量。<br><li>default（RTC 音视频）<br><li>miniapp（小程序）<br><li>recording（本地服务端录制）<br><li>cloudRecording（云端录制）<br>详细响应参数描述见[查询公司用量](#CompanyUsage)。 |
| transcoding | string | 该公司中 RTC 音视频的转码用量。<br><li>transcodeDuration（转码时长）<br><li>concurrentChannel（转码并发频道数）<br><li>counter（日活跃用户数）<br>详细响应参数描述见[查询公司用量](#CompanyUsage)。 |

**响应示例**

```
[
  {
    "dateTime": "2019-09-23 00:00:00",
    "usage": {
      "audioAll": 0,
      "videoAll": 0,
      "sdAll": 0,
      "hdAll": 0,
      "hdpAll": 0
    }
  }
]
```

##  查询日账单（GET）

**基本信息**

| 请求基本信息 | 描述                                       |
| :----------- | :----------------------------------------- |
| 方法         | GET                                        |
| 请求 URL     | BaseUrl/v1/resellers/companies/daily-bills |

**请求参数**

#### Body 参数

| 参数名     | 类型   | 描述                                                         |
| :--------- | :----- | :----------------------------------------------------------- |
| period     | string | <br><li>指定账单周期，字符串格式为 yyyyMMdd，例如：20190905。<br><li>此周期按 UTC 时间计算， （默认）最新的账单只能获取到当前 UTC 日期 - 2，例如：当前日期是 UTC 20190905，最新的账单日期默认是 2019 年 9 月 5 日。 |
| pageNumber | number | 指定查询页的记录数，默认为第一页。由于返回数据可能较大，采用将数据分页的方式返回记录数，每页最大的记录数为 1000。 |

**请求示例**

```
https://api.agora.io/v1/resellers/companies/daily-bills?period=20190901&pageNumer=1

```

**响应参数**

整体结构

| 参数名       | 类型       | 描述                              |
| :----------- | :--------- | :-------------------------------- |
| `totalSize`  | number     | 总记录数。                        |
| `pageSize`   | number     | 当前页的记录数，最大为 1000。     |
| `pageNumber` | number     | 页码。                            |
| `hasMore`    | boolean    | 是否有下一页。<br><li>true：有<br><li>false：没有 |
| `elements`   | Elements[] | 账单数据。                        |

Elements 结构

| 参数名      | 类型    | 描述                       |
| :---------- | :------ | :------------------------- |
| `companyId` | number  | 公司 ID。                  |
| `projectId` | string  | 项目 ID。                  |
| `amount`    | decimal | 金额。<br>**Note:**<br>若返回数据为 0，有以下两种可能：<ul><li>该公司未消耗用量。请使用用量接口查看该公司用量情况。<br><li>该公司消耗的用量未超过免费额度，不计费。                    |
| `currency`  | string  | 货币类型。<br><li>CNY：人民币<br><li>USD：美元 |

**响应示例**

```
{
    "totalSize": 10041,
    "pageSize": 2,
    "pageNumber": 1,
    "hasMore": true,
    "elements": [
        {
            "companyId": 10001
            "projectId": "10013"
            "amount": 100.1
            "currency": "CNY"
        },
        {
            "companyId": 10001
            "projectId": "10014"
            "amount": 200.2
            "currency": "CNY"
        }
    ]
}
```

## 相关文档

你可以查看[代理商控制台概览](https://docs.agora.io/cn/Agora%20Platform/reseller_console_guide?platform=All%20Platforms)，了解如何通过代理商控制台查看并管理公司。
