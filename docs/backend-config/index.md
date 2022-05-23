---
layout: default
title: Backend configuration
nav_order: 3
has_toc: true
---
# Backend Configuration
{: .no_toc }

As part of the architecture we have included the use of **Firebase** as a complement for backend operations. Firebase is a Google Backend-as-a-Service (BaaS) platform that helps you build, improve and grow your applications. It offers a wide range of features, but for the purposes of this project we will focus on the following services:

* Firebase Authentication (For our user authentication service)
* Firebase Realtime Database (As realtime database)
* Cloud Firestore (As database)
* Cloud Messaging (For Push Notifications)

**Warning**: Before running the app for the first time, it’s required to first configure a new application from **Firebase Console platform**.
{: .text-red-100 }

Below we describe all the steps to configure the Firebase based backend.

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Create a Firebase project

Before you can add Firebase to your app, you need to create a Firebase project to connect to your iOS or Android app. Follow these steps or consult the [Firebase documentation![icon](/images/ext-link.png)](https://firebase.google.com/docs?authuser=0){:target="_blank"} to create a new project.

1. Sign into [Firebase![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"} using a Google account.
2. In the **Firebase console**, click **Add project**, then select or enter your *Project name*
3. Click **Continue**.
4. Click **Create project**

Firebase automatically provides resources for your project. When the process completes, you’ll be taken to the overview page of your Firebase project.

![Image](/images/createFirebaseProject.gif)

### Register your app with Firebase (iOS)

If you are working with the iOS platform it’s necesary to register a new iOS App in Firebase. From the **Project Overview**, use the **Add app** option and then select the iOS icon to open the registration wizard.

![Image](/images/addApp.png)

In the registration wizard you must fill out your **iOS bundle ID**, example: _com.myadventure.app_. This will be the identifier of your app and later you will have to put it in Xcode as well.

![Image](/images/createIOSProject.gif)

In the next step you must download the configuration file **GoogleService-Info.plist** and continue to the end to complete the app registration. This file contains all the information to link the app with the created project, allowing you to access any of the Firebase services.

**Warning**: This file may be generated with **.xml** extension, however the project will not recognize this extension so it’s very important to change it to **.plist** after you download it.
{: .text-red-100 }

The downloaded **GoogleService-Info.plist** file must be added to the project from Xcode. Inside Xcode you must also change the **Bundle Identifier**, the app **version** and the **build** number.

![Image](/images/xcodeConfig.gif)

---
> If you want to migrate in depth to a new App, you can review: [Migrating to your own brand](/docs/migrating-to-your-brand){:target="_blank"}.
{: .text-grey-dk-000 }
---

### Register your app with Firebase (Android)

If you are working with the Android platform it’s necesary to register a new Android App in Firebase. From the **Project Overview**, use the **Add app** option and then select the Android icon to open the registration wizard.

![Image](/images/addApp.png)

In the registration wizard you must fill out an **Android package name** for your app, example: _com.adventuretravelapp_. This will be the identifier of your app and later you will also have to include it in the android project.

![Image](/images/addAndroidApp.png)

Download the generated configuration file **google-service.json** and continue until the end of the wizard to complete the app registration in Firebase. This file contains all the information to link the app with the created project, allowing you to access any of the Firebase services.

![Image](/images/downloadJson.png)

The downloaded **google-service.json** file must be added to the project inside the **/android/app/** directory. Then you can set the **Android package name** (**applicationId**) and **versionName** in the **_/android/app/build.gradle_** file.

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

After that you must setup the Firebase project to allow the authentication, create the databases and activate the push notifications (optional).

## Firebase Authentication

> Firebase Authentication provides backend services, easy-to-use SDKs, and ready-made UI libraries to authenticate users to your app. It supports authentication using passwords, phone numbers, popular federated identity providers like Google, Apple, Facebook and Twitter, and more. - [Firebase Authentication Docs![icon](/images/ext-link.png)](https://firebase.google.com/docs/auth){:target="_blank"}
{: .text-grey-dk-000 }

As authentication method we use **Email/Password** authentication provider, which allows users to register with their email address and password. Firebase Authentication also provide email address verification, password recovery, and email address change primitives. To activate the Email/Password provider go to the **Authentication** menu of your project in [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"}. Then select the **Sign-in method** tab and activate the Email/Password provider to start using this authentication method in the app.

![Image](/images/configFirebaseAuth.gif)

### Apple Sign-in

For iOS devices you can also activate authentication with Apple Sign-in. With Apple Sign-in users can access the App using their Apple Account and hiding their email address. To activate Apple Sign-in you must go to the **Authentication** menu of your project in [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"}. Then select the **Sign-in method** tab and activate the **Apple** provider to start using this authentication method in the app.

![Image](/images/apple-signin-firebase.png)

To complete the activation of **Apple Sign-in** you must go to the [Apple Developer Program![icon](/images/ext-link.png)](https://developer.apple.com){:target="_blank"} site using your apple developer account and go to the **Account** menu and then to [**Certificates, IDs & Profiles**![icon](/images/ext-link.png)](https://developer.apple.com/account/resources/certificates/list){:target="_blank"}. Select **Identifiers** from the menu on the left to add/edit your app identifier.

![Image](/images/identifiers.png)

If your app identifier is not created yet you can click on the **plus** icon to add your app identifier. In this case you must register an **App ID**, specify the **Bundle ID** and check the option for **Sign In with Apple**. If your App ID already exists select it and go to **Sign In with Apple** and check it.

![Image](/images/apple-sign-in.png)

In this way you have already configured the Sign in with Apple. If you do not want to include this option you can remove the **AppleButton** from the **LoginScreen** screen located in _/src/screens/loginScreen/index.js_.

---
## Firebase Realtime Database

In some cases you may need to handle data that requires realtime updating to be displayed on different devices at the same time. We have incorporated **Firebase Realtime Database** (FRD) to store and handle in real time: bookmarks, user information and bookings. When you connect your app to FRD, you’re actually connecting through a WebSocket. WebSockets are much faster than the HTTP protocol, you don’t have to make individual WebSocket calls, because a single socket connection is enough. This allows all your data to be automatically synchronised through that single WebSocket. When you save a change in data, all connected clients receive the updated data almost instantly from the backend.

![Image](/images/firebaseRealtimeDatabase.png)

For this project it’s necessary to create a FRD before running the app for the first time. To do this just go to the **Realtime Database** menu from [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"} and click on **Create Database** button. Then select the option _Start in test mode_, just to start using it, later you must include some [validation rules](/docs/firebase-database-rules/#realtime-database-rules){:target="_blank"} for a better security.

![Image](/images/configRealtimeDB.gif)

With the introduction of this remote database system we hope you will be able to use it plainly in your project for all the data you need to store and synchronize in realtime.

---
## Cloud Firestore

There are also data that may not require updating in real time and just run the typical CRUD (Create, Read, Update, Delete) operations of a database without any realtime subscription. In this case we also use **Cloud Firestore** to store application data such as experiences, categories and popular sites, therefore it’s required to create a Firestore database first. To create a Firestore database in your proyect go to the **Cloud Firestore** menu from [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"} and click on **Create Database** button. Then select the option _Start in test mode_, just to start using it, later you must include some [validation rules](/docs/firebase-database-rules/#cloud-firestore-security-rules){:target="_blank"} for a better security.

![Image](/images/configFirestore.gif)

---
At this point we should have all the backend configuration completed, then we can move on to make other [adjustments to the app](/docs/app-config) code and start running it.

## Cloud Messaging (Push Notifications)

We have added **push notifications** to this project, allowing you to send alerts to users using Firebase’s **Cloud Messaging** service. Firebase Cloud Messaging (FCM) is a cross-platform messaging solution that lets you reliably send messages at no cost. Notifications can be sent directly from Firebase Console or through third party applications integrated with FCM.

The notification service is included by default in Firebase iOS and Android projects, however you need to make a few adjustments before you can start using it. For now we recommend to follow the guide to [configure the app and run it for the first time](/docs/app-config) and finally proceed with the settings for [Push Notifications](/docs/push-notifications).