
---
title: Chrome Extension for Screen Sharing
description: 
platform: Web
updatedAt: Mon Oct 12 2020 07:33:25 GMT+0800 (CST)
---
# Chrome Extension for Screen Sharing
To enable screen sharing on Chrome, you need to add our Chrome extension for screen sharing.

> As of v2.6.0, the Agora Web SDK supports screen sharing on Chrome 72 or later without an extension, see [Screen sharing without an extension](../../en/Video/screensharing_web.md).

<a name = "Add-the-Chrome-Screen-sharing-Extension"></a>

## Add the screen-sharing extension

Follow these steps to add the screen-sharing extension.

### 1. Get the screen-sharing extension

[Download](http://download.agora.io/sdk/release/chrome-extension.zip) the screen-sharing extension and unzip the package. The package includes the following files.

<img alt="../_images/chrome_extension_screenshare.png" src="https://web-cdn.agora.io/docs-files/en/chrome_extension_screenshare.png" style="width: 630px; "/>

### 2. Add your domain name

Open the `manifest.json` file in the extension folder, and add the domain name of your web app in the line beginning with `"matches"`.

For example, if you are running your web app on localhost, add `"*://localhost/*"` to the values of `"matches"`:

```json
"matches": ["*://localhost/*","*://*.agora.io/*","*://webdemo.agora.io/*","*://webdemo.agorabeckon.com/*","*://videocall.agora.io/*"]
```

### 3. Load the extension and get the extension ID

1. Open your Chrome browser and click **More Tools** > **Extensions** in the settings menu of Chrome to open the Extensions page.
 ![](https://web-cdn.agora.io/docs-files/1566267251936)
3. Enable **Developer mode** in the upper-right corner on the Extensions page.
 ![](https://web-cdn.agora.io/docs-files/1566267300318)
 Three buttons appear under the Extensions menu.
5. Click **Load unpacked** and select the **chrome-extension** folder.

Now, you can see the extension ID in Chrome. You need the `extensionId` to enable screen sharing,  see [Screen sharing with the Chrome extension](../../en/Quickstart%20Guide/screensharing_web.md).

<img alt="../_images/chrome_extension_id.png" src="https://web-cdn.agora.io/docs-files/en/chrome_extension_id.png" style="width: 420px;"/>

<a name = "Uploading-the-Chrome-Screen-sharing-Extension"></a>

## Upload the screen-sharing extension

1. Modify the JSON file.

   Delete the code that begins with **“key”** and the **,** above **“key”** in the `manifest.json` file in the extension package:

   ```json
   {
   	 "manifest_version": 2,
   	 "minimum_chrome_version": "34",
   	 "name": "Agora Web Screensharing",
   	 "permissions": [ "desktopCapture" ],
   	 "short_name": "Screen sharing for Agora",
   	 "update_url": "https://clients2.google.com/service/update2/crx",
   	 "version": "0.0.0.1",
   
   	 "background": {
   			"persistent": true,
   			"scripts": [ "script.js" ]
   	 },
   	 "description": "Extension to allow screen sharing in Agora Web Application.",
   	 "externally_connectable": {
   			"matches": ["*://gs.agora.io/*", "*://webdemo.agora.io/*", "*://webdemo.agorabeckon.com/*"]
   	 },
   	 "icons": {
   			"128": "128w.png",
   			"16": "16w.png",
   			"48": "48w.png"
   	 },
   	 "browser_action":{
   		 "default_icon": "16w.png"
   	 },
   	 "key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyRcxkUO0XuAsLqzRMIL+XlNTAgbc4/CtRrC2o7qDHGv6uAjmeS7HiK0hzK4PowsUTi0Y38LLzxju0Zr0IFoz9R5fKQt45rAdViujkuCURI4gFKUn6nOJ1/LjaTXYh02v1qWR17Aih8dc1VkWlBQKcapaH6y0N35i7IHZVWsT+ySXsdS6GDFPZVb1wYhDZRZYbkRYpBVEf11HOX+PkQGO5zhbdjBsp7BPF4L//vRwUxcxmeqgkRgzPAAy99UMsrgh/kbJSzE8XacUET9eYKzT21/ZSkiXEddWWCm2jeRWTrfie6D+c1K4zGFnS47in9timvpkMl5OM7J58wqjK20FiwIDAQAB"
   }
   ```

2. Package the modified Chrome screen-sharing extension and register it in the Google App Store so that your user can download and use it. 

   On how to publish your extension on the Chrome Web Store, see [Publish in the Chrome Web Store](https://developer.chrome.com/webstore/publish).

To enable your customers to directly download and install the extension from your website, see [Using Inline Installation](https://developer.chrome.com/webstore/inline_installation). In your app, you also need to register the ID of the extension using the Agora Web SDK.
