---
layout: default
title: "Extra: Migrating to your own brand"
nav_order: 8
has_toc: true
---
# Migrating to your own brand
{: .no_toc }

You already have a React Native project such as **Adventure Travel** but need to move forward with your own project/brand. Great! Continue with this guide to successfully migrate without old dependencies, update elements such as your app name, id, icons or splash screen.

![Image](/images/migrating_img.png)

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Updating files, folders and namespaces

Each React Native project contains a generated **android** and **ios** folders with the respective mobile application projects for each platform. Both projects include files, folders and namespaces referring to the original project, in this case: `adventureTravelApp`.
```diff
 📂 __test__
 📂 android
  ┣ 📂 app
  ┃ 📂 gradle
- ┃ ┣ 🧱 adventureTravelApp.iml
  ┃ ┣ 🧱 build.gradle
  ┃ ┣ 🧱 gradle.properties
  ┃ ┣ 🧱 gradlew
  ┃ ┣ 🧱 gradlew.bat
  ┃ ┣ 🧱 local.properties
  ┃ ┗ 🧱 settings.gradle
 📁 assets
 📁 ios
- ┣ 📁 adventureTravelApp
- ┣ 📁 adventureTravelApp-tvOS
- ┣ 📁 adventureTravelApp-tvOSTest
- ┣ 📁 adventureTravelApp.xcodeproj
- ┣ 📁 adventureTravelApp.xcworkspace
- ┣ 📁 adventureTravelAppTest
  ┣ 📁 Fonts
```

You could change the App **Display Name**, the **Bundle Identifier** or any other parameter, but your project will still contain the same files, folders and namespaces as the original project. If you can survive with that continue to the next session [Setting up a name for your App](/docs/migrating-to-your-brand/#setting-up-a-name-for-your-app). Otherwise continue to update the whole project with your brand.

The most convenient way to update any file, folder or namespace of your project is by creating a new React Native project from scratch and then copying the source code of the original project. To do this, open the terminal and run the following command specifying your brand name:

```
npx react-native init YourSuperBrand
```

Then copy the **_/src_** folder from the original project to the newly created project. Also copy/replace these files from the original project: 
- .env
- .env.production
- App.js
- babel.config.js
- package.json
- react-native.config.js

Your project should be structured like this:

![Image](/images/newProjectStructure.png)

---

## Setting up a name for your App

Let’s start by renaming the project package **name** and **version** with the name of your project from the `package.json` file:

_package.json_
``` js
{
  "name": "your-super-brand",
  "version": "1.0.1",
  "private": true,
  "scripts": {
      ...
```

### Android

For Android environment you can set the **applicationId** and **versionName** from the **_/android/app/build.gradle_** file.

```js
...
defaultConfig {
    applicationId "com.yoursuperbrand"
    minSdkVersion rootProject.ext.minSdkVersion
    targetSdkVersion rootProject.ext.targetSdkVersion
    versionCode 1
    versionName "1.0"
}
...
```

You can change the Display Name of your Android App from the **_/android/app/src/main/res/values/strings.xml_** file.

```xml
<resources>
    <string name="app_name">YourSuperBrand</string>
</resources>
```

### iOS

To change the App name for iOS platform open the project in Xcode from the project file: **_/ios/YourSuperBrand.xcworkspace_***. In Xcode select your project and specify a nice **Bundle Identifier**, a **Display Name** and the initial **Version**.

*Starting React Native 0.60, the iOS project uses cocoapods, so you have to open **_YourSuperBrand.xcworkspace_** and not **_YourSuperBrand.xcodeproj_**.
{: .text-red-000 }

![Image](/images/XcodeDisplayName.png)

---

## Backend configuration

> Below you can find a summary to configure the backend of your App in Firebase, but if you want to go deeper into this topic check the [Backend Configuration](/docs/backend-config){:target="_blank"} for more details.
{: .text-blue-300 }

In this project we are using Firebase as backend solution to handle the user authentication, databases, realtime updates (Realtime database) and push notifications. Therefore it’s required to configure the whole environment from [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"}. So let’s start by creating a new project from [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"}, when the creating process completes, you’ll be taken to the overview page for your Firebase project. Then you must add your iOS/Android application using the actions in each case.

![Image](/images/firebaseNewApp.png)

In the registration wizard you must fill out the **iOS bundle ID** (iOS) or **Android package name** (Android) of the application, which you should have already created when you [Setting up a name for your App](/docs/migrating-to-your-brand/#setting-up-a-name-for-your-app).

To finish download the generated configuration file and include it into the project from Xcode (for iOS) or add it to the project inside the **_/android/app/_** directory if you are creating an Android project as well. This file contains all the information to link your app with the created project, allowing you to access any of the Firebase services.

---
**Warning**: For iOS this file may be generated with **.xml** extension instead of **.plist**, however the project will not recognize this extension so it’s very important to change it to **.plist** after you download it.
{: .text-red-100 }
---

![Image](/images/downloadPList.png)

After that you just have to continue until the end of the wizard to finish with the app registration. Now that you have the firebase configuration file incorporated, you must continue to activate from Firebase Console all the required services:

- [Firebase Autentication](/docs/backend-config/#firebase-authentication){:target="_blank"}
- [Firebase Realtime Database](/docs/backend-config/#firebase-realtime-database){:target="_blank"}
- [Cloud Firestore](/docs/backend-config/#cloud-firestore){:target="_blank"}

## Application settings

To complete the Firebase configuration it’s necessary to add some settings in the App source code. If you are generating for iOS open the file **_/ios/YourSuperBrand/AppDelegate.m_** and add these lines:

_/ios/YourSuperBrand/AppDelegate.m_

```diff
  #import "AppDelegate.h"
  #import <React/RCTBridge.h>
  #import <React/RCTBundleURLProvider.h>
  #import <React/RCTRootView.h>
+ #import <Firebase.h>

  ···

  @implementation AppDelegate

  - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
  {
+  if ([FIRApp defaultApp] == nil) {
+    [FIRApp configure];
+ }
  #ifdef FB_SONARKIT_ENABLED
    InitializeFlipper(application);
  #endif

  ···
```

For Android it’s necesary to include a dependency, therefore you must add these lines in both **_build.gradle_** files:

_/android/build.gradle_

```diff
buildscript {
    ...
    dependencies {
        classpath('com.android.tools.build:gradle:4.1.1')
+       classpath('com.google.gms:google-services:4.3.4')
        ...
    }
}
```

_/android/app/build.gradle_

```diff
    apply plugin: "com.android.application"
+   apply plugin: "com.google.gms.google-services"
```

---
**Warning**: If you plan to include Google API to your project for searches and maps, you should make [these updates](/docs/app-config/#google-apis-setup){:target="_blank"} as well.
{: .text-red-100 }
---

Once you have included the firebase configuration file(s) in the project, you can proceed to install the packages of the project from the terminal:

```
npm install
```

---

## Native Vector Icons

> Source: [https://aboutreact.com/react-native-vector-icons![icon](/images/ext-link.png)](https://aboutreact.com/react-native-vector-icons){:target="_blank"}{: .text-grey-dk-000 }

**Vector Icons** are perfect for buttons, logos and tab bars, as part of the migration to your custom app it is necessary to include them as well.

### Android

To use Vector Icon in Android we need to import the Icons from **react-native-vector-icons**:

1. Create **_assets/fonts_** directory in **_android/app/src/main_**
1. Once you create the fonts directory copy all the font files from **_node_modules/react-native-vector-icons/Fonts_** into it **_/android/app/src/main/assets/fonts_**

### iOS

Follow the below steps to use vector icons in iOS:

1. Create a **_fonts_** directory in **_ios_** and copy all the font files from **_node_modules/react-native-vector-icons/Fonts_** into it
1. Now open the project **YourSuperBrand -> ios -> YourSuperBrand.xcworkspace** in Xcode
1. After opening the project in Xcode click on the project from the left sidebar to open the options and select **Add Files to “YourSuperBrand”** ![Image](/images/XcodeAddFiles.png){: .mt-lg-4 }
1. Select the fonts directory which you have created. Remember to select **Create Folder references** from below and click **Add** ![Image](/images/XcodeAddFonts.png){: .mt-lg-4 }
1. Now click the **project name** on the left top, and select the project name on **TARGETS**. Click the **Info** tab on the top menu to see Info.plist and add **Fonts provided by application** and font files under it ![Image](/images/XcodeIncludeFonts.png){: .mt-lg-4 }
Here we are adding two but you can add or remove according to your need. You can also add all font files which we have copied in above step.

---

## Creating an icon for your App

An important step to migrate to your personal brand is to update the App icon. To do this you must already have an image with your logo design, we recommend having the logo in a high quality image file of _1024x1024 pixels_. Now that you have your logo you must generate multiple size files for both platforms: **iOS** and **Android**.

### iOS

A simple way to generate the multiple image files is through a third tool, in the case of **iOS** we can use [Icon Set Creator![icon](/images/ext-link.png)](https://itunes.apple.com/us/app/icon-set-creator/id939343785?mt=12){:target="_blank"}.

![Image](/images/IconSetCreator.png)

After you generate the files, copy the generated folder **_AppIcon.appiconset_** and replace it in the directory of your project **_/iOS/YourSuperBrand/Images.xcassets/AppIcon.appiconset_**, you must also replace the **Contents.json** file.

### Android

> Source: [https://developer.android.com/studio/write/image-asset-studio#access![icon](/images/ext-link.png)](https://developer.android.com/studio/write/image-asset-studio#access){:target="_blank"}{: .text-grey-dk-000 }

For **Android** platform you can use the **Asset Studio tool**, included in **Android Studio**. So, open Android Studio and from the _Project window_, select the _Android view_. Then Right-click the **_res_** folder and select **New > Image Asset**.

![Image](/images/AndroidStudioNewImageAsset.png)

After you open Image Asset Studio, you can add adaptive and legacy icons by following these steps:

1. In the **Foreground Layer** tab, select **Image** as **Asset Type**, and then specify the path for your image logo file.
1. You can use the slider **Resize** to specify a scaling factor in percent to resize the icon.
1. Click **Next**.
1. Click **Finish**. Image Asset Studio adds the images to the **_mipmap_** folders for the different densities.

![Image](/images/AssetStudio.png)

---
## Including a Splash Screen

iOS and Android applications natively handle the **Splash Screen**, which is activated every time the app starts and will automatically hide after a few seconds. Although a component is included in the project as a loading screen (**_/src/screens/splashScreen/index.js_**), it’s necessary to create this screen natively first, also to avoid showing the default blank loading screen.

For this guide we will design a simple Splash Sreen that is composed of a centered logo and a red background. We need three sizes for the logo image, to better match all the devices screen sizes (300px, 600px @x2, 900px @x3).

### Add splash screen to iOS

First, open the project in Xcode from the project file: **_/ios/YourSuperBrand.xcworkspace_**, remember to open **_YourSuperBrand.xcworkspace_** and not **_YourSuperBrand.xcodeproj_**. In the left most navigator:
1. select _YourSuperBrand > YourSuperBrand > Imagex.xcassets_
1. click the **+** icon in the second left navigator and select **New Image Set** ![Image](/images/xcodeNewImageSet.png){: .mt-lg-4 }
1. name your image set _SplashIcon_
1. Add the three logo images (300px, 600px @x2, 900px @x3) selected earlier. You can drag and drop all of them at the same time, Xcode will sort them by pixel density automatically. ![Image](/images/xcodeAddSplashIncos.png){: .mt-lg-4 }

Once you have imported your logo you must edit the **LaunchScreen**, which is the storyboard created by default in a new React Native project. From Xcode, in the left most navigator:
1. open **_LaunchScreen.storyboard_**, select the generated labels and delete them.
![Image](/images/xcodeEditSplash.png){: .mt-lg-4 }
1. select **View**, then on the right navigator, click on the Attribute inspector icon _(2)_
1. select a background color from the **Background select list**
1. in the top right of the Xcode window, click on the Library icon _(4)_ to add a new **Image View**
![Image](/images/xcodeChangeBackground.png){: .mt-lg-4 }
1. search for **Image view**
![Image](/images/xcodeAddImage.png){: .mt-lg-4 }
1. and drag and drop this item into the **View** element
![Image](/images/xcodeAddImageView.png){: .mt-lg-4 }
1. You can rename the **Image View** and select the uploaded splash icon from the **Image View Panel** on the right navigator
![Image](/images/xcodeSelectImage.png){: .mt-lg-4 }

We need to make sure the icon is centered regardless of the device the app is running on. To do that:

1. Click on the **Align** button at the bottom right of the editor
1. Add new alignment constraints **Horizontally in container** and **Vertically in container**
![Image](/images/xcodeAddAlignment.png){: .mt-lg-4 }

To avoid a white screen flashing, just before the content is loaded, it’s necesary to include a few code lines referencing the `react-native-splash-screen` package.

In Xcode, open the file _YourSuperBrand > YourSuperBrand > AppDelegate.m_
1. Add `#import "RNSplashScreen.h"` with the other imports
1. Add `[RNSplashScreen show];` just above `return YES;` in the `didFinishLaunchingWithOptions` method.

_YourSuperBrand > YourSuperBrand > AppDelegate.m_
```diff
  #import "AppDelegate.h"
  #import <React/RCTBridge.h>
  #import <React/RCTBundleURLProvider.h>
  #import <React/RCTRootView.h>
  #import <Firebase.h>
+ #import "RNSplashScreen.h"

  ...

    @implementation AppDelegate

  - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
  {
    if ([FIRApp defaultApp] == nil) {
        [FIRApp configure];
    }
    #ifdef FB_SONARKIT_ENABLED
    InitializeFlipper(application);
    #endif

    RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
    RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
                                                    moduleName:@"YourSuperBrand"
                                                initialProperties:nil];

    rootView.backgroundColor = [[UIColor alloc] initWithRed:1.0f green:1.0f blue:1.0f alpha:1];

    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    UIViewController *rootViewController = [UIViewController new];
    rootViewController.view = rootView;
    self.window.rootViewController = rootViewController;
    [self.window makeKeyAndVisible];
+   [RNSplashScreen show];
    return YES;
  }

  ...
```

### Add splash screen to Android

While for iOS we mostly use the Xcode interface, for Android we will directly create or edit code files this time.

Android assets are located in **_android/app/src/main/res_**. There is a folder for each pixel density. Add your splash screen logo to the folders following this mapping:
* mipmap-mdpi = splash.png
* mipmap-hdpi = splash@2x.png
* mipmap-xhdpi = splash@3x.png
* mipmap-xxhdpi = splash@3x.png
* mipmap-xxxhdpi = splash@3x.png

and then rename all the files to **_splash_icon.png_**.

Next we will set up several files in the Android project to manage the Splash Screen.

* create a **colors.xml** file in **_/android/app/src/main/res/values_**, where you must include the selected background color, in this case: `#FF2D55`. You can include the following content:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="splashscreen_bg">#FF2D55</color>
    <color name="app_bg">#FF2D55</color>
</resources>
```
* create a **background_splash.xml** file in **_android/app/src/main/res/drawable_** with the list of layers composed of two items: the plain background and the included icon. Add the following code:
*/android/app/src/main/res/drawable/background_splash.xml*

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@color/splashscreen_bg"/>
    <item android:width="300dp" android:height="300dp" android:drawable="@mipmap/splash_icon" android:gravity="center" />
</layer-list>
```

* open **_android/app/src/main/res/values/styles.xml_** and add these lines:

```diff
<resources>
    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="android:textColor">#000000</item>
+       <item name="android:statusBarColor">@color/app_bg</item>
+       <item name="android:windowLightStatusBar">false</item>
+       <item name="android:windowBackground">@color/app_bg</item>
    </style>

    <!-- Adds the splash screen definition -->
+   <style name="SplashTheme" parent="Theme.AppCompat.Light.NoActionBar">
+       <item name="android:statusBarColor">@color/splashscreen_bg</item>
+       <item name="android:background">@drawable/background_splash</item>
+   </style>

</resources>
```

Now you have to configure the app to boot on the splash screen.

* Open **_/android/app/src/main/AndroidManifest.xml_** and modify the contents as follows:

```diff
 <manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.yoursuperbrand">
     <uses-permission android:name="android.permission.INTERNET" />
     <application android:name=".MainApplication" android:label="@string/app_name" android:icon="@mipmap/ic_launcher" android:roundIcon="@mipmap/ic_launcher_round" android:allowBackup="false" android:theme="@style/AppTheme">
+        <activity android:name=".SplashActivity" android:theme="@style/SplashTheme" android:label="@string/app_name">
+            <intent-filter>
+                <action android:name="android.intent.action.MAIN" />
+                <category android:name="android.intent.category.LAUNCHER" />
+            </intent-filter>
+        </activity>
+        <activity android:name=".MainActivity" android:label="@string/app_name" android:configChanges="keyboard|keyboardHidden|orientation|screenSize|uiMode" android:windowSoftInputMode="adjustResize" android:exported="true"/>
-        <activity android:name=".MainActivity" android:label="@string/app_name" android:configChanges="keyboard|keyboardHidden|orientation|screenSize|uiMode" android:launchMode="singleTask" android:windowSoftInputMode="adjustResize">
-            <intent-filter>
-                <action android:name="android.intent.action.MAIN" />
-                <category android:name="android.intent.category.LAUNCHER" />
-            </intent-filter>
-        </activity>
         <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
     </application>
 </manifest>
```

* Create a file **_/android/app/src/main/java/yoursuperbrand/SplashActivity.java_** with the contents:

```java
package com.yoursuperbrand;

import android.content.Intent;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

public class SplashActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Intent intent = new Intent(this, MainActivity.class);
        startActivity(intent);
        finish();
    }
}
```

* In **_android/app/src/main/java/yoursuperbrand/MainActivity.java_**, add these lines:

```diff
  package com.yoursuperbrand;

  import com.facebook.react.ReactActivity;
+ import org.devio.rn.splashscreen.SplashScreen;
+ import android.os.Bundle;

  public class MainActivity extends ReactActivity {

+   @Override
+   protected void onCreate(Bundle savedInstanceState) {
+       SplashScreen.show(this);
+       super.onCreate(savedInstanceState);
+   }

    ...

  }

```

* Create a file **_/android/app/src/main/res/layout/launch_screen.xml_** with the contents:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/background_splash"
    android:orientation="vertical">
</LinearLayout>
```

## Running the app 🚀

Great! You have finished all the configuration and it's time to build and run your App.

### iOS

From the terminal, go to your project folder and run these commands:

```
cd ios
pod update
pod install
cd ..
yarn ios
```

Voilà!

### Android

If you're working in an Android environment, to start the app you only have to run from the terminal:
```
yarn android
```
Remember that you must have an Android simulator installed correctly for the application to run successfully. In case you have any problem running the app for the first time open with **Android Studio** the generated android project, located in the **/android** folder of the project. From Android Studio verify that the sync runs correctly from: **File->Sync Project with Gradle Files** menu and also that you have a simulator configured. 