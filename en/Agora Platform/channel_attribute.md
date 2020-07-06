
---
title: channel attribute
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 12:47:21 GMT+0800 (CST)
---
# channel attribute
Channel attributes are tags added to RTM channels, including the property name, property value, the ID of the last RTM user who updated the attribute, and the time of the last update. 

The Agora RTM SDK supports adding, deleting, updating, and getting the channel attributes. Each property name must have only one property value. Each channel attribute can contain multiple pairs of property names and property values. For example, the following property name and property value in a channel attribute describes the title and content of a group notification:

```json
{"group_notice_title":"This is a group notice","group_notice_content":"Hello"}
```

<div class="alert info">See also:
<a href="https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/index.html#channelattributes">Channel attribute operations</a>
</div>

<a href="../../en/Agora%20Platform/terms.md"><button>Back to glossary</button></a>
