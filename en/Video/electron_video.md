
---
title: Integrate the SDK
description: 
platform: Electron
updatedAt: Mon Jul 08 2019 02:46:42 GMT+0800 (CST)
---
# Integrate the SDK
This page contains information on how to prepare the development environment before enabling a call/video broadcast with the Agora SDK for Electron.

## Prerequisites

Development environment:

- Node.js 6.9.1 or later
- Electron 1.8.3 or later

> If you use Windows for development, ensure that you run npm install -D —arch = ia32 electron to install  a 32-bit Electron.

## Create an Agora Project and Get an App ID

1. Sign up for a developer account at [Agora Dashboard](https://dashboard.agora.io) and follow the on-screen instructions to create a project.
2. Click the Project Management icon in the left navigation panel.
3. Find the corresponding App ID under the created project.

## Add the Agora SDK to Your Project

You can directly install the SDK through npm.

1. Go to the project path, and run the following command line to install the latest version of the Electron SDK:

	`nmp install agora-electron-sdk`
	
2. Run the following command to import the SDK into your project:

	`import AgoraRtcEngine from 'agora-electron-sdk'`

## Switch the Prebuilt add-on version

By default, Agora uses Electron 1.8.3 to build the project. Switch the prebuilt add-on version in the .npmrc file according to your Electron version:

```
// Downloads a prebuilt add-on with Electron 1.8.3
AGORA_ELECTRON_DEPENDENT = 2.0.0
// Downloads a prebuilt add-on with Electron 3.0.6
AGORA_ELECTRON_DEPENDENT = 3.0.6
// Downloads a prebuilt add-on With Electron 4.0.0
AGORA_ELECTRON_DEPENDENT = 4.0.0
```

## Install the Dependency
Under the project path, run ` nmp install` to install the dependency and trigger the `npm run download` command. You can also install the dependency manually.
If you want to debug with Xcode or Visual Studio, run `npm run debug` to generate project files and SDK files for the debug environment. 

You have now integrated the Agora SDK for Electron into your project. Refer to  [Agora Electron Github Demo](https://github.com/AgoraIO-Community/Agora-Electron-Quickstart) to implement various real-time communication functions in your project.
