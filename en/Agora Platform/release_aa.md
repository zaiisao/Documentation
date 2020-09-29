
---
title: Agora Analytics Changelog
description: 
platform: All Platforms
updatedAt: Fri Sep 25 2020 07:22:24 GMT+0800 (CST)
---
# Agora Analytics Changelog
## Overview
Agora Analytics is a tool that tracks and analyzes the usage and quality of calls. You can use this tool to locate quality issues, find root causes, and fix the issues to improve the final user experience. See details in [Agora Analytics Overview](../../en/Agora%20Platform/aa_guide.md).

## 2020.09

**New features**

#### 1. Big Channel RESTful API (Beta)

This release adds the Big Channel RESTful API, and you can get the following data:

- The number of users who have a poor communication experience in a Big Channel. Poor communication experience in this case refers to video freezes and channel-join delay.
- The number of users who try to join a Big Channel.
- Users' call rating for a Big Channel.
- The number of online users in a Big Channel.

<div class="alert note">Before using the Big Channel RESTful API, you need to contact <a href="mailto:support@agora.io">support@agora.io</a > to enable the Big Channel function. See <a href="../../en/Agora%20Platform/aa_big_channel.md">Big Channel</a > and <a href="https://docs.agora.io/en/Agora%20Platform/aa_api?platform=All%20Platforms#big_channel">Big Channel RESTful API</a >.</div>

**Improvement**

#### 1. Call Search

- Moves the event timeline from the **Quality of Experience Overview** page to the **End-to-End Details** page. You can see the event timeline on the **Audio/Video Upstream Bitrate and Packet Loss** and **Audio/Video Downstream Bitrate and End-to-End Packet Loss** diagrams.

- Adds the following statistics on the **End-to-End Details** page:

   - Bitrate of the sent high-quality video
   - Bitrate of the sent low-quality video
   - Frame rate of the captured video
   - Resolution of the sent video

- Supports submitting a ticket on the **Quality of Experience Overview** page.

- Improves the user interface design.

#### 2. Others

Except for RESTful APIs, all Agora Analytics functions use the local time zone by default.

## 2020.05

This release adds the Real-time Alarm function (Beta), to help you identify abnormalities, analyze quality factors, and locate the source of each abnormal issue, all in real time. See details in [Real-time Alarm](../../en/Agora%20Platform/aa_realtime_alarm.md).

## 2020.01

This release adds the Big Channel function (Beta), which provides the Big Channel monitoring and issues diagnosis, and improves the operational efficiency of user activities in Big Channels. See details in [Big Channel](../../en/Agora%20Platform/aa_big_channel.md).

## 2019.10

This release adds the Realtime function (Beta), which helps to monitor the live status of your project. It also informs you of any abnormalities that occur along with their root cause. See details in [Realtime](../../en/Agora%20Platform/aa_live_data.md).

## 2019.09

This release adds the Data Insight function (Beta), which provides statistics on the usage and quality of your project over a specified period of time. You can view the quality statistics in various dimensions, such as country and SDK version. See details in [Data Insight](../../en/Agora%20Platform/aa_data_insight.md).

## 2019.08

This release adds key events in Call Search function, which improves the efficiency of call search.

## 2018.08

This release provides Call Search function, which helps to search calls and analyze quality issues. See details in [Call Search](../../en/Agora%20Platform/aa_call_search.md).
