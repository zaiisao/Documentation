
---
title: Set Video Layout
description: 
platform: Linux
updatedAt: Wed Sep 30 2020 07:38:05 GMT+0800 (CST)
---
# Set Video Layout
## Overview
In the composite recording mode, you need to set the size and position of the region for each user in the layout.

As shown in the following image, the background of the video is **canvas**, and each user occupies a **region** on the canvas.

![](https://web-cdn.agora.io/docs-files/1577697787996)

> If the aspect ratio of a user's video does not match that of the user's region, the video may be cropped or letterboxed to fit the region. The aspect ratio of the user's region depends on the aspect ratio of the canvas and the layout type.

The demo of Agora On-premise Recording provides three predefined video layouts.

## Implementation

When you start recording in the command line, set `--isMixingEnabled 1`  to use the composite mode and set the `--layoutMode` parameter to choose a layout:

- `--layoutMode 0` : [Floating Layout](#float). (Default) The first user in the channel occupies the full canvas. The other users occupy the small regions on top of the canvas, starting from the bottom left corner. The small regions are arranged in the order of the users joining the channel. This layout supports one full-size region and up to four rows of small regions on top with four regions per row, comprising 17 users.
- `--layoutMode 1`: [Best Fit Layout](#bestfit). This is a grid layout. The number of columns and rows and the grid size vary depending on the number of users in the channel. This layout supports up to 17 users.
- `--layoutMode 2`: [Vertical Layout](#vertical). One large region is displayed on the left edge of the canvas, and several smaller regions are displayed along the right edge of the canvas. The space on the right supports up to 2 columns of small regions with 8 regions per column. This layout supports up to 17 users.

### <a name="float"></a>Floating layout

This is the default layout, where small regions are on top of a full-size region. The first user in the channel occupies the full canvas. The other users occupy the small regions on top of the canvas, starting from the bottom left corner. The small regions are arranged in the order of the users joining the channel. This layout supports up to 4 rows of regions on top with 4 regions per row.

- If a user sends only audio, the user's region is transparent.
- If the aspect ratio of a user's video does not match that of the user's region, the video is cropped to fit the region.
- The width and height of each small region are 23.5% of those of the canvas. The gaps between two horizontally or vertically adjacent regions are 1.2 % of the width and height of the canvas respectively. The gap between the bottom row and the border-bottom is 1.2 % of the height of the canvas, and the left/right margin is also 1.2 % of the width of the canvas.

See the following pictures for the layouts with different number of users in the channel.

![](https://web-cdn.agora.io/docs-files/1577697865676)

![](https://web-cdn.agora.io/docs-files/1577697930206)

![](https://web-cdn.agora.io/docs-files/1577697950264)

### <a name="bestfit"></a>Best fit layout

This is a grid layout. In this layout, the grid size varies depending on the number of users in the channel.

- The region which is not occupied by any user shows the background color.
- If a user sends only audio, the user's region shows the background color.
- If the aspect ratio of a user's video does not match that of the user's region, the video is cropped to fit the region.

See the following pictures for the layouts with different number of users in the channel.

![](https://web-cdn.agora.io/docs-files/1577697975441)

![](https://web-cdn.agora.io/docs-files/1577697997849)

![](https://web-cdn.agora.io/docs-files/1577698010532)

The best fit layout for 17 users in a channel is unique. As shown in the following figure, the regions are displayed across the middle of the canvas with bands on the left and right of the canvas. The width and height of each region are 20% of those of the canvas respectively, while the width of the two bands is 10% of that of the canvas. The 17th region is placed in the middle of the bottom row.

![](https://web-cdn.agora.io/docs-files/1577698032073)

### <a name="vertical"></a>Vertical layout

In this layout, one large region is displayed on the left edge of the screen, and several smaller regions are displayed along the right edge of the screen.

Therefore, when you recording, you must  set the `--maxResolutionUid` parameter to specify a uid, whose region is displayed on the left edge of the screen in a large size.

This layout supports up to two columns of small regions on the right edge with eight regions per column. 

- In the following figures, "Large" refers to the large region, displaying the video of the specified user. 
	- If you do not specify a user or the specified user does not join the channel, the "Large" region shows the background color.
	- If the aspect ratio of the specified user's video does not match that of the large region, the video is letterboxed to fit within the region, ensuring the completeness of the video.
- The small regions are tiled in the order of the users joining the channel. 
	- If user 1 leaves the channel, user 2 takes over user 1's region, and so on.
	- If the aspect ratio of a user's video does not match that of the user's region, the video is cropped to fit the region.
- The region which is not occupied by any user shows the background color.
- If a user sends only audio, the user's region shows the background color.

See the following pictures for the layouts with different number of users in the channel.

![](https://web-cdn.agora.io/docs-files/1577698419720)

- For the layout of 1 to 5 users, the regions of users 1 to 4 are tiled along the right edge of the canvas. The width and height of the small region are one-fifth and one-fourth of those of the canvas respectively.
- For the layout of 6 to 7 users, the regions of users 1 to 6 are tiled along the right edge of the canvas. The width and height of the small region are one-seventh and one-sixth of those of the canvas respectively.

![](https://web-cdn.agora.io/docs-files/1577698564432)

- For the layout of 6 to 7 users, the regions of users 1 to 8 are tiled along the right edge of the canvas. The width and height of the small region are one-ninth and one-eighth of those of the canvas respectively.
- For the layout of 6 to 7 users, the regions of users 1 to 16 are tiled along the right edge of the canvas. The width and height of the small region are one-tenth and one-eighth of those of the canvas respectively.

