---
layout: default
title: Backend configuration
nav_order: 4
has_toc: true
---
# Backend Configuration
{: .no_toc }

As part of the architecture of this template we have added the use of Firebase as a complement for backend operations. Firebase is a Google Backend-as-a-Service (BaaS) platform that helps you build, improve and grow your applications. Firebase offers a wide range of features, but for the purposes of this project we will focus on the following services:

* Firebase Authentication (For our user authentication service)
* Firebase Realtime Database (As database)
<!-- * Cloud Messaging (For Push Notifications) -->

To start working on the project, it is necessary to first configure a new application from Firebase Console platform. Below we describe all the steps to configure the Firebase backend.

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Create a Firebase project

Before you can add Firebase to your app, you need to create a Firebase project to connect to your iOS or Android app. Follow these steps or consult the [Firebase documentation![icon](/images/ext-link.png)](https://firebase.google.com/docs?authuser=0){:target="_blank"} to create a new project.

1. Sign into [Firebase![icon](/images/ext-link.png)](https://console.firebase.google.com/?authuser=0){:target="_blank"} using a Google account.
2. In the **Firebase console**, click **Add project**, then select or enter your *Project name*
3. Click **Continue**.
4. Click **Create project**

Firebase automatically provisions resources for your Firebase project. When the process completes, you'll be taken to the overview page for your Firebase project in the Firebase console.

## Register your app with Firebase (iOS)

If you are working with the iOS platform it's necesary to register a new iOS App in Firebase. From the **Project Overview**, use the **Add app** option and then select the iOS icon to open the registration wizard.

![Image](/images/addApp.png)

In the registration wizard you must fill out the **iOS bundle ID** of your application. In this case we use the value: _org.reactjs.native.example.adventureTravelApp_, but you must change it according to the **Bundle Identifier** of your app, which must match the value specified in your Bundle Identifier located in the General tab for your app's primary target in Xcode, as described in the section [Setup your app name](/docs/app-config/#setup-your-app-name).

![Image](/images/addiOSApp.png)

To finish download the generated configuration file **google-service.plist** and include it into the project from Xcode.

![Image](/images/downloadPList.png)

Skip the next steps (we already have done this configuration in the app) and continues until the end of the wizard to finish with the app registration.

## Register your app with Firebase (Android)

If you are working with the Android platform it's necesary to register a new Android App in Firebase. From the **Project Overview**, use the **Add app** option and then select the Android icon to open the registration wizard.

![Image](/images/addApp.png)

In the registration wizard you must fill out the **Android package name** of your application. In this case we use the value: _com.adventuretravelapp_, but you must change it according to the **applicationId** of your app, which must match the value specified in the file: **/android/app/build.gradle**, as described in the section [Setup your app name](/docs/app-config/#setup-your-app-name).

![Image](/images/addAndroidApp.png)

To finish download the generated configuration file **google-service.json** and add it to the project inside the **/android/app/** directory.

![Image](/images/downloadJson.png)

Skip the next steps (we already have done this configuration in the app) and continues until the end of the wizard to finish with the app registration.

## Firebase Authentication

## Firebase Realtime Database

<!-- ## Cloud Messaging (Push Notifications) -->