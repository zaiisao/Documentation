
---
title: Start Video Call
description: 
platform: Flutter
updatedAt: Thu Oct 01 2020 02:06:47 GMT+0800 (CST)
---
# Start Video Call
This page includes the following sections:

- [Run the sample project](#run-the-sample-project): Agora provides an [open-source sample project](https://github.com/AgoraIO-Community/Agora-Flutter-Quickstart) on GitHub that implements Agora Flutter Quickstart. Learn how to run the sample project.
- [Implement video call](#implement-video-call): Learn how to create a simple project and implement video call using the Agora Flutter SDK.

## Run the sample project

### Prerequisites

**Development environment**

If your target platform is iOS, your development environment must meet the following requirements:

- Flutter 2.0.0 or later
- macOS
- Xcode (Latest version recommended)

If your target platform is Android, your development environment must meet the following requirements:

- Flutter 2.0.0 or later
- macOS or Windows 
- Android Studio (Latest version recommended)

**Deployment environment**

- If your target platform is iOS, you need a real iOS device.
- If your target platform is Android, you need an Android simulator or a real Android device.

<div class="alert note">You need to run <code>flutter doctor</code> to check whether the development environment and the deployment environment are correct.</div>

#### Other

A valid Agora [developer account](https://docs.agora.io/en/Agora%20Platform/sign_in_and_sign_up).

### Steps to run

**Step 1. Create an Agora project**

Take the following steps to create an Agora project in Agora Console.

1. Log in to Agora [Console](https://console.agora.io/) and click ![img](https://web-cdn.agora.io/docs-files/1551254998344) in the left navigation menu to enter the [Project Management](https://console.agora.io/projects) page.

2. Click **Create**.

   ![img](https://web-cdn.agora.io/docs-files/1574924327108)

3. Enter your project name, set the App ID authentication mechanism as **APP ID** in the dialog box, and click **Submit**.

4. When the project is created successfully, you can see the newly created project in the project list. Agora assigns an App ID to each project as the only identifier.

**Step 2. Get App ID**

Click ![img](https://web-cdn.agora.io/docs-files/1592488399929) to view and copy the App ID.

![img](https://web-cdn.agora.io/docs-files/1574924570426)

**Step 3. Generate a temporary token **

To facilitate authentication at the test stage, Agora Console provides temporary tokens. Take the following steps to generate a temporary token:

1.  Project Management page, find the project for which you want to generate a temporary token, and click ![](https://web-cdn.agora.io/docs-files/1602841076825).![](https://web-cdn.agora.io/docs-files/1602841103054)
2. On the Token page, enter the name of the channel that you want to join, and click Generate Temp Token to get a temporary token.![](https://web-cdn.agora.io/docs-files/1602841110522)

When in a production environment, Agora recommends generating a token at your server by calling `buildTokenWithUid`. See [Generate a token](../../en/Audio%20Broadcast/token_server.md).



**Step 4. Run the project**

1. Download the [Agora-Flutter-Quickstart](https://github.com/AgoraIO-Community/Agora-Flutter-Quickstart) repository. Open `settings.dart` (`lib/src/utils/settings.dart`). Add App ID and Token.

	```
const APP_ID = Your_App_ID;
const Token = Your_Token;
	```

2. Run the following command to install dependencies.

	 ```bash
   flutter packages get
	 ```

3. Run the project.

   ```bash
   flutter run
   ```

## Create a Flutter Project

This article uses Visual Studio Code to create a Flutter project. Before you begin, you need to install the Flutter plugin in Visual Studio Code. See [Set up an editor](https://flutter.dev/docs/get-started/editor?tab=vscode).

1. Launch Visual Studio Code. Select the **Flutter: New Project** command in **View > Command**. Enter the project name and press `<Enter>`. 
2. Select the location of the project in the pop-up window. Visual Studio Code automatically generates a simple project.

### Add dependencies

Add the following dependencies in `pubspec.yaml`:

1. Add the `agora_rtc_engine` dependency to integrate Agora Flutter SDK. See https://pub.dev/packages/agora_rtc_engine for the latest version of `agora_rtc_engine`.
2. Add the `permission_handler` dependency to add the permission handling function.

```
environment:
  sdk: ">=2.7.0 <3.0.0"
  
dependencies:
  flutter:
    sdk: flutter
  
  
  # The following adds the Cupertino Icons font to your application.
  # Use with the CupertinoIcons class for iOS style icons.
  cupertino_icons: ^0.1.3
  # Please use the latest version of agora_rtc_engine
  agora_rtc_engine: ^3.1.2
  permission_handler: ^3.0.0
```

### Implement video call

Open `main.dart`, remove all code after

```
void main() => runApp(MyApp());
```

Use these steps to add the following code:


**Step 1: Import packages**

```
import 'dart:async';
 
 
import 'package:agora_rtc_engine/rtc_engine.dart';
import 'package:flutter/material.dart';
import 'package:permission_handler/permission_handler.dart';
```


**Step 2: Define App ID and Token**

See [Set up Authentication](../../en/Agora%20Platform/token.md).


```
/// Define App ID and Token
const APP_ID = '<Your App ID>';
const Token = '<Your Token>';
```

**Step 3: Define MyApp Class**


```
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}
```

**Step 4: Define app states**

```
// App states
class _MyAppState extends State<MyApp> {
  bool _joined = false;
  int _remoteUid = null;
  bool _switch = false;
 
  @override
  void initState() {
    super.initState();
    initPlatformState();
  }
 
  // Initialize the app
  Future<void> initPlatformState() async {
    await PermissionHandler().requestPermissions(
      [PermissionGroup.camera, PermissionGroup.microphone],
    );
     
    // Create RTC client instance
    var engine = await RtcEngine.create(APP_ID);
    // Define event handling
    engine.setEventHandler(RtcEngineEventHandler(
        joinChannelSuccess: (String channel, int uid, int elapsed) {
          print('joinChannelSuccess ${channel} ${uid}');
          setState(() {
            _joined = true;
          });
        }, userJoined: (int uid, int elapsed) {
      print('userJoined ${uid}');
      setState(() {
        _remoteUid = uid;
      });
    }, userOffline: (int uid, UserOfflineReason reason) {
      print('userOffline ${uid}');
      setState(() {
        _remoteUid = null;
      });
    }));
    // Enable video
    await engine.enableVideo();
    // Join channel 123
    await engine.joinChannel(Token, '123', null, 0);
  }
 
 
  // Create UI with local view and remote view
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Flutter example app'),
        ),
        body: Stack(
          children: [
            Center(
              child: _switch ? _renderRemoteVideo() : _renderLocalPreview(),
            ),
            Align(
              alignment: Alignment.topLeft,
              child: Container(
                width: 100,
                height: 100,
                color: Colors.blue,
                child: GestureDetector(
                  onTap: () {
                    setState(() {
                      _switch = !_switch;
                    });
                  },
                  child: Center(
                    child:
                    _switch ? _renderLocalPreview() : _renderRemoteVideo(),
                  ),
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
   
  // Generate local preview
  Widget _renderLocalPreview() {
    if (_joined) {
      return RtcLocalView.SurfaceView();
    } else {
      return Text(
        'Please join channel first',
        textAlign: TextAlign.center,
      );
    }
  }
 
  // Generate remote preview
  Widget _renderRemoteVideo() {
    if (_remoteUid != null) {
      return RtcRemoteView.SurfaceView(uid: _remoteUid);
    } else {
      return Text(
        'Please wait remote user join',
        textAlign: TextAlign.center,
      );
    }
  }
}
```


### Run the project

1. Run the following command in the root folder to install dependencies.

```
flutter packages get
```

2. Run the project.

```
flutter run
```

### Common issues

If the deployment environment is Android, users in mainland China may get stuck in `Running Gradle task 'assembleDebug'...` or see the following error:

```
Running Gradle task 'assembleDebug'...
Exception in thread "main" java.net.ConnectException: Connection timed out: connect
```

Take the following steps to resolve this issue:

1. In the `build.gradle` file of the Android project, use mirrors in China for `google` and `jcenter.`

```
buildscript {
    ext.kotlin_version = '1.3.50'
    repositories {
        // google()
        // jcenter()
        maven { url 'https://maven.aliyun.com/repository/google' }
        maven { url 'https://maven.aliyun.com/repository/public' }
    }
 
...
 
allprojects {
    repositories {
        // google()
        // jcenter()
        maven { url 'https://maven.aliyun.com/repository/google' }
        maven { url 'https://maven.aliyun.com/repository/public' }
    }
}
```

2. In the `gradle-wrapper.properties` file, set `distributionUrl` to a local file. For example, for gradle 5.6.4, you can copy `gradle-5.6.4-all.zip` to `gradle/wrapper` and set `distributionUrl` to:

```
distributionUrl=gradle-5.6.4-all.zip
```


