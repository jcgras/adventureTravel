---
layout: default
title: Backend configuration
nav_order: 3
has_toc: true
---
# Backend Configuration
{: .no_toc }

As part of the architecture we have added the use of **Firebase** as a complement for backend operations. Firebase is a Google Backend-as-a-Service (BaaS) platform that helps you build, improve and grow your applications. Firebase offers a wide range of features, but for the purposes of this project we will focus on the following services:

* Firebase Authentication (For our user authentication service)
* Firebase Realtime Database (As realtime database)
* Cloud Firestore (As database)
* Cloud Messaging (For Push Notifications)

To start working on the project, it is necessary to first configure a new application from **Firebase Console platform**. Below we describe all the steps to configure the Firebase based backend.

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

Firebase automatically provisions resources for your Firebase project. When the process completes, you'll be taken to the overview page for your Firebase project in the Firebase console.

### Register your app with Firebase (iOS)

If you are working with the iOS platform it's necesary to register a new iOS App in Firebase. From the **Project Overview**, use the **Add app** option and then select the iOS icon to open the registration wizard.

![Image](/images/addApp.png)

In the registration wizard you must fill out the **iOS bundle ID** of the application (located in the General tab in Xcode), for this project would be: _dev.svalbard.app_. Later, for your particular project, you must use the **Bundle Identifier** you created your project with, but to test the app we will use this value first.

![Image](/images/addiOSAppConsole.png)

To finish download the generated configuration file **google-service.plist** and include it into the project from Xcode. This file contains all the information to link our app with the created project, allowing you to access any of the Firebase services.

![Image](/images/downloadPList.png)

Skip the next steps (we already have done this configuration in the app) and continues until the end of the wizard to finish with the app registration.

### Register your app with Firebase (Android)

If you are working with the Android platform it's necesary to register a new Android App in Firebase. From the **Project Overview**, use the **Add app** option and then select the Android icon to open the registration wizard.

![Image](/images/addApp.png)

In the registration wizard you must fill out the **Android package name** of the application (specified in the file: **/android/app/build.gradle**), for this project would be: _com.adventuretravelapp_. Later, for your particular project, you must use the **applicationId** you created your project with, but to test the app we will use this value first.

![Image](/images/addAndroidApp.png)

To finish download the generated configuration file **google-service.json** and add it to the project inside the **/android/app/** directory. This file contains all the information to link our app with the created project, allowing you to access any of the Firebase services.

![Image](/images/downloadJson.png)

Skip the next steps (we already have done this configuration in the app) and continues until the end of the wizard to finish with the app registration.

---
## Firebase Authentication

As authentication method we use the **Firebase Authentication** Email/Password provider, so it must be activated from the Firebase Console.

> Firebase Authentication provides backend services, easy-to-use SDKs, and ready-made UI libraries to authenticate users to your app. It supports authentication using passwords, phone numbers, popular federated identity providers like Google, Facebook and Twitter, and more. - [Firebase Authentication Docs![icon](/images/ext-link.png)](https://firebase.google.com/docs/auth){:target="_blank"}
{: .text-grey-dk-000 }

With the Email/Password provider the users can sign up using their email address and password. Firebase Authentication also provide email address verification, password recovery, and email address change primitives. To activate the Email/Password provider go to the **Authentication** menu of your project in [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"}. Then select the **Sign-in method** tab and activate the Email/Password provider to start using this authentication method in the app.

![Image](/images/FirebaseAuthentication.png)

---
## Firebase Realtime Database

In some cases you may need to handle data that requires realtime updating to be displayed on different devices at the same time. That is why we have incorporated **Firebase Realtime Database** (FRD) to store bookmarks, user information and bookings. When you connect your app to FRD, you’re actually connecting through a WebSocket. WebSockets are much faster than the HTTP protocol, you don’t have to make individual WebSocket calls, because a single socket connection is enough. This allows all your data to be automatically synchronised through that single WebSocket. When you save a change in data, all connected clients receive the updated data almost instantly.

![Image](/images/firebaseRealtimeDatabase.png)

For this project, or if you want to include it later in your own application, it’s necessary to create a Firebase Realtime Database. To do this just go to the **Realtime Database** menu from [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"} and click on **Create Database** button. Then select the option _Start in test mode_, just to start using it, later you must include some validation rules for a better security.

![Image](/images/CreatingFirebaseDB.png)

With the introduction of this remote database system we hope you will be able to use it plainly in your project for all the data you need to store and synchronize in realtime.

---
## Cloud Firestore

There are also data that may not require updating in real time and just run the typical CRUD (Create, Read, Update, Delete) operations of a database without any realtime subscription. In this case we also use **Cloud Firestore** to store application data such as experiences, categories and popular sites. To start using Firestore in your proyect go to the **Cloud Firestore** menu from [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"} and click on **Create Database** button. Then select the option _Start in test mode_, just to start using it, later you must include some validation rules for a better security.

![Image](/images/CreatingFirestoreDB.png)

---
At this point we should have all the backend configuration completed, then we can move on to make other [adjustments to the app](/docs/app-config) code and start running it.

## Cloud Messaging (Push Notifications)

We have added **push notifications** to this project, allowing you to send alerts to users using Firebase's **Cloud Messaging** service. Firebase Cloud Messaging (FCM) is a cross-platform messaging solution that lets you reliably send messages at no cost. Notifications can be sent directly from Firebase Console or through third party applications integrated with FCM.

The notification service is included by default in Firebase iOS and Android projects, however you need to make a few adjustments before you can start using it. For now we recommend to follow the guide to [configure the app and run it for the first time](/docs/app-config) and finally proceed with the settings for [Push Notifications](/docs/push-notifications).