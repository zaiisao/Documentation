
---
title: FAQ
description: 
platform: Linux
updatedAt: Mon Apr 29 2019 04:17:15 GMT+0800 (CST)
---
# FAQ
##  Java SDK integration error

### Common errors in integrating Java SDK

#### java.land.UnsatisfiedLinkError: no agora-cloud-recording-java in java.library.path

##### **Reason**
The system cannot find the `libagora-cloud-recording-java.so` file.

##### **Solution**

1. Print  `java.library.path`, and check if the Linux environment variable  `LD_LIBRARY_PATH` includes the .so file.
  ```bash
System.out.println(System.getProperty("java.library.path"))
  ```
2. We recommend adding `java.library.path` to `LD_LIBRARY_PATH`. You can add `LD_LIBRARY_PATH` to **/etc/profile** or put the `libagora-cloud-recording-java.so` file in the system directory **/usr/lib**.

#### java.land.NoClassDefFoundError: Could not initialize class io.agora.cloud_recording.CloudRecorder

##### **Reason**
`agora-cloud-recording-sdk.jar` is not in the `classpath` variable. Integrating the Java SDK needs to load the `agora-cloud-recording-sdk.jar` and `libagora-cloud-recording-java.so` files. The .jar file should be included in the `classpath` variable and the .so file should be included in the `LD_LIBRARY_PATH` variable.

##### **Solution**

1. Print `classpath`, and check if it includes `agora-cloud-recording-sdk.jar`.
```bash
System.out.println("<Path to the class>：" + System.getProperty("java.class.path"));
```
2.  If not, add the path to the .jar file in `classpath`.


## Uploading to cloud storage fails

Check if your cloud storage settings are correct:

- bucket: name of your storage bucket, created by yourself in your cloud storage account.
- accessKey: information in your cloud storage account.
- secretKey: information in your cloud storage account.

## How to get the M3U8 file URL

The URL of the M3U8 file consists of the domain of your cloud storage and the file name. You can copy the URL in your cloud storage.


The following callbacks provide the file name of the M3U8 file:

- C++
  - [`OnRecordingUploadingProgress`](https://docs.agora.io/en/cloud-recording/cloud-recording/cloud_recording_api#OnRecordingUploadingProgress)
  - [`OnRecordingUploaded`](https://docs.agora.io/en/cloud-recording/cloud-recording/cloud_recording_api#OnRecordingUploaded)
  - [`OnRecordingBackedUp`](https://docs.agora.io/en/cloud-recording/cloud-recording/cloud_recording_api/#OnRecordingBackedUp)

- Java
  - [`onRecordingUploadingProgress`](https://docs.agora.io/en/cloud-recording/cloud-recording/cloud_recording_api_java#onRecordingUploadingProgress)
  - [`onRecordingUploaded`](https://docs.agora.io/en/cloud-recording/cloud-recording/cloud_recording_api_java#onRecordingUploaded)
  - [`onRecordingBackedUp`](https://docs.agora.io/en/cloud-recording/cloud-recording/cloud_recording_api_java/#onRecordingBackedUp)

## 101 error

The error `"ErrorUint32":101` in the log file is usually caused by token error, including the following situations:

- Wrong token 
- Expired token
- The Native/Web SDK uses token while the cloud recording does not use token
- The cloud recording uses token while the Native/Web SDK does not use token
