
---
title: Server RESTful API
description: An overview of the RESTful APIs provided on the Agora server side.
platform: All Platforms
updatedAt: Fri Jul 03 2020 02:15:08 GMT+0800 (CST)
---
# Server RESTful API
In addition to client APIs, Agora also provides RESTful APIs which enable you to use HTTPS requests to get, put, post, and delete project and usage-related data on the server side.

Visit our API documentation at [Server RESTful API](https://docs.agora.io/en/rtc/restfulapi/) ![](https://web-cdn.agora.io/docs-files/1583736328279). This documentation contains detailed help for each Server RESTful API and its parameters, and provides the **Try it out** function which allows you to send RESTful API requests and receive responses directly on the web page.

All requests are sent to the BaseUrl: `https://api.agora.io`.

## API Overview

### Project management

| Request URL                                                  | Method | Function                                         |
| :----------------------------------------------------------- | :----- | :----------------------------------------------- |
| [BaseUrl/dev/v1/project](https://docs.agora.io/en/rtc/restfulapi/#/Project%20management/createProject) | POST   | Creates a project.                               |
| [BaseUrl/dev/v1/project](https://docs.agora.io/en/rtc/restfulapi/#/Project%20management/getProject) | GET    | Gets a specified project.                        |
| [BaseUrl/dev/v1/project](https://docs.agora.io/en/rtc/restfulapi/#/Project%20management/deleteProject) | DELETE | Deletes a specified project.                     |
| [BaseUrl/dev/v1/projects](https://docs.agora.io/en/rtc/restfulapi/#/Project%20management/projects) | GET    | Gets all projects.                               |
| [BaseUrl/dev/v1/project_status](https://docs.agora.io/en/rtc/restfulapi/#/Project%20management/changeProjectStatus) | POST   | Disables or enables a project.                   |
| [BaseUrl/dev/v1/recording_config](https://docs.agora.io/en/rtc/restfulapi/#/Project%20management/setRecordingServer) | POST   | Sets the IP of the recording server.             |
| [BaseUrl/dev/v1/signkey](https://docs.agora.io/en/rtc/restfulapi/#/Project%20management/changeSignKey) | POST   | Enables or disables the primary App Certificate. |
| [BaseUrl/dev/v1/reset_signkey](https://docs.agora.io/en/rtc/restfulapi/#/Project%20management/resetSignKey) | POST   | Resets the primary App Certificate.              |



### Project usage query

| Request URL                                                  | Method | Function                                    |
| :----------------------------------------------------------- | :----- | :------------------------------------------ |
| [BaseUrl/dev/v3/usage](https://docs.agora.io/en/rtc/restfulapi/#/Project%20usage%20query/getProjectUsagesV3) | GET    | Gets the usage data of a specified project. |



### Banning rule management

| Request URL                                                  | Method | Function                                                 |
| :----------------------------------------------------------- | :----- | :------------------------------------------------------- |
| [BaseUrl/dev/v1/kicking-rule](https://docs.agora.io/en/rtc/restfulapi/#/Banning%20rule%20management/createKickingRule) | POST   | Creates a banning rule.                                  |
| [BaseUrl/dev/v1/kicking-rule](https://docs.agora.io/en/rtc/restfulapi/#/Banning%20rule%20management/listKickingRule) | GET    | Gets a list of all banning rules.                      |
| [BaseUrl/dev/v1/kicking-rule](https://docs.agora.io/en/rtc/restfulapi/#/Banning%20rule%20management/updateKickingRule) | PUT    | Updates the expiration time of a specified banning rule. |
| [BaseUrl/dev/v1/kicking-rule](https://docs.agora.io/en/rtc/restfulapi/#/Banning%20rule%20management/deleteKickingRule) | DELETE | Deletes a specified banning rule.                        |



### Online channel statistics query

| Request URL                                                  | Method | Function                                     |
| :----------------------------------------------------------- | :----- | :------------------------------------------- |
| [BaseUrl/dev/v1/channel/user/property/{appid}/{uid}/{channelName}](https://docs.agora.io/en/rtc/restfulapi/?&_ga=2.180935975.1695148571.1593515861-1969480941.1589793536#/Online%20channel%20statistics%20query/userProperty) | GET    | Gets the user role of a specified channel.   |
| [BaseUrl/dev/v1/channel/user/{appid}/{channelName}](https://docs.agora.io/en/rtc/restfulapi/?&_ga=2.180935975.1695148571.1593515861-1969480941.1589793536#/Online%20channel%20statistics%20query/userList) | GET    | Gets the user list of a specified channel.   |
| [BaseUrl/dev/v1/channel/{appid}](https://docs.agora.io/en/rtc/restfulapi/?&_ga=2.180935975.1695148571.1593515861-1969480941.1589793536#/Online%20channel%20statistics%20query/channelList) | GET    | Gets the channel list of a specified project. |
