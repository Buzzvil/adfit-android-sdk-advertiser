# BuzzAd Android SDK For Advertiser integration guideline
- By integrating this SDK, you will be able to run CPE (install and open) and CPA (any sort of in-app action required by advertiser) campaign through BuzzAd network.
- Android Version Requirement: Above 2.3 (API Level 9)
- For this integration, please get `app_id` issued from your account manager.

## 1. Add SDK to your android project
- [Download the SDK](https://github.com/Buzzvil/buzzad-android-sdk-advertiser/archive/master.zip) and unzip it.
- Add buzzad-android-sdk-advertiser.jar file in the project. (e.g, inside the libs folder)
- Add a permission in the AndroidManifext.xml file as described below. In case the permission is already inserted, please skip this step.

```Xml
<manifest>
    ...
    <!-- Permission for BuzzAd-->
    <uses-permission android:name="android.permission.INTERNET" />
</manifest>
```

- Set up Google play service library for ads - [How to add google play service library] (https://developers.google.com/android/guides/setup)
    
    > If you are using Android Studio, you can just add `compile 'com.google.android.gms:play-services-ads:7.5.0'` command in the **build.gradle > dependencies**

## 2. Add tracking functions
- `BATracker.init(Context context, String appId)` : Call this function whenever users open your app
- `BATracker.actionCompleted(Context context)` : Call this function when users open your app for CPE campaign and when users complete a mission required for CPA campaign. This function is used to track if users finish the action, depending on adveritser’s needs.

Also, please keep in mind that:

- **You must call BATracker.init before you call BATracker.actionCompleted**
- **Please check out “api call success” in logcat (tag: buzzad-analytics) when calling BATracker.actionCompleted.**

### For CPE
Add the two functions to the point of activity creation(onCreate) which will be called right after opening the app.

```Java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    
    ...
    
    // app_id : unique key number issued by BuzzAd account manager
    BATracker.init(this, "app_id");
    BATracker.actionCompleted(this);
}
```
### For CPA (e.g, registration, completion of tutorial, etc.)
##### Step 1. When app is opened
Call `BATracker.init` at the point of activity creation which will be called right after opening the app.

```Java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    
    ...
    
    // app_id : unique key number issued by BuzzAd account manager
    BATracker.init(this, "app_id");
}
```

##### Step2. When action is completed
Call `BATracker.actionCompleted` when a mission required is completed.

```Java
void onAction() {
    
    ...
    
    // Call when a mission is completed
    BATracker.actionCompleted(this);
}
```
## 3. Confirm if the integration is done right
Send the apk file to BuzzAd account manager and confirm if the integration is done right.
