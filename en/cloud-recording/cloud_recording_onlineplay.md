
---
title: Play Recorded Files Online
description: 
platform: All Platforms
updatedAt: Mon Apr 29 2019 02:59:03 GMT+0800 (CST)
---
# Play Recorded Files Online
## Introduction

The Agora server automatically uploads the recorded files in TS format to the cloud storage that you set up, and generates an M3U8 file as a playlist pointing to all the recorded TS files. You can get the URL of the M3U8 file and play it online.

## Implementation

Before you start, ensure that all the recorded files are uploaded (by the `OnRecordingUploaded` callback).

Here we use [Amazon S3](https://aws.amazon.com/s3/) as an example to show you how to play the recorded files online.

1. Log in the Amazon S3 console, and go to the **Overview** page of the bucket you use for cloud recording. Set the metadata of the files as follows:
   1. Select the M3U8 file, click **Actions**, and select **Change metadata**.
![](https://web-cdn.agora.io/docs-files/1556174648050)
   2. Set the Value of the **Content-Type** Key  as **application/x-mpegURL** (you need to type in the value).
![](https://web-cdn.agora.io/docs-files/1556174883364)

 3.Select all the TS files and set **Content-Type** as **video/MP2T**.

2. Configure the bucket policy so that it can be accessed publicly. On the **Permissions** tab, click **Bucket Policy**, paste the following code (change  `<YourBucketName>` to the name of your bucket), and click **Save**:
   ```json
   {
    "Version": "2012-10-17",
    "Id": "Policy1553255976836",
    "Statement": [
        {
            "Sid": "Stmt1553255974279",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::<YourBucketName>"
        }
    ]
   }
   ```
	 ![](https://web-cdn.agora.io/docs-files/1556173842532)
3. Select the M3U8 file to view the URL.
![](https://web-cdn.agora.io/docs-files/1556174270602)

4. Navigate to the URL on a web browser to start playing the recorded files.

See [How to Serve HLS Video from an S3 Bucket](http://hlsbook.net/how-to-serve-hls-video-from-an-s3-bucket/) for more information.

## Considerations

- The M3U8 file can be played directly on Safari. For other web browsers, you might need to install an HLS playback extension.
- You can also play the M3U8 file on players supporting HLS, such as the VLC media player.
- If the `OnRecordingBackedUp` callback is triggered after the recording, it indicates that some of the recorded files are uploaded to Agora's backup cloud storage. You should wait until these files get uploaded automatically to your cloud storage before playing the M3U8 file.
