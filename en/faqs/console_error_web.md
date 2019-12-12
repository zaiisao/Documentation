
---
title: Common Errors in Console Log
description: 
platform: Web
updatedAt: Mon Dec 02 2019 17:33:10 GMT+0800 (CST)
---
# Common Errors in Console Log
After integrating the Agora Web SDK into your web app, you can debug your code using the console log. This document lists the common errors in the console log.

## User is not in the session

### Reason

The connection has not been established. This is usually the result of an incorrect API call sequence, for example, calling  `Client.publish` after `Client.leave`.

### Solution

Check your API call sequence.

## Cannot read property "appendChild" of null

### Reason

The played DOM does not exist, or the element ID cannot be found.

### Solution

Ensure that the parent DOM exists when you call  `Stream.play`.

## Invalid elementID

### Reason

 The `HTMLElementID`  specified in `Stream.play`  is invalid.

### Solution

Ensure that the string in `HTMLElementID` contains only "_", "-", ".", or any digits and letters in the ASCII character set. The string must be non-empty and less than 256 bytes in length.

## UID_CONFLICT

### Reason

Multiple users joins the channel with the same `uid`.

### Solution

Ensure that all users in a channel have a unique `uid`.

## Failed to execute 'addStream' on 'RTCPeerConnection': parameter 1 is not of type 'MediaStream'

### Reason

You have called `Stream.publish`  before the `Stream.init`  method call succeeds.

### Solution

Call  `publish`  in the `onSuccess` callback of  `Stream.init`.

## Media access MEDIA_NOT_SUPPORT: video/audio streams not supported yet /enumerateDevices() not supported

### Reason

Possible reasons:

- The web browser is incompatible with the Agora Web SDK.
- HTTPS protocol is not used.
- Localhost is not used.

### Solution

- Use a supported [Web browser](https://docs.agora.io/en/faq/browser_support).
- Deploy your web app through HTTPS or localhost.

## Uncaught TypeError: Failed to execute 'createObjectURL' on 'URL': No function was found that matched the signature provided

### Reason

A mobile device emulator is running in the web browser.

### Solution

Agora Web SDK does not support mobile device emulators. Do not use an emulator to debug your app.

## Uncaught DOMException: Failed to execute 'addTransceiver' on 'RTCPeerConnection': This operation is only supported in 'unified-plan'.

### Reason

A mobile device emulator is running in the web browser.

### Solution

Agora Web SDK does not support emulated mobile devices. Do not use an emulator to debug your app.

## INVALID_VENDOR_KEY

### Reason

Possible reasons:

- The App ID is invalid. For example, the Agora project is disabled, or the App ID is not yet active within a newly created project.
- The token is invalid.

### Solution

Check your settings for `appId` in  `Client.init` and `tokenOrKey`  in  `Client.join`. Ensure that the App ID and token are correct and valid.

## Failed to set remote answer sdp: Called in wrong state: kStable

This error is caused by calling `switchDevice`  and can be ignored.

## Cannot read property 'stringuid' of undefined

### Reason

You have called `Stream.publish`  before the `Client.join`  method call succeeds.

### Solution

Check your code logic and ensure that you call `Stream.publish` after the `Client.join`  method call succeeds.
