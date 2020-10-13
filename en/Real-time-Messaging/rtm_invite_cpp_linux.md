
---
title: Call Invitation
description: 
platform: Linux CPP
updatedAt: Sat Oct 10 2020 10:58:32 GMT+0800 (CST)
---
# Call Invitation
## Overview

The Agora RTM SDK supports the call invitation function, including the following behaviors in common call scenarios:

- Caller: Sends or cancels a call invitation.
- Callee: Accepts or refuses a call invitation.

![](https://web-cdn.agora.io/docs-files/1602314541995)

The call invitation function provided by the Agora RTM SDK only implements the basic control logic of the call invitation: sending, canceling, accepting, and refusing the call invitation. The Agora RTM SDK does not handle operations after a callee accepts the invitation, nor does it manage the entire lifecycle. You must implement that yourself according to your requirements.

The call invitation can be applied to the following scenarios:

- A call invitation requiring notification of an incoming call.
- A screen-sharing scenario requiring call invitations.
- A whiteboard-sharing scenario requiring a video call between two parties.
- A call invitation requiring synchronizing the states of both parties.

## Implementation

In a complete call invitation process, the call invitation states of the caller and the callee are defined by `LocalInvitation` and `RemoteInvitation,` respectively.

![](https://web-cdn.agora.io/docs-files/1602314550083)


### Send a call invitation

The steps to send a call invitation are as follows:

1. The caller creates the `LocalInvitation` by calling `createLocalInvitation`; at the same time, the lifecycle of the `LocalInvitation` begins.
2. The caller sends a call invitation by calling `sendLocalInvitation`. The lifecycle of the `RemoteInvitation` begins as the callee receives the `onRemoteInvitationReceived` callback, and then the caller receives the `onLocalInvitationReceivedByPeer` callback.



### Cancel a call invitation

The caller cancels the call invitation by calling `cancelLocalInvitation`. The lifecycle of the `RemoteInvitation` ends as the callee receives the `onRemoteInvitationCanceled` callback. And then the lifecycle of the `LocalInvitation` ends as the caller receives the `onLocalInvitationCanceled` callback.

![img](https://web-cdn.agora.io/docs-files/1598604387439)


### Accept a call invitation

The callee gets RemoteInvitation from `onRemoteInvitationReceived` and accepts the call invitation by calling `acceptRemoteInvitation`. The lifecycle of the `RemoteInvitation` ends as the caller receives the `onRemoteInvitationAccepted` callback. The lifecycle of the `LocalInvitation` ends as the caller receives the `onLocalInvitationAccepted` callback.


![img](https://web-cdn.agora.io/docs-files/1598604394009)


###  Refuse a call invitation

The callee gets RemoteInvitation from `onRemoteInvitationReceived` and refuses the call invitation by calling `refuseRemoteInvitation`. The lifecycle of the `RemoteInvitation` ends as the caller receives the `onRemoteInvitationRefused` callback. The lifecycle of the `LocalInvitation` ends as the caller receives the `onLocalInvitationRefused` callback.

![img](https://web-cdn.agora.io/docs-files/1598604400671)


##  API Reference

See [Call invitation](https://docs.agora.io/en/Real-time-Messaging/API%20Reference/RTM_cpp/index.html#callinvitation).
