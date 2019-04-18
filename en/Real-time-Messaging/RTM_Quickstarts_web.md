
---
title: RTM Quickstart Guide
description: v0.9.0web quickstart 
platform: Web
updatedAt: Thu Apr 18 2019 02:30:25 GMT+0800 (CST)
---
# RTM Quickstart Guide
## Integrate the SDK

### `npm link` local installation

This process requires a module bundler such as web pack or Browserify.

1. Save the downloaded SDK to a specific directory (`$SDK_PATH`). Navigate to this directory from command line and run:

```shell
npm install
```

> This command installs dependencies for the `AgoraRTM.d.ts` file. Skip this step if you do not use TyperScript for definition purposes.

2. In your root directory of your project, which holds the `package.json` file, run the following command：

```shell
npm link $SDK_PATH
```

#### Considerations

- Installing`npm link` locally does not write dependancy to `package.json`, so you cannot use the SDK unless you have run the abovementioned command.
- This is but a makeshift solution before npm publication. Do not use this for production.

### Direct `<script>` include

Simply download and include with a `<script>` tag, and `AgoraRTM` is registered as a global variable.

Add to the HTML file the following code and replace `/path/to/agora-rtm-sdk.min.js` with the URL of `agora-rtm-sdk.min.js`:

```html
<script src="/path/to/agora-rtm-sdk.min.js"></script>
```

> Ensure that the address specified by `src` is accessible as a URL in a web browser.

#### Use the TypeScript definition file (`AgoraRTM.d.ts`) to enable IntelliSense

After getting the global `AgoraRTM` object using Direct `<script>` Include, you can use it in the rest of your JavaScript code, but you cannot enable IntelliSense.
To enable IntelliSense, add the following annotation at the beginning of the JavaScript and replace `path/to/AgoraRTM.d.ts` with the actual directory holding the `AgoraRTM.d.ts` file:

```JavaScript
/// <reference path="path/to/AgoraRTM.d.ts" />
```

## Initialize the Client

### Create an Instance

Before logging in the Agora RTM system, call the `AgoraRTM.createInstance` method to create an `RtmClient` method.

```JavaScript
import AgoraRTM from 'agora-rtm-sdk';
const client = AgoraRTM.createInstance('demoAppId'); // Pass your App ID here.
```
> Only RTM instances with the same App ID can talk to each other.

### Pass your Token and initialize your client (optional)

Pass a Token for specifying the user role and privilege. You need to generate the Token on your app server. In situations where security does not take priority.

```JavaScript
client.init({ token: 'demoToken' }) // token: Token generated from your app server.
```

## Send and receive peer-to-peer messages

You can send and receive peer-to-peer messages after logging in Agora's RTM server.

### Send a peer-to-peer message

Call the `sendMessageToPeer` method of `client` to send a peer-to-peer message. You need to

- Pass the user ID of the intended receiver.
- Pass an 'RtmMessage` object.

And this method returns a Promise:

- The *resolve* state of this Promise indicates whether the intended receiver receives the message.
- The *reject* state of this Promise indicates timeout, rejected-by-the-server, or connection failure.

```JavaScript
client.sendMessageToPeer(
'demoPeerId', // The user ID of the remote user
{ text: 'test peer message' }, // An RtmMessage object.
).then(sendResult => {
if (sendResult.hasPeerReceived) {
/* Your code for handling events that the remote user receives the message. */
} else {
/* Your code for handling events that the message is received by the server but the remote user cannot be reached. */
}
}).catch(error => {
/* Your code for handling events of message send failure. */
});
```

### Receive a peer-to-peer message

Set a listener to the `MessageFromPeer` event on `client` to receive a peer-to-peer message.

```JavaScript
client.on('MessageFromPeer', { text }, peerId) => { // text: text of the received message; peerId: User ID of the sender.
/* Your code for handling events of receiving a peer-to-peer message. */
});
```

## Send and receive channel messages

### Create and join a channel

The following code snippet shows how to create a channel.

```JavaScript
const channel = client.createChannel('demoChannelName'); // Pass your channel ID here.
```

The following code snippet shows how to join a channel.  `channel` refers to the newly-created channel instance; each channel ID corresponds to a separate channel instance. Same for the rest.

```JavaScript
channel.join().then(() => {
/* Your code for handling the event of join-channel success. */
}).catch(error => {
/* Your code for handling the event of join-channel failure. */
});
```

### Send a channel message

You can send channel messages after joining a channel.

```JavaScript
channel.sendMessage({ text: 'test channel message' }).then(() => {
/* Your code for handling events such as channel message-send success. */
}).catch(error => {
/* Your code for handling events such as channel message-send failure. */
});
```

### Receive a channel message

```JavaScript
channel.on('ChannelMessage', ({ text }, senderId) => { // text: text of the received channel message; senderId: user ID of the sender.
/* Your code for handling events such as receiving a channel message. */
});
```

### Set listener to events such as a member joins or leaves the channel.

```JavaScript
channel.on('MemberJoined', memberId => { // memberId: user ID of the user joining the channel
/* Your code for handling events such as a member joins the channel. */
})
```

```JavaScript
channel.on('MemberLeft', memberId => { // memberId: user ID of the user joining the channel.
/* Your code for handling events such as a member leaves the channel. */
})
```


