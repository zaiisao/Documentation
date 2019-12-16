
---
title: Billing for the voice call
description: 语音通话计费说明
platform: All Platforms
updatedAt: Mon Dec 16 2019 20:24:56 GMT+0800 (CST)
---
# Billing for the voice call
## Cost

Voice communication is charged by the minutes used and the number of users.

After deducting the free monthly 10,000 minutes, Agora charges you:

<table>
  <tr>
    <th>Scenario</th>
    <th>Total Fee</th>
  </tr>
  <tr>
    <td>Recording disabled</td>
    <td>Communication Fee = Voice Unit Price x Total Communication Minutes</td>
  </tr>
  <tr>
    <td>Recording enabled </td>
    <td>Communication Fee + Recording Fee = Voice Unit Price x Total Communication Minutes + Voice Unit Price x Total Recording Minutes</td>
  </tr>
</table>

The Unit Price (price per minute) can be found at [Pricing](https://www.agora.io/en/price/).
Regardless of the number of users recording at the same time in a channel, the recording of the entire channel is only charged as one stream.

## Example: Voice Only

In this example, Agora charges Communication Fee + Recording Fee

### Communication Fee

<table>
  <tr>
    <th>User</th>
    <th>Minutes</th>
  </tr>
  <tr>
    <td>A</td>
    <td>30</td>
  </tr>
  <tr>
    <td>B</td>
    <td>40</td>
  </tr>
  <tr>
    <td>C</td>
    <td>20</td>
  </tr>
  <tr>
    <td>D</td>
    <td>15</td>
  </tr>
</table>

Communication Fee = Voice Unit Price x (30 + 40 + 20 + 15) min

### Recording Fee

<table>
  <tr>
    <th>User</th>
    <th>10 min</th>
    <th>20 min</th>
    <th>30 min</th>
    <th>40 min</th>
  </tr>
  <tr>
    <td>A</td>
    <td>Recording</td>
    <td>Recording</td>
    <td>Recording</td>
    <td></td>
  </tr>
  <tr>
    <td>B</td>
    <td></td>
    <td>Recording</td>
    <td>Recording</td>
    <td>Recording</td>
  </tr>
  <tr>
    <td>C</td>
    <td>Recording</td>
    <td>Recording</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>D</td>
    <td>Recording</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>

Recording Fee = Voice Unit Price x 40 min
