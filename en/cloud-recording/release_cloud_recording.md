
---
title: Release Notes for Agora Cloud Recording
description: 
platform: Linux
updatedAt: Tue Oct 08 2019 01:51:04 GMT+0800 (CST)
---
# Release Notes for Agora Cloud Recording
## Overview

Agora Cloud Recording is an add-on service to record and save voice calls, video calls, and interactive broadcasts on your cloud storage. Compared with Agora On-premise Recording, Agora Cloud Recording is more efficient and convenient as it does not require deploying Linux servers.

### Compatibility

Agora Cloud Recording is compatible with the following SDKs:

- Agora Native SDK v1.7.0 or later.
- Agora Web SDK v1.12 or later.

## v1.2.0

v1.2.0 is released on July 22, 2019. 

### New features

#### Customized video layout

The RESTful API adds a customized layout for the recording video. See [Set Video Layout](../../en/cloud-recording/cloud_layout_guide.md) for details.

You can set the `mixedVideoLayout` parameter as `3` and set the regions for each user in the `layoutConfig` parameter when starting a recording.

You can update the layout anytime during the recording by the `updateLayout` method.

#### Customized background color

The RESTful API adds the `backgroundColor` parameter to support customized background colors for the video layout. 

#### Timestamps

To get the accurate starting time of a recording, the RESTful API provides the Unix timestamp of when the first slicing starts in the response of the `query` method.  The RESTful API callback service adds the `recorder_slice_start` event to report the time when the first slicing starts and the time when the last recording fails.

### Improvement

Optimizes the verification of whether `resourceId` corresponds with `uid` and `cname` when calling the RESTful API.

### Fixed issue

Fixed minor issues in the default video layout (floating layout).

## v1.1.1

v1.1.1 is released on July 2, 2019 with the following improvements:

- Changes the default background color in the composite layout to black.
- Reduces video freeze under poor network conditions.

## v1.1.0

v1.1 is released on June 13, 2019.

This version supports RESTful APIs. With the RESTful APIs, you can use Agora Cloud Recording through HTTP requests without integrating the SDK.

See the following documents for details:

- [RESTful API Quickstart](../../en/cloud-recording/cloud_recording_rest.md): Start cloud recording with RESTful APIs.
- [RESTful API Reference](../../en/cloud-recording/cloud_recording_api_rest.md): Details of the RESTful API methods.
- [RESTful API Callback Service](../../en/cloud-recording/cloud_recording_callback_rest.md): Enable the callback service to receive notifications of Agora Cloud Recording. 

## v1.0.0

v1.0.0 is released on April 30, 2019.

This is the first release of Agora Cloud Recording with the following functions:

- High-quality voice and video recordings.
- Mixed-stream voice and video recordings of all users in a channel.
- Three composite video layouts: float (default), best fit, and vertical, see [Set Video Layout](../../en/cloud-recording/cloud_layout_guide.md) for details.
- Third-party cloud storage. Agora Cloud Recording supports Amazon S3, Alibaba Cloud, and Qiniu Cloud.
- Provides C++ and Java SDK packages.
