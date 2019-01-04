
---
title: Audio-related Issues
description: 
platform: Audio-related Issues
updatedAt: Fri Jan 04 2019 11:09:37 GMT+0000 (UTC)
---
# Audio-related Issues
### My H5 game integrates the Agora SDK v2.2.0 for iOS. When the host uses WKWebview with Layabox and joins the channel, why is the game volume very low?
The H5 game loaded by WKWebView plays audio by AVAudioSession instead of the SDK.

To fix this issue, call the `AudioProfile` method and set `scenario` to the `GameStreaming` mode:
`self.agoraEngine setAudioProfile:(AgoraRtc_AudioProfile_Default) scenario:(AgoraRtc_AudioScenario_GameStreaming)`.

Setting `scenario` to the `GameStreaming` mode may lead to echo issues. If you have any concerns, please contact Agora technical support.

### Why is no audio captured when an Android 9 device locks its screen or the app on the device runs in the background?

According to the Android Developer website, this is a system restriction:

> **Limited access to sensors in background**
> Android 9 limits the ability for background apps to access user input and sensor data. If your app is running in the background on a device running Android 9, the system applies the following restrictions to your app:
> * Your app cannot access the microphone or camera.
> * Sensors that use the continuous reporting mode, such as accelerometers and gyroscopes, don't receive events.
> * Sensors that use the on-change or one-shot reporting modes don't receive events.
> If your app needs to detect sensor events on devices running Android 9, use a foreground service.

See [Behavior changes: all apps](https://developer.android.com/about/versions/pie/android-9.0-changes-all).

Developers can use **Foreground Service** to work around this restriction.
If you need to use an Android 9 device to capture audio after the device locks its screen, you can start a foreground service before the screen locks, and stop the service before exiting the screen lock. See the following code snippets:

- Starts a Foreground Service

	```java
	Intent intent = new Intent(this, MyForegroundService.class);
	intent.setAction(MyForegroundService.ACTION_START_FOREGROUND_SERVICE);
	this.startService(intent);
	```
	
- Stops a Foreground Service

	```java
	Intent intent = new Intent(this, MyForegroundService.class);
	intent.setAction(MyForegroundService.ACTION_STOP_FOREGROUND_SERVICE);
	stopService(intent);
	```

- Sample code for the `MyForegroundService.class`

	```java
	package io.agora.tutorials1v1acall;

	import android.app.Notification;
	import android.app.PendingIntent;
	import android.app.Service;
	import android.content.Intent;
	import android.os.IBinder;
	import android.support.v4.app.NotificationCompat;
	import android.util.Log;
	import android.widget.Toast;

	public class MyForegroundService extends Service {
			private static final String TAG_FOREGROUND_SERVICE = "FOREGROUND_SERVICE";

			public static final String ACTION_START_FOREGROUND_SERVICE = "ACTION_START_FOREGROUND_SERVICE";

			public static final String ACTION_STOP_FOREGROUND_SERVICE = "ACTION_STOP_FOREGROUND_SERVICE";

			public MyForegroundService() {
			}

			@Override
			public IBinder onBind(Intent intent) {
					throw new UnsupportedOperationException("Not yet implemented");
			}

			@Override
			public void onCreate() {
					super.onCreate();
					Log.d(TAG_FOREGROUND_SERVICE, "My foreground service onCreate().");
			}

			@Override
			public int onStartCommand(Intent intent, int flags, int startId) {

					Log.d(TAG_FOREGROUND_SERVICE, "onStartCommand " + intent);

					if (intent != null) {
							String action = intent.getAction();

							switch (action) {
									case ACTION_START_FOREGROUND_SERVICE:
											startForegroundService();
											Toast.makeText(getApplicationContext(), "Foreground service is started.", Toast.LENGTH_LONG).show();
											break;
									case ACTION_STOP_FOREGROUND_SERVICE:
											stopForegroundService();
											Toast.makeText(getApplicationContext(), "Foreground service is stopped.", Toast.LENGTH_LONG).show();
											break;
							}
					}
					return super.onStartCommand(intent, flags, startId);
			}

			/* Builds and starts a foreground service. */
			private void startForegroundService() {
					Log.d(TAG_FOREGROUND_SERVICE, "Start foreground service.");

					// Creates the notification default intent.
					Intent intent = new Intent();
					PendingIntent pendingIntent = PendingIntent.getActivity(this, 0, intent, 0);

					// Creates the notification builder.
					NotificationCompat.Builder builder = new NotificationCompat.Builder(this, "sfdsfds");

					// Enables the notification to show in big text.
					NotificationCompat.BigTextStyle bigTextStyle = new NotificationCompat.BigTextStyle();
					bigTextStyle.setBigContentTitle("Music player implemented by foreground service.");
					bigTextStyle.bigText("Android foreground service is a android service which can run in foreground always, it can be controlled by user via notification.");
					// Sets the big text style.
					builder.setStyle(bigTextStyle);

					builder.setWhen(System.currentTimeMillis());

					// Sets the notification to maximum priority.
					builder.setPriority(Notification.PRIORITY_MAX);
					// Creates a heads-up notification.
					builder.setFullScreenIntent(pendingIntent, true);

					// Builds the notification.
					Notification notification = builder.build();

					// Starts the foreground service.
					startForeground(1, notification);
			}

			private void stopForegroundService() {
					Log.d(TAG_FOREGROUND_SERVICE, "Stop foreground service.");

					// Stops the foreground service and removes the notification.
					stopForeground(true);

					// Stops the foreground service.
					stopSelf();
			}
	}
	```
