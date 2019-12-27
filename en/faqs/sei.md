
---
title: SEI-related Questions
description: 
platform: All Platforms
updatedAt: Fri Dec 27 2019 15:22:56 GMT+0800 (CST)
---
# SEI-related Questions
## The Agora SEI

By default, Agora adds the encoding information of the current video to the transcoded H264/H265 SEI frames during CDN streaming. The encoding information is a JSON string. The following is the sample code:

```json
{
  "canvas": {
    "w": 640,
    "h": 360,
    "bgnd": "#000000"
  },
  "regions": [{
	 "suid": 1,
    "uid": 1,
    "alpha": 1.0,
    "zorder": 1,
    "volume": 50,
    "x": 0,
    "y": 0,
    "w": 320,
    "h": 360
  }, {
    "uid": 2,
    "alpha": 1.0,
    "zorder": 1,
    "volume": 89,
    "x": 320,
    "y": 0,
    "w": 320,
    "h": 360
  }],
  "ver": "20180828",
  "ts": 1535385600000,
  "app_data": ""
}
```

The definition of the parameters:

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| canvas    | The canvas information. It contains the following properties:<br><li>w: Width (pixels) of the canvas. It corresponds to the  `width`  parameter in the `LiveTranscoding` class.<br><li>h: Height (pixels) of the canvas. It corresponds to the `height` parameter in the `LiveTranscoding` class.<br><li>bgnd: The background color (RGB) of the canvas, represented by a hexademical string. It corresponds to the `backgoundColor` parameter in the `LiveTranscoding` class. |
| regions   | The layout information of the broadcaster. It corresponds to the  `transcodingUsers`  class in the `LiveTranscoding` class. It contains the following properties:<br><li>suid: (optional)The string user account of the broadcaster in this region. This parameter applies to scenarios where string user accounts are used to identify the broadcaster.<br><li>uid: UID of the broadcaster in this region. It corresponds to the `uid` parameter in the `transcodingUsers` class.<br><li>alpha: The transparency of the video frame of the broadcaster. The value range is [0.0, 1.0]. It corresponds to the `alpha` parameter in the `transcodingUsers` class.<br><li>zorder: The layout position of the video frame of the broadcaster. The value range is [0, 100]. It corresponds to the `zorder`parameter in the `transcodingUsers` class.<br><li>volumn: The volume of the broadcaster. The value range is [0, 100].<br><li>x: The horizontal position of the video frame of the broadacaster from the top left corner of the CDN live streaming. It corresponds to the `x` parameter in the `transcodingUsers` class.<br><li>y: The vertical position of the video frame of the broadcaster from the top left corner of the CDN live streaming. It corresponds to the `y` parameter in the `transcodingUsers` class.<br><li>w: Width of the video frame of the broadcaster. It corresponds to the `width` parameter in the `transcodingUsers` class.<br><li>h: Height of the video frame of the broadcaster. It corresponds to the `height` parameter in the `transcodingUsers` class. |
| ver       | The version of the SEI protocol. The current version is 20190611. |
| ts        | Timestamp of the current encoding information.               |
| app_data  | Extra user-defined information. It corresponds to the `transcodingExtraInfo` parameter in the `LiveTranscoding` class. |

## The structure of SEI

The SEI information is as follows:

```
0000  0664bd7b 22617070 5f646174 61223a22  .d.{"app_data":"
0010  222c2263 616e7661 73223a7b 2262676e  ","canvas":{"bgn
0020  64223a22 23666666 66666622 2c226822  d":"#ffffff","h"
0030  3a363430 2c227722 3a333630 7d2c2272  :640,"w":360},"r
0040  6567696f 6e73223a 5b7b2261 6c706861  egions":[{"alpha
0050  223a3235 352c2268 223a3634 302c2275  ":255,"h":640,"u
0060  6964223a 33313031 32373137 39312c22  id":3101271791,"
0070  766f6c75 6d65223a 32382c22 77223a33  volume":28,"w":3
0080  36302c22 78223a30 2c227922 3a302c22  60,"x":0,"y":0,"
0090  7a6f7264 6572223a 317d5d2c 22747322  zorder":1}],"ts"
00a0  3a313533 37393630 32333537 38332c22  :1537960235783,"
00b0  76657222 3a223230 31383038 3238227d  ver":"20180828"}
```

In which:

- 06: The SEI frame.
- 64: The SEI frame type defined by the user. Here we define it as 100.
- bd: The SEI frame length. If the frame length exceeds 255, for example, 922, it is represented as ffffff9d.
- Other digits: Content of the SEI frame.

## FAQ

Q: If I use the SEI frame in CDN streaming, is it necessary to use the signaling system to send the layout information? Do I use either signaling or SEI?
A: Adding the H264/H265 SEI information to the video stream during CDN streaming is different from sending the uplink data. Only the `app_data` parameter of the SEI information relates to the uplink data. 

Q: Does the signaling system send the layout information? Why does the SEI frame contain the layout information?
A: Agora puts the layout information in the SEI frame because the audience members in the Live Broadcast profile also need the layout information of the current stream to set the window region of the broadcaster.
