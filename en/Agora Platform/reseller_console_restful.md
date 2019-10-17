
---
title: Agora Reseller Console RESTful API
description: 
platform: All Platforms
updatedAt: Thu Oct 17 2019 01:58:05 GMT+0800 (CST)
---
# Agora Reseller Console RESTful API
## Signature

<div class="alert note">Before using the RESTful APIs, ensure that you have created an Agora Reseller Console account. If you don't have the account, contact sales@agora.io or  <a href="https://dashboard.agora.io/support">submit a ticket</a > in Agora Dashboard to apply for an account.</div>

The RESTful API only supports HTTPS, and the user must pass the `Authorization` field in the Basic HTTP request header for signature.

### 1. Create a string to sign

 Pass the GMT time and UUID in code to create a string to sign. See the parameter description and the pseudocode as follow:

| Parameter        | Type   | Description                                                  |
| :--------------- | :----- | :----------------------------------------------------------- |
| x-date       | String | The GMT time defined in the HTTP 1.1 protocol. For example: Wed, 11 Sep 2019 14:28:28 GMT.<br>**Note**<br>Ensure that the time you set in the machine is the GMT time. |
| x-request-id | String | UUID. The UUID includes 32 hexadecimal digits, displayed in 5 groups separated by hyphens, in the form 8-4-4-4-12 for a total of 36 characters (32 alphanumeric characters and 4 hyphens). For example: 4c18781f-16b2-43c5-9281-2eddfd313bd8. |

```
StringToSign = "x-date: " + {gmtTime} + "\n" + "x-request-id: " + {uuid}
```

### 2. Calculate the signature

Pass the Access Key Secret and the string to sign in the code to calculate the signature. Log in to [Agora Reseller Console](http://reseller.agora.io/), click **Account Name > Setting** in the top navigation menu to enter the **Setting** interface. You can generate the Access Key Secret and the Access Key ID in **API Key**.

<div class="alert note">The Access Key Secret and the Access Key ID are specific only to RESTful API access.</div>

Agora encrypts the signature by a keyed hash function, and strengthens the security of the signature. See the parameter description and the pseudocode as follow:

| Parameter       | Type   | Description                                                  |
| :-------------- | :----- | :----------------------------------------------------------- |
| accessKeySecret | String | The Access Key Secret generated in Agora Reseller Console. The string format should be smaller than 40 bytes. Supports characters: <br><li>The 26 lowercase English letters: a to z<br><li>The 26 uppercase English letters: A to Z<br><li>The 10 numbers: 0 to 9</br> |
| StringToSign    | String | The string to sign which created in the step 1.              |

```
Signature = HexEncode(HAMC(accessKeySecret, StringToSign))
```

### 3. Add the signature to the HTTPS request

Pass the Access Key ID and the signature in code to add the signature in the `Authorization` field of the HTTPS request header. Ensure that the order appointed in the `headers` filed of this request is the same as the header of the signature. 

See the parameter description and the pseudocode as follow:

| Parameter   | Type   | Description                                                  |
| :---------- | :----- | :----------------------------------------------------------- |
| accessKeyId | String | The Access Key ID generated in Agora Reseller Console. The string format should be smaller than 40 bytes. Supports characters:<br><li>The 26 lowercase English letters: a to z<br><li>The 26 uppercase English letters: A to Z<br><li>The 10 numbers: 0 to 9</br> |
| Signature   | String | The signature which calculated in the step 2.                |

```
Authorization: hmac username="{accessKeyId}", algorithm="hmac-sha256", headers="x-date x-request-id", signature="{Signature}" 
```

### Sample code

This session shows the sample codes to get the `Authorization` filed.

<details>
	<summary><font color="#3ab7f8">cURL</font></summary>
	
``` cURL
curl -i -X GET https://localhost:8443/benchmark \ 
-H "X-Date: Wed, 11 Sep 2019 16:21:09 GMT" \  
-H "X-Request-ID: 70a0a14b-f38c-4aaf-aa46-04d8fae3e38c" \  
-H 'Authorization: hmac username="ccc", algorithm="hmac-sha256", headers="x-date x-request-id", signature="nu/RSu8rOCm8Y6kbr7iqHEq6rukF+MtXmz8Ds26GABI="'
```
</details>

<details>
	<summary><font color="#3ab7f8">JavaL</font></summary>

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
	
## EndPoint

All requests should be sent to BaseUrl: [https://api.agora.io](https://api.agora.io/).

- Request: Send all parameters using JSON format, with content type: application/json.

- Response: The response content is in JSON format. The response status is defined as follows:

  | Status  | Description                                          |
  | :------ | :--------------------------------------------------- |
  | 200/201 | Request handle successful                            |
  | 400     | Bad request                               |
  | 401     | Unauthorized (incorrect signature)                   |
  | 403     | No permission to access                              |
  | 404     | Wrong API invoked                                    |
  | 429     | Too many requests                               |
  | 500     | Internal error within the Agora RESTful API service. |

  

## Create a company (POST)

**Basic information**

| Basic information | Description                    |
| :---------------- | :----------------------------- |
| Method            | POST                           |
| Request URL       | BaseUrl/v1/resellers/companies |

**Request parameter**

#### Body parameter

| Parameter              | Type    | Description                                                  |
| :--------------------- | :------ | :----------------------------------------------------------- |
| companyName            | String  | (Mandatory) Company name, with the value range of [1,20].    |
| email                  | String  | (Mandatory) Email, used for creating the Dashboard account.  |
| firstName              | String  | (Mandatory) First name, with the value range of [1,255].     |
| lastName               | String  | (Mandatory) Last name, with the value range of [1,255].      |
| country                | String  | (Mandatory) Country. The two-letter country codes defined in the ISO 3166-1 standard. |
| area                   | String  | (Mandatory) Region. <br><li>CN: China<br><li>Non-CN: Outside of China</br>        |
| appLimit               | Number  | The limit of the application number, with the value of more than 1. The default value is 10. |
| memberLimit            | Number  | The limit of the member account number, with the value of more than 1. The default value is 10. |
| industry               | String  | Industry. <br><li>1: Customer service<br><li>2: E-commerce<br><li>3: Education<br><li>4: Enterprise<br><li>5: Finance<br><li>6: Gaming<br><li>7: IOT<br><li>8: Medical & Health<br><li>9: Human resource<br><li>10: Social / Pan entertainment<br><li>11: System integration<br><li>12: (Default) Other<br><li>13: Live broadcast<br><li>14: Enterprise collaboration / Contact center<br><li>15: Housekeeping / O2O<br><li>16: Software outsourcing<br><li>17: Individual developer<br><li>18: Call center<br><li>19: AR/VR/AI</br> |
| interest               | String  | Interested product. <br><li>1: (Default) Other<br><li>2: Video<br><li>3: Audio<br><li>4: Live broadcast<br><li>5: Gaming</br> |
| environment            | String  | Platform. <br><li>1: (Default) Other<br><li>2: Web<br><li>3: Android<br><li>4: iOS<br><li>5: Windows<br><li>6: macOS<br><li>7: Mini app<br><li>8: Linux</br> |
| sendPasswordResetEmail | Boolean | Whether to send the password reset email or not. <br><li>true: Send<br><li>false: (Default) Not send</br> |

**Request sample**

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
**Response parameter**

| Parameter   | Type   | Description                                                  |
| :---------- | :----- | :----------------------------------------------------------- |
| id          | Number | Company ID                                                   |
| name        | String | Company name.                                                |
| country     | String | Country. The two-letter country codes defined in the ISO 3166-1 standard. |
| area        | String | Region.<br><li>CN: China<br><li>Non-CN: Out of China</br>                        |
| appLimit    | Number | The limit of the application number.                         |
| memberLimit | Number | The limit of the member account number.                      |
| industry    | String | Industry. <br><li>1: Customer service<br><li>2: E-commerce<br><li>3: Education<br><li>4: Enterprise<br><li>5: Finance<br><li>6: Gaming<br><li>7: IOT<br><li>8: Medical & Health<br><li>9: Human resource<br><li>10: Social / Pan entertainment<br><li>11: System integration<br><li>12: Other<br><li>13: Live broadcast<br><li>14: Enterprise collaboration / Contact center<br><li>15: Housekeeping / O2O<br><li>16: Software outsourcing<br><li>17: Individual developer<br><li>18: Call center<br><li>19: AR/VR/AI</br> |
| interest    | String | Interested product. <br><li>1: Other<br><li>2: Video<br><li>3: Audio<br><li>4: Live broadcast<br><li>5: Gaming</br> |
| environment | String | Platform. <br><li>1: Other<br><li>2: Web<br><li>3: Android<br><li>4: iOS<br><li>5: Windows<br><li>6: macOS<br><li>7: Mini app<br><li>8: Linux</br> |
| status      | Number | Company status. <br><li>0: Normal<br><li>1: Insufficient balance<br><li>2: Automatically suspended<br><li>3: Manually suspended</br> |

**Response sample**

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

## Fetch the company list (GET)

**Basic information**

| Basic information | Description                    |
| :---------------- | :----------------------------- |
| Method            | GET                            |
| Request URL       | BaseUrl/v1/resellers/companies |

**Request parameter**

#### Query parameter

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| limit     | Number | The limit of the company number which shows in each page of the query result. The default value is 20. |
| offset    | Number | The place where the query result starts to show. The default value is 0. |

**Request sample**

```
https://api.agora.io/v1/resellers/companies?limit=20&offset=0
```

**Response parameter**

#### Whole structure

| Parameter | Type      | Description          |
| :-------- | :-------- | :------------------- |
| rows      | JSONArray] | Company information. |
| count     | Number    | Company number.      |

#### Company structure

| Parameter   | Type   | Description                                                  |
| :---------- | :----- | :----------------------------------------------------------- |
| id          | Number | Company ID                                                   |
| name        | String | Company name.                                                |
| country     | String | Country. The two-letter country codes defined in the ISO 3166-1 standard. |
| area        | String | Region. <br><li>CN: China<br><li>Non-CN: Outside of China</br>                    |
| appLimit    | Number | The limit of the application number.                         |
| memberLimit | Number | The limit of the member account number.                      |
| industry    | String | Industry. See details in the Response parameter of [Create a company](#CreateCompanyReturn). |
| interest    | String | Interested product. See details in the Response parameter of [Create a company](#CreateCompanyReturn). |
| environment | String | Platform. See details in the Response parameter of [Create a company](#CreateCompanyReturn). |
| status      | Number | Company status. <br><li>0: Normal<br><li>1: Insufficient balance<br><li>2: Automatically suspended<br><li>3: Manually suspended</br> |

**Response sample**

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

## Create a project (POST)

**Basic information**

| Basic information | Description                                         |
| :---------------- | :-------------------------------------------------- |
| Method            | POST                                                |
| Request URL       | BaseUrl/v1/resellers/companies/{companyId}/projects |

<div class="alert note">After creating a project by using this method, you can not use the project immediately. You need to wait for 3 to 5 minutes to synchronize the data online.</div>

**Request parameter**

#### Body parameter

| Parameter | Type   | Description                                                |
| :-------- | :----- | :--------------------------------------------------------- |
| name      | String | (Mandatory) Project name, with the value range of [1,255]. |

#### Path parameter

| Parameter | Type   | Description             |
| :-------- | :----- | :---------------------- |
| companyId | Number | (Mandatory) Company ID. |

**Request sample**

```
{ 
	 "name": "ProjectX" 
}
```



**Response parameter**

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| id        | String | Project ID.                                                  |
| name      | String | Project name.                                                |
| status    | Number | Project status.<br><li>-1: Deleted<br><li>0: Disabled<br><li>1: Enabled</br>              |
| created   | String | Project created time. For example: 2019-09-24T08:37:38.502Z. |

**Response sample**

```
{
  "id": "u_gRV4g0",
  "name": "ProjectX",
  "status": 1,
  "created": "2019-09-25T04:30:41.529Z"
}
```

## Fetch the project list (GET)

**Basic information**

| Basic information | Description                                         |
| :---------------- | :-------------------------------------------------- |
| Method            | GET                                                 |
| Request URL       | BaseUrl/v1/resellers/companies/{companyId}/projects |

**Request parameter**

#### Path parameter

| Parameter | Type   | Description             |
| :-------- | :----- | :---------------------- |
| companyId | Number | (Mandatory) Company ID. |

#### Query parameter

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| limit     | Number | The limit of the company number which shows in each page of the query result. The default value is 20. |
| offset    | Number | The place where the query result starts to show. The default value is 0. |


**Request sample**

```
https://api.agora.io/v1/resellers/companies/1/projects?limit=20&offset=0
```



**Response parameter**

#### Whole structure

| Parameter | Type      | Description          |
| :-------- | :-------- | :------------------- |
| rows      | JSONArray | Project structure. |
| count     | Number    | Project number.      |

#### Project structure

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| id        | String | Project ID                                                   |
| name      | String | Project name.                                                |
| status    | Number | Project status.<br><li>-1: Deleted<br><li>0: Disabled<br><li>1: Enabled</br>              |
| created   | String | Project created time. For example: 2019-09-24T08:37:38.502Z. |


**Response sample**

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

## Enable UAP self-service (POST)

**Basic information**

| Basic information | Description                                                  |
| :---------------- | :----------------------------------------------------------- |
| Method            | POST                                                         |
| Request URL       | BaseUrl/v1/resellers/companies/{companyId}/projects/{projectId}/uap |

**Request parameter**

#### Body parameter

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| typeId    | Number | (Mandatory) Product type. <br><li>1: Mini app<br><li>3: CDN live streaming<br><li>7: Cloud recording</br> |
| region    | Number | (Mandatory) Region. <br><li>1: China<br><li>2: Outside of China</br>                  |

#### Path parameter

| Parameter | Type   | Description             |
| :-------- | :----- | :---------------------- |
| companyId | Number | (Mandatory) Company ID. |
| projectId | String | (Mandatory) Project ID. |

**Request sample**

```
{
  "typeId": 1,
  "region": 1
}
```


**Response parameter**

| Parameter   | Type   | Description                                                  |
| :---------- | :----- | :----------------------------------------------------------- |
| id          | Number | Project ID.                                                  |
| typeId      | Number | Product type. <br><li>1: Mini app<br><li>3: CDN live streaming<br><li>7: Cloud recording</br> |
| projectName | String | Project name.                                                |
| status      | Number | UAP self-service status.<br><li>0: Disabled<br><li>1: Enabled</br>                |
| region      | Number | Region. <br><li>1: China<br><li>2: Outside of China</br>                              |

**Response sample**

```
{
  "id": 1,
  "typeId": 1,
  "projectName": "ProjectX",
  "status": 0,
  "region": 1
}
```

## Fetch the company usage (GET)

**Basic information**

| Basic information | Description                                      |
| :---------------- | :----------------------------------------------- |
| Method            | GET                                              |
| Request URL       | BaseUrl/v1/resellers/companies/{companyId}/usage |

**Request parameter**

#### Path parameter

| Parameter | Type   | Description             |
| :-------- | :----- | :---------------------- |
| companyId | Number | (Mandatory) Company ID. |

#### Query parameter

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| model     | String | (Mandatory) Measurement model. <br><li>duration<br><li>transcoding<br><li>counter <br>Refer to [Business model and measurement model comparison table](#Model).</br> |
| business  | String | (Mandatory) Business model. <br><li>default: [RTC](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#rtc-sdk)<br><li>recording: On-premise recording<br><li>cloudRecording: Cloud recording<br><li>miniapp: Mini app<br><li>rtm: Real-time messaging (RTM)  <br>Refer to [Business model and measurement model comparison table](#Model).</br> |
| fromTs    | Number | (Mandatory) Start time (sec), using UTC time and Unix timestamp. For example: 1569196800. |
| endTs     | Number | (Mandatory) End time (sec), using UTC time and Unix timestamp. For example: 1569801599. |
| interval  | String | (Mandatory) Time interval. <br><li>daily<br><li>monthly</br>                      |

<a name="Model"></a>
#### Business model and measurement model comparison table

| Model          | duration                         | transcoding                 | counter                           |
| :------------- | :------------------------------- | :-------------------------- | :-------------------------------- |
| default        | Duration of RTC audio and video  | Duration of RTC transcoding | N/A                               |
| recording      | Duration of on-premise recording | N/A                         | N/A                               |
| cloudRecording | Duration of cloud recording      | N/A                         | N/A                               |
| miniapp        | Duration of mini app             | N/A                         | N/A                               |
| rtm            | N/A                              | N/A                         | Daily activate user number of RTM |


**Request sample**

```
https://api.agora.io/v1/resellers/companies/1/projects/1/usage?model=duration&business=default&fromTs=1569196800&endTs=1569801599&interval=daily
```

<a name="CompanyUsage"></a>
**Response parameter**

#### Whole structure

| Parameter  | Type   | Description      |
| :--------- | :----- | :--------------- |
| UsageDatum | JSONArray | UsageDatum structure. |

#### UsageDatum structure

| Parameter | Type    | Description                 |
| :-------- | :------ | :-------------------------- |
| dateTime  | String  | Query time, using UTC time. |
| usage     | JSONObject | Usage structure.                      |

#### Usage structure

| Parameter   | Type   | Description                                                  |
| :---------- | :----- | :----------------------------------------------------------- |
| duration    | String | The duration of each product.<ul><li>default (RTC):</li><ul><li>audioAll: Total audio duration<br><li>videoAll: Total video duration<br><li>sdAll: SD video duration<br><li>hdAll: HD video duration<br><li>hdpAll: HDP video duration</ul><li>miniapp (mini app):</li><ul><li>audioAll: Total audio duration<br><li>videoAll: Total video duration</ul><li>recording (on-premise recording):</li><ul><li>audioAudience: Audience total audio duration<br><li>sdAudience: Audience SD video duration<br><li>hdAudience: Audience HD video duration<br><li>hdpAudience: Audience HDP video duration</ul><li>cloudRecording (cloud recording):</li><ul><li>audioAudience: Audience total audio duration<br><li>sdAudience: Audience SD video duration<br><li>hdAudience: Audience HD video duration<br><li>hdpAudience: Audience HDP video duration</br> |
| transcoding | String | The usage of transcoding in RTC.<ul><li>transcodeDuration (transcoding):</li><ul><li>audioHostin: Audio hosting-in duration<br><li>avcAudio: Total audio duration<br><li>videoSingleHost: Single host H.264 video duration<br><li>avcSd: H.264 SD video hosting-in duration<br><li>avcHd: H.264 HD video hosting-in duration<br><li>avcHdp: H.264 HDP video hosting-in duration<br><li>hevcAudio: H.265 audio hosting-in duration<br><li>hevcSd: H.265 SD video hosting-in duration<br><li>hevcHd: H.265 HD video hosting-in duration<br><li>hevcHdp: H.265 HDP video hosting-in duration</ul><li>concurrentChannel (concurrent channel):</li><ul><li>avcTotal: Total concurrent channel<br><li>avcTotalHostin: Total H.264 hosting-in concurrent channel<br><li>avcTotalVideoHostin: H.264 video hosting-in concurrent channel<br><li>avcAudio: H.264 audio hosting-in concurrent channel<br><li>audioHostin: Total audio hosting-in concurrent channel<br><li>videoSingleHost: Single host H.264 video concurrent channel<br><li>avcSd: H.264 SD video hosting-in concurrent channel<br><li>avcHd: H.264 HD video hosting-in concurrent channel<br><li>avcHdp: H.264 HDP video hosting-in concurrent channel<br><li>hevcAudio: H.265 audio hosting-in concurrent channel<br><li>hevcSd: H.265 SD video hosting-in concurrent channel<br><li>hevcHd: H.265 HD video hosting-in concurrent channel<br><li>hevcHdp: H.265 HDP video hosting-in concurrent channel</ul><li>counter (daily activate user):</li><ul><li>rtm (real-time mesagging)</li><ul><li>dau: Daily activate user</br> |

**Response sample**

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

## Fetch the project usage (GET)

**Basic information**

| Basic information | Description                                                  |
| :---------------- | :----------------------------------------------------------- |
| Method            | GET                                                          |
| Request URL       | BaseUrl/v1/resellers/companies/{companyId}/projects/{projectId}/usage |

**Request parameter**

#### Path parameter

| Parameter | Type   | Description             |
| :-------- | :----- | :---------------------- |
| companyId | Number | (Mandatory) Company ID. |
| projectId | String | (Mandatory) Project ID. |

#### Query parameter

| Parameter | Type   | Description                                                  |
| :-------- | :----- | :----------------------------------------------------------- |
| model     | String | (Mandatory) Measurement model. <br><li>duration<br><li>transcoding<br><li>counter <br>Refer to [Business model and measurement model comparison table](#Model2).</br> |
| business  | String | (Mandatory) Business model. <br><li>default: [RTC](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#rtc-sdk)<br><li>recording: On-premise recording<br><li>cloudRecording: Cloud recording<br><li>miniapp: Mini app<br><li>rtm: Real-time messaging (RTM) Refer to [Business model and measurement model comparison table](#Model2).</br> |
| fromTs    | Number | (Mandatory) Start time (sec), using UTC time and Unix timestamp. For example: 1569196800. |
| endTs     | Number | (Mandatory) End time (sec), using UTC time and Unix timestamp. For example: 1569801599. |
| interval  | String | (Mandatory) Time interval. <br><li>daily<br><li>monthly </br>                     |

<a name="Model2"></a>
#### Business model and measurement model comparison table

| Model          | duration                         | transcoding                 | counter                           |
| :------------- | :------------------------------- | :-------------------------- | :-------------------------------- |
| default        | Duration of RTC audio and video  | Duration of RTC transcoding | N/A                               |
| recording      | Duration of on-premise recording | N/A                         | N/A                               |
| cloudRecording | Duration of cloud recording      | N/A                         | N/A                               |
| miniapp        | Duration of mini app             | N/A                         | N/A                               |
| rtm            | N/A                              | N/A                         | Daily activate user number of RTM |

#### **Request sample**

```
https://api.agora.io/v1/resellers/companies/1/projects/1/usage?model=duration&business=default&fromTs=1569196800&endTs=1569801599&interval=daily
```



**Response parameter**

#### Whole structure

| Parameter  | Type   | Description      |
| :--------- | :----- | :--------------- |
| UsageDatum | JSONArray | UsageDatum structure. |

#### UsageDatum structure

| Parameter | Type    | Description                 |
| :-------- | :------ | :-------------------------- |
| dateTime  | String  | Query time, using UTC time. |
| usage     | JSONObject | Usage structure.                      |

#### Usage structure

| Parameter   | Type   | Description                                                  |
| :---------- | :----- | :----------------------------------------------------------- |
| duration    | String | The duration of each product.<br><li>default: RTC<br><li>miniapp: Mini app<br><li>recording: On-premise recording<br><li>cloudRecording: Cloud recording <br>Refer to [Fetch the company usage](#CompanyUsage).</br> |
| transcoding | String | The usage of transcoding in RTC.<br><li>transcodeDuration: Transcoding<br><li>concurrentChannel: Concurrent channel<br><li>counter: Daily activate user <br>Refer to [Fetch the company usage](#CompanyUsage).</br> |


**Response sample**

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

## Fetch the daily bill (GET)

**Basic information**

| Basic information | Description                                |
| :---------------- | :----------------------------------------- |
| Method            | GET                                        |
| Request URL       | BaseUrl/v1/resellers/companies/daily-bills |

**Request parameter**

#### Body parameter

| Parameter  | Type   | Description                                                  |
| :--------- | :----- | :----------------------------------------------------------- |
| period     | String | <li>Billing cycle, with the string format of yyyyMMdd. For example: 20190905.<br><li>Time specified in UTC is the basis for the billing cycle. By default, you can only fetch the bill of the current UTC date minus two days. For example: if the current date is UTC 20190930, the latest bill date available is for September 28, 2019.</br> |
| pageNumber | Number | Record number of the query page, the default value is the first page. Because of the big data amount, Agora adopts the data paging method to return the record number. The maximum value is 1000. |

**Request sample**

```
https://api.agora.io/v1/resellers/companies/daily-bills?period=20190901&pageNumer=1
```



**Response parameter**

#### Whole structure

| Parameter    | Type       | Description                                                  |
| :----------- | :--------- | :----------------------------------------------------------- |
| `totalSize`  | Number     | Total record number.                                         |
| `pageSize`   | Number     | Record number of current page, with the value less than 1000. |
| `pageNumber` | Number     | Page number.                                                 |
| `hasMore`    | Boolean    | Whether there is more pages or not. <br><li>ttrue: There is<br><li>tfalse: There is not</br> |
| `elements`   | JSONArray | Elements structure.                                                   |

#### Elements structure

| Parameter   | Type    | Description                                                  |
| :---------- | :------ | :----------------------------------------------------------- |
| `companyId` | Number  | Company ID.                                                  |
| `projectId` | String  | Project ID.                                                  |
| `amount`    | Decimal | Amount of money.<br>**Note**</br>When it returns 0 in the `amount` parameter, there are two reasons.<ul><li>The project doesn't consume usage.<br><li>The usage of the project is less than the free threshold.</br> |
| `currency`  | String  | Currency.<br><li>CNY: Chinese Yuan<br><li>USD: United States Dollar </br>       |

#### ** Response sample**

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

## Related documentation

About how to manage company and check the usage in Agora Reseller Console, see [Reseller Console Overview](http://link/) for details.
