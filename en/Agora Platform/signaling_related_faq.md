
---
title: Signaling-related Issues
description: 
platform: Signaling-related Issues
updatedAt: Fri Nov 02 2018 04:17:49 GMT+0800 (CST)
---
# Signaling-related Issues
### Signaling Login Failure

1. Check the network connection.

2. Check the App ID, App Certificate, and Signaling Key.

### LOGIN_E_TOKENWRONG(206) Error

The Signaling Key is invalid:

1. Check the App ID.

2. Check the App Certificate, and whether it is enabled on the Dashboard.

3. It takes about an hour for the App Certificate to take effect after it is enabled on the Dashboard.

4. Check the algorithm to generate the Signaling Key.

Refer to [Security Keys](../../en/Agora%20Platform/token.md).

### I Cannot Connect to a Verified PSTN (Public Switched Telephone Network) Phone Number

Confirm that you have included the country and area codes for the telephone number.

### I Cannot Connect on the Phone. Is it an App or Agora SDK Issue?

If you can connect in the demo application provided by Agora, then it is an app issue.


