
---
title: Data Insight (Beta)
description: Introduction to Data Insight in AA
platform: All Platforms
updatedAt: Mon Jul 06 2020 05:05:01 GMT+0800 (CST)
---
# Data Insight (Beta)
The **Data Insight** function of Agora Analytics provides periodic call usage and call quality statistics. It is designed to help you understand the trend of the usage and quality of your calls, their distribution in various dimensions, and daily data breakdown.

The **Data Insight** function does the following:

- Shows daily statistics within a specified date range. 
- Updates the statistics every day.

<div class="alert info"><b>Data Insight</b> covers only the usage and quality statistics of the Agora RTC SDK.</div>

## Getting started

1. Contact support@agora.io to enable the **Data Insight** function for your project.
2. Login to [Agora Console](https://console.agora.io) and click **Agora Analytics** on the left navigation bar.
3. Select a project in the top-left corner.
   ![](https://web-cdn.agora.io/docs-files/1570869036486)
4. Click [**Usage Overview**](#usage) to view the usage statistics or [**Quality Overview**](#quality) to view the quality statistics.
5. Specify a date range in the upper-right corner (UTC) and click the ![](https://web-cdn.agora.io/docs-files/1570870803303) button to select the items to display (all selected by default).
   ![](https://web-cdn.agora.io/docs-files/1570776247336)

<div class="alert note"><b>Data Insight</b> has the following limits:
  <li>Provides data only after August 1, 2019.</li>
<li>The maximum specified date range is 30 days.</li>
<li>Supports viewing data for the past three months. Click <b>More</b> in the top-right corner of the charts to download the data.</li>
	<li><b>Data Insight</b> finishes calculating data for the previous day, closing at 08:00 UTC. You may find that there is no data available prior to this time. To view real-time data, use <b>Live Data</b>.</li>
</div>

## <a name="usage"></a>Usage overview

The following statistics are available on the **Usage Overview** page:

- **Overview**: View the variations of the following statistics and locate the dates with abnormalities:
  - Calls' duration
  - Number of channels
  - Maximum online users
  - Number of users
- **Distribution**: View the top five or ten usage statistics by different dimensions, such as country, operation system, SDK versions and more.

### Overview

The Overview panel shows the daily usage data in two sections: Duration and Channel & User.

In each section, the left area shows the total or average values, and the right area shows the daily values in line charts. The dates with abnormal values are marked with red dots. 

Click **More** in the top-right corner of the charts to view the data in a table or download the data.

![](https://web-cdn.agora.io/docs-files/1570776637728)

The following tables list the metrics in the subsequent sections of the Overview panel. 

**Duration**

|       Metric        | Description                                                  |
| :-----------------: | :----------------------------------------------------------- |
|   Total duration    | Total duration (minutes) of the voice and video calls of all users. |
| Video call duration | Total duration (minutes) of the video usage of all users.    |
| Audio call duration | Total duration (minutes) of the audio usage of all users.    |

**Channel & User**

|          Metric          | Description                                                  |
| :----------------------: | :----------------------------------------------------------- |
|      Total channels      | A count of all channels. A single channel begins when the first user joins and ends when the last user leaves. |
|    Total high-concurrent channels    | The number of channels with more than 50 users online at the same time. |
| Peak concurrent channels | Greatest number of simultaneous calls in one day.            |
|  Peak concurrent users   | Greatest number of online users in all the channels in one day. |
|       Total users        | Total number of unique UIDs in each channel.                 |
|    Total join counts     | Each time a user joins a channel is one count.               |

### Distribution

The Distribution panel shows the top five or ten usage statistics in the following dimensions.

|           Dimension           | Description                                                  |
| :---------------------------: | :----------------------------------------------------------- |
|          Geo - Globe          | Top 10 countries with the highest usage.                     |
|          Geo - China          | Top 10 provinces in China with the highest usage.            |
|         Network type          | Top 5 network types with the highest usage.                  |
|              OS               | Top 5 operating systems with the highest usage.              |
| Peak user number in a channel | Top 5 types of channels with the highest usage. The channles are categorized by the peak user number. |
|          SDK version          | Top 10 SDK versions with the highest usage.                  |

<div class="alert note">Items with no usage are not displayed.</div>

## <a name="quality"></a>Quality overview

The following statistics are available on the **Quality Overview** page:

- **Overview**: View the variations of the following statistics and locate the dates with abnormalities:
  - Join-channel success rate
  - First loading delay
  - Audio/video freeze rate
  - Network quality
- **Distribution**: View the top five or ten quality statistics  by different dimensions, such as country, operation system, SDK versions and more.

### Overview

The Overview panel shows the daily quality statistics in five sections: Join Channel, First Loading, User Experience, Network Delay, and Network Transmission.

In each section, the left area shows the daily average values (the sum of all daily statistics / number of days), and the right area shows the daily values with line charts (the dotted lines indicate the averages), and uses red dots to mark the dates with abnormal values.

Click **More** in the top-right corner of the charts to view the data in a table, or to download the data.

![](https://web-cdn.agora.io/docs-files/1570776833862)

**Join Channel**

|           Metric           | Description                                                  |
| :------------------------: | :----------------------------------------------------------- |
|     Join success rate      | Number of users who have joined / Number of users who have tried to join. |
| Join success in 5 sec rate | Number of users who have joined a channel successfully within 5 seconds / Number of users who have tried to join. |

**First Loading**

| <span style="white-space:nowrap;">&emsp;&emsp;Metric&emsp;&emsp;</span>         | Description                                                  |
| :--------------------: | :----------------------------------------------------------- |
| Audio first show delay | The elapsed time from when a user first joins a channel to when audio playback begins. The daily value is the median of all the users' elapsed time on that day. If a channel does not have an ongoing audio stream when a user joins it, this metric excludes that user. |
| Video first show delay | The elapsed time between a user first joining a channel to when video playback begins. The daily value is the median of all the users' elapsed time on that day. If a channel does not have an ongoing video stream when a user joins it, this metric excludes that user. |

**User Experience**

|      Metric       | Description                                                  |
| :---------------: | :----------------------------------------------------------- |
| Audio freeze rate | Total audio freeze time / Total audio playback duration. Audio freeze time counts all the audio freezes at least 200 ms in length. |
| Video freeze rate | Total video freeze time / Total video playback duration. Video freeze time counts all the video freezes at least 600 ms in length. |

**Network Delay**

|    Metric     | Description                                                  |
| :-----------: | :----------------------------------------------------------- |
| Network delay | The network delay (ms) from the sender to the receiver. The daily value is the median of  all the users' delays on that day. |

**Network Transmission**

|                Metric                | Description                                                  |
| :----------------------------------: | :----------------------------------------------------------- |
| High-quality audio transmission rate | The percentage of audio transmission with a ≤ 5% packet loss rate. |
| High-quality video transmission rate | The percentage of video transmission with a ≤ 5% packet loss rate. |

### Distribution

The Distribution panel shows the top five or ten quality statistics in the following dimensions.

|    Dimension     | Description                                                  |
| :--------------: | :----------------------------------------------------------- |
|   Geo - Globe    | Top 5 countries with the highest usage.                      |
|   Network type   | Top 5 network types with highest usage.                      |
|        OS        | Top 5 operating systems with highest usage.                  |
| Peak user number in a channel | Top 5 types of channels with the highest usage. The channles are categorized by the peak user number. |
|   SDK version    | Top 10 SDK versions with highest usage.                      |

<div class="alert note">Items with no usage are not displayed.</div>

## Key terms

This section describes the key terms used in **Data Insight**. See [Agora Key Terms](https://docs.agora.io/en/Agora%20Platform/terms) for more key terms.

### Channel

Every call or ineractive streaming happens in a channel. If we imagine an app being a building, a channel will be a room in the building.

**Data Insight** does not count a channel by name, but by lifecycle. It counts each time from when the first users joins the channel to when the last user leaves it.

### User

Each user in the channel has a unique [user ID](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-nameusernameausername). 

**Data Insight** counts a unique username in a unique channel as one user. If a real-life user joins one channel with different usernames, or joins different channels with one username, **Data Insight** counts the user in both situations as multiple users.

### Usage

Usage counts the user's actual duration in the channel.

Video usage is the duration of received video after a user joins a channel; audio usage is the duration of video not received after a user joins a channel. 

### Outlier

An outlier is a value that differs significantly from other values.

Data Insight marks outliers in red numbers on the Top panel under **Quality Overview**, to indicate the dates when some issues might have occurred.
