
---
title: IM 返回值说明
description: 
platform: Unity
updatedAt: Fri Jul 26 2019 04:28:59 GMT+0800 (CST)
---
# IM 返回值说明
Agora SDK 在调用 IM RESTful 接口时，可能会返回一下状态码或业务返回码。相关描述及解析见下文。

## HTTP 状态码

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>code</strong></td>
<td><strong>描述</strong></td>
<td><strong>详细解释</strong></td>
</tr>
<tr><td>200</td>
<td>成功</td>
<td>成功</td>
</tr>
<tr><td>400</td>
<td>错误请求</td>
<td>该请求是无效的，详细的错误信息会说明原因</td>
</tr>
<tr><td>401</td>
<td>未授权</td>
<td>验证失败，详细的错误信息会说明原因</td>
</tr>
<tr><td>403</td>
<td>服务器拒绝请求</td>
<td>被拒绝调用，详细的错误信息会说明原因</td>
</tr>
<tr><td>404</td>
<td>未找到</td>
<td>服务器找不到请求的地址</td>
</tr>
<tr><td>405</td>
<td>方法禁用</td>
<td>群容量超出上限，禁止调用</td>
</tr>
<tr><td>429</td>
<td>太多的请求</td>
<td>超出的调用频率限制，详细的错误信息会说明原因</td>
</tr>
<tr><td>500</td>
<td>服务器内部错误</td>
<td>服务器内部出错了，请联系我们尽快解决问题</td>
</tr>
<tr><td>504</td>
<td>网关超时</td>
<td>服务器在运行时，本次请求响应超时，请稍后重试</td>
</tr>
</tbody>
</table>



## 业务返回码

<table>
<colgroup>
<col/>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>code</td>
<td>HTTP 状态码</td>
<td>描述</td>
<td>详细解释                                                            HTTP 状态码</td>
</tr>
<tr><td>404</td>
<td>404</td>
<td>未找到</td>
<td>服务器找不到请求的地址</td>
</tr>
<tr><td>1000</td>
<td>500</td>
<td>服务器内部错误</td>
<td>服务器内部逻辑错误，请稍后重试</td>
</tr>
<tr><td>1001</td>
<td>401</td>
<td>App Secret 错误</td>
<td>App Key 与 App Secret 不匹配</td>
</tr>
<tr><td>1002</td>
<td>400</td>
<td>参数错误</td>
<td>参数错误，详细的描述信息会说明</td>
</tr>
<tr><td>1003</td>
<td>400</td>
<td>无 POST 数据</td>
<td>没有 POST 任何数据</td>
</tr>
<tr><td>1004</td>
<td>401</td>
<td>验证签名错误</td>
<td>验证签名错误</td>
</tr>
<tr><td>1005</td>
<td>400</td>
<td>参数长度超限</td>
<td>参数长度超限，详细的描述信息会说明</td>
</tr>
<tr><td>1006</td>
<td>401</td>
<td>App 被锁定或删除</td>
<td>App 被锁定或删除                                                      401</td>
</tr>
<tr><td>1007</td>
<td>401</td>
<td>被限制调用</td>
<td>该方法被限制调用，详细的描述信息会说明</td>
</tr>
<tr><td>1008</td>
<td>429</td>
<td>调用频率超限</td>
<td>调用频率超限，详细的描述信息会说明，广播消息未开通时也会返回此状态码</td>
</tr>
<tr><td>1009</td>
<td>430</td>
<td>服务未开通</td>
<td>未开通该服务，请联系 <a href="mailto:sales%40agora.io">sales<span>@</span>agora<span>.</span>io</a> 了解更多信息</td>
</tr>
<tr><td>1015</td>
<td>200</td>
<td>删除的数据不存在</td>
<td>要删除的保活聊天室 ID 不存在</td>
</tr>
<tr><td>1016</td>
<td>403</td>
<td>设置保活聊天室个数超限</td>
<td>设置的保活聊天室个数超限</td>
</tr>
<tr><td>1050</td>
<td>504</td>
<td>内部服务超时</td>
<td>内部服务响应超时</td>
</tr>
<tr><td>2007</td>
<td>403</td>
<td>测试用户数量超限</td>
<td>测试用户数量超限</td>
</tr>
</tbody>
</table>




