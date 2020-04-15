
---
title: How many users can Agora RTC SDK support at the same time?
description: 
platform: All Platforms
updatedAt: Wed Mar 11 2020 16:09:47 GMT+0800 (CST)
---
# How many users can Agora RTC SDK support at the same time?
<table>
  <tr>
    <th>Profile</th>
    <th>Platform</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Communication</td>
    <td>Android, iOS, macOS, Windows, the Web, Electron, Unity</td>
    <td><li>Voice: Up to one million users in a channel.<ul><ul><li>As of SDK v3.0.0, Agora recommends limiting the number of users sending streams concurrently to 17 at most.<li>With the SDK of a version earlier than v3.0.0, Agora recommends limiting the number of users sending streams concurrently to 7 at most.</li></ul></ul><li>Video: Up to one million users in a channel.<ul><ul><li>As of SDK v3.0.0, Agora recommends limiting the number of users sending streams concurrently to 17 at most.<li>With the SDK of a version earlier than v3.0.0, Agora recommends limiting the number of users sending streams concurrently to 7 at most.</li></ul></ul><li>Number of Channels: No limit.</li></td>
  </tr>
  <tr>
    <td>Live Broadcast</td>
    <td>Android, iOS, macOS, Windows, the Web, Electron, Unity</td>
    <td><li>Voice: Up to one million users in a channel. Agora recommends limiting the number of hosts sending streams concurrently to 17 at most.<li>Video: Up to one million users in a channel. Agora recommends limiting the number of hosts sending streams concurrently to 17 at most.<li>Number of Channels: No limit.</li></td>
  </tr>
</table>

<div class="alert note">If the number of users sending streams concurrently exceeds the recommend value, you may encounter the following issues:<ul><li>All users in the channel cannot hear the voice or see the video of some users casually. For example, with the SDK of v3.0.0, all users cannot hear the voice or see the video of a user casually when 18 hosts are sending audio and video streams concurrently in the channel.<li>When the downlink network bandwith is limited, you may encounter some undefined behaviors, such as audio or video freezes, and black screen.</li></ul></div>
