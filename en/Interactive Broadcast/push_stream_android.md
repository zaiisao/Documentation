
---
title: Push Streams to the CDN
description: 
platform: Android
updatedAt: Fri Nov 02 2018 17:08:14 GMT+0000 (UTC)
---
# Push Streams to the CDN
In a live broadcast, there are two options to push streams:

-   Pushing streams at the server
-   Pushing streams at the client


## Push streams at the server

1.  Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable this function if required. This function will be enabled in later releases.
2.  Call the `PublisherConfiguration` API to push streams from the Agora server.

## Adjusting the Picture-in-picture Layout

There are two methods to adjust the picture-in-picture layout:

-   [Flexible Adjustment](#layout_flexible)
-   [Default Layout](#layout_default)

<a name = "layout_flexible"></a>
### Flexible Adjustment

You can adjust the layout to display any user in any position, and in any size.

1.  Contact [sales-us@agora.io](mailto:sales-us@agora.io) to enable the function of pushing streams at the Agora server.

2.  Call the API method `setVideoCompositingLayout` to adjust the layout according to your needs:

> -   Ensure only one user in the same channel calls this API, otherwise, the other users must call `clearVideoCompositingLayout` to remove the settings.
> -   There will not be any video but only voice in CDN Live and the recordings of CDN Live if this API is not called correctly.


#### Example 1: Full Screen for One Host

To display the following layout:

<img alt="../_images/sei_1host.jpg" src="https://web-cdn.agora.io/docs-files/en/sei_1host.jpg" style="width: 420px;"/>

Set the parameters as follows:

```
Canvas:360*640, #FFFFFF
UserList
   User1 :
       userId: 123
       renderRegion: 0.0, 0.0, 1.0, 1.0
       alpha: 1.0
       zorder: 0
       renderMode: Cropped
```

#### Example 2: Two-host Tile Horizontally

To display the following layout:

<img alt="../_images/sei_2host.jpg" src="https://web-cdn.agora.io/docs-files/en/sei_2host.jpg" style="width: 420px;"/>

Set the parameters as follows:

```
Canvas: 640 * 360,  #FFFFFF

UserList
User1 :
    userId: 123
    renderRegion: 0.0, 0.0, 0.5, 1.0
    alpha: 1.0
    zorder: 0
    renderMode: Cropped
User2:
    userId: 456
    renderRegion: 0.5, 0.0, 0.5, 1.0
    alpha: 1.0
    zorder: 0
    renderMode: Cropped
```

#### Example 3: Three-host Tile Vertically

To display the following layout:

<img alt="../_images/sei_3host.jpg" src="https://web-cdn.agora.io/docs-files/en/sei_3host.jpg" style="width: 420px;"/>

Set the parameters as follows:

```
Canvas: 360 * 640,  #FFFFFF

UserList
   User1 :
       userId: 123
       renderRegion: 0.0, 0.0, 1.0, 0.6
       alpha: 1.0
       zorder: 0
       renderMode: Cropped
   User2:
       userId: 456
       renderRegion: 0.0, 0.6, 0.5, 0.4
       alpha: 1.0
       zorder: 0
       renderMode: Cropped
   User3:
       userId: 789
       renderRegion: 0.5, 0.6, 0.5, 0.4
       alpha: 1.0
       zorder: 0
       renderMode: Cropped
```

#### Example 4: 1 Full Screen + Randomly Floating Thumbnails

To display the following layout:

<img alt="../_images/sei_random.jpg" src="https://web-cdn.agora.io/docs-files/en/sei_random.jpg" style="width: 840px;"/>

Set the parameters as follows:

```
Canvas: 360 * 640,  #FFFFFF
UserList

   User1:
       userId: 123
       renderRegion: 0.0, 0.0, 1.0, 1.0
       alpha: 1.0
       zorder: 0
       renderMode: Cropped

   User2:
       userId: 456
       renderRegion: 0.1, 0.5, 0.3, 0.4
       alpha: 1.0
       zorder: 100
       renderMode: Cropped

   User3:
       userId: 789
       renderRegion: 0.5, 0.5, 0.3, 0.4
       alpha: 1.0
       zorder: 100
       renderMode: Cropped
```

<a name = "layout_default"></a>
### Default Layout

#### Layered Windows

**Floating**: One dominant upper window + multiple lower floating thumbnails \(the maximum layout is one dominant upper window + 6 lower floating thumbnails\).

For example,

<img alt="../_images/inpicture_1plusn.jpg" src="https://web-cdn.agora.io/docs-files/en/inpicture_1plusn.jpg" style="width: 840px;"/>


#### Tile Horizontally

**Horizontally Split**: Two horizontally displayed equally-sized windows.

For example,

<img alt="../_images/inpicture_horizontal.jpg" src="https://web-cdn.agora.io/docs-files/en/inpicture_horizontal.jpg" style="width: 420px;"/>


#### Tile Vertically

**Vertically Split**: One upper window \(60%\) + two lower equally-sized windows.

For example,

<img alt="../_images/inpicture_vertical.jpg" src="https://web-cdn.agora.io/docs-files/en/inpicture_vertical.jpg" style="width: 840px;"/>


