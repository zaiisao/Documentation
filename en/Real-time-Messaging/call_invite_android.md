
---
title: Call Invitation
description: 
platform: Android
updatedAt: Wed May 27 2020 03:47:15 GMT+0800 (CST)
---
# Call Invitation
## Introduction

You can use the the Agora RTM SDK to manage a call invitation by:

- Send a call invitation.
- Cancel a sent call invitation.
- Accept an incoming call invitation.
- Refuse an incoming call invitation.

Call invitation also includes the following functions:

- A caller can attach additional information when sending out a call invitation, such as channel name (`channelId`) and text messages (`content`).
- A callee can attach additional text information using the `response` parameter when refusing an incoming call invitation.
- A caller can initiate multiple call invitations at the same time, namely to send call invitations to multiple users.


<div class="alert note">The call invitation function in RTM SDK does not handle operations after a callee accepts the invitation, such as joining an RTC channel. You must implement the related media functions using a media SDK, such as the Agora RTC SDK.</div>

## Scenarios

- A call invitation requiring notification of an incoming call.
- A screen-sharing scenario requiring call invitations.
- A whiteboard-sharing scenario requiring a video call between two parties.
- A call invitation requiring synchronizing the states of both parties.


## Life cycle of a call invitation

The Agora RTM SDK introduces `LocalInvitation` and `RemoteInvitation` for a call invitation. You can think of them as the same invitation taking two different forms.

### The beginning of a life cycle

A `LocalInvitation` object is created when the caller explicitly calls `createLocalInvitation`. It allows the caller to specify the callee, set text messages, or check the state of the call invitation.
A `RemoteInvitation` object is returned by the `onRemoteInvitationReceived` callback when the callee receives the call invitation. It allows the callee to check the ID of the caller, set a response, or check the state of the call invitation.

### The end of a life cycle

The life cycle of a `LocalInvitation` object ends when the caller receives any of the following callbacks, and the SDK releases it when the corresponding callback ends.

- `onLocalInvitationCanceled`: The caller has cancelled the call invitation. 
- `onLocalInvitationAccepted`: The callee has accepted the call invitation. 
- `onLocalInvitationRefused`: The callee has refused the call invitation. 
- `onLocalInvittionFailure`: The call invitation has started but ends in failure. The error code `LocalInvitationError` that this callback returns covers the following reasons:
  - `LOCAL_INVITATION_ERR_PEER_OFFLINE`: The callee is offline. 
  - `LOCAL_INVITATION_ERR_PEER_NO_RESPONSE`: The callee does not ACK within 30 seconds after the caller sends out the call invitation. 
  - `LOCAL_INVITATION_ERR_INVITATION_EXPIRE`: The call invitation has expired (It has not been cancelled, accepted, or refused within 60 seconds after being received by the callee).

The life cycle of a `RemoteInvitation` object ends when the callee receives any of the following callbacks, and the SDK releases it when the corresponding callback ends. 

- `onRemoteInvitationCanceled`: The caller has cancelled the call invitation.
- `onRemoteInvitationAccepted`: The callee has accepted the call invitation. 
- `onRemoteInvitationRefused`: The callee has refused the call invitation. 
- `onRemoteInvitationFailure`: The call invitation ends in failure. The error code `RemoteInvitationError` that this callback returns covers the following reasons: 
  - `REMOTE_INVITATION_ERR_PEER_OFFLINE`: The caller is offline when the callee accepts the call invitation. 
  - `REMOTE_INVITATION_ERR_ACCEPT_FAILURE`: The caller does not ACK after the callee accepts the call invitation. 
  - `REMOTE_INVITATION_ERR_INVITATION_EXPIRE`: The call invitation has expired (It has not been cancelled, accepted, or refused within 60 seconds after being received by the callee).


<div class="alert note">The process of accepting a call invitation is virtually a 3-step handshake process.<ul>

<li>Step 1: The caller calls `sendLocalInvitation` to send out the call invitation, and then the callee receives the `onRemoteInvitationReceived` callback.</li>

<li>Step 2: The callee calls `acceptRemoteinvitation` and then the caller is notified that the callee has accepted the call invitation. </li>

<li>Step 3: The callee is notified that the caller has ACKed to this action. The call invitation succeeds. </li></ul></div>

## Behavior boundaries

Now that you understand the life cycle of a call invitation, it is important to get to understand where the line should be drawn in terms of normal behaviors and abnormalities. As a golden rule, all call invitation-specific method calls should be conducted within the life cycle of a call invitation. 

When the caller explicitly calls `sendLocalInvitation` or `cancelLocalInvitation` to send or cancel a call invitation, or when the callee explicitly calls `acceptRemoteInvitation` or `refuseRemoteInvitation` to accept or refuse a call invitation, the SDK validates the behavior and returns the `onFailure` callback with the corresponding error code `invitationApiCallError` if it detects a breach of the rule. 

| Callback                                                                        | Description                                                                                                                                                                                                                                                                |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `INVITATION_API_CALL_ERR_NOT_STARTED`         | The call invitation is not started. For example, the caller calls `cancelLocalInvitation` before `sendLocalInvitation`.                                                                                  |
| `INVITATION_API_CALL_ERR_ALREADY_END`         | You have called `sendLocalInvitation`, `cancelLocalInvitation`, `acceptRemoteInvitation`, or `refuseRemoteInvitation` when the lifecycle of a call invitation ends.  |
| `INVITATION_API_CALL_ERR_ALREADY_ACCEPT`  | The callee calls the `acceptRemoteInvitation` method repeatedly.                                                                                                                                                                     |
| `INVITATION_API_CALL_ERR_ALREADY_SENT`       | The caller calls the `sendLocalInvitation` method repeatedly.                                                                                                                                                                             |

## State diagram

In a call invitation session, the caller can use the `getState` method that the `LocalInvitation` object provides to get the state of the current call invitation; the callee can use the `getState` method that the SDK-returned `RemoteInvitation` object provides to get the state of the current call invitation. 

### LocalInvitationState

The following diagram shows the change of the call invitation states that are related to the caller. 

![](https://web-cdn.agora.io/docs-files/1582270646018)

### RemoteInvitationState

The following diagram shows the change of the call invitation states that are related to the callee. 

![](https://web-cdn.agora.io/docs-files/1582270656158)

## API call sequence diagram

### Cancel a sent call invitation

![](https://web-cdn.agora.io/docs-files/1565426396109)

### Accept or refuse a call invitation

![](https://web-cdn.agora.io/docs-files/1565427974586)

## Considerations

- The `content` parameter of the local invitation must be in UTF-8 format and not exceed 8 KB in length.
- The `response` parameter of the remote invitation must be in UTF-8 format and not exceed 8 KB in length. 
- The `channelId` parameter of the local invitation is used for interoperability with the legacy Signaling SDK only. Only when you set this parameter can an Agora Signaling SDK user receive the `onInviteReceived` callback. The `channelId` parameter must be in UTF-8 format and not exceed 64 KB in length. 

## API Reference

See [Call invitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_java/index.html#callinvitation).
