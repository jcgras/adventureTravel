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
* Firebase Realtime Database (As database)
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

![Image](/images/addiOSApp.png)

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

In this app we use some data as a constant value in the local storage, but we also include a remote database system based on **Firebase Realtime Database** (FRD) to store bookmarks, user information and bookings. With the introduction of FRD we hope you will be able to use it plainly in your project for all the data you need to store permanently.

When you connect your app to FRD, you’re actually connecting through a WebSocket. WebSockets are much faster than the HTTP protocol, you don’t have to make individual WebSocket calls, because a single socket connection is enough. This allows all your data to be automatically synchronised through that single WebSocket. When you save a change in data, all connected clients receive the updated data almost instantly.

![Image](/images/firebaseRealtimeDatabase.png)

For this project, or if you want to include it later in your own application, it’s necessary to create a Firebase Realtime Database. To do this you just have to go to the **Database** menu from [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"} and then scroll down to the **Real Time Database** option and click on **Create Database** button. Then select the option _Start in test mode_, just to start using it, later you must include some validation rules for a better security.

![Image](/images/CreatingFirebaseDB.png)

---
At this point we should have all the backend configuration completed, then we can move on to make other [adjustments to the app](/docs/app-config) code and start running it.

## Cloud Messaging (Push Notifications)

We have added **push notifications** to this project, allowing you to send alerts to users using Firebase's **Cloud Messaging** service. Firebase Cloud Messaging (FCM) is a cross-platform messaging solution that lets you reliably send messages at no cost. Notifications can be sent directly from Firebase Console or through third party applications integrated with FCM. If you want to set up push notifications for your project in iOS you need to make some security settings first. But for Android projects you are not required to do anything else and you can start testing by [broadcasting push notifications from FCM](/docs/backend-config/#broadcasting-push-notifications-from-fcm).

### Apple Push Notifications service (APNs) configuration

> As a first requirement you should keep in mind that push notifications for iOS is only possible on **physical devices** and does not apply to simulators. Therefore, if you want to test push notifications in iOS, you should connect a physical device to your computer and run the app from there. Also you must have a paid Apple Developer Program account to create certificates for push notifications.
{: .text-grey-dk-000 }

Security certificates are required to start push notifications on iOS-based devices. Below we will describe the steps required to obtain and configure development certificates, which although indicated as being for development, it is also possible to use it in production applications. However, it is recommended to use certificates issued by certified authorities for production applications.

**1. Generate a Certificate Request**
{: .no_toc }

We must start by generating a certificate request, for that we must open the **Keychain Access** program, accessible from _Finder->Applications->Utilities_ in a MAC environment. From the main menu of Keychain Access, select the menu _Certificate Assistant->Request a Certificate From a Certificate Authority..._

![Image](/images/KeychainAccessMenu.png)

In the assistant you must fill out an applicant's email address, the name and email address of the authority, in this case as it is a development environment you can put any value. Then select **save to disk** and **continue** to generate a certificate request file that you should save for the next action.

![Image](/images/CertificateAssistant.png)

**2. Generate a certificate from Apple Developer Program**

With the certificate request file, generated previously, you can get a certificate for the push notifications. To do that, you must log in to the [Apple Developer Program![icon](/images/ext-link.png)](https://developer.apple.com){:target="_blank"} site using your apple developer account and go to the **Account** menu and then to [**Certificates, IDs & Profiles**![icon](/images/ext-link.png)](https://developer.apple.com/account/resources/certificates/list){:target="_blank"}.

![Image](/images/identifiers.png)

Select **Identifiers** from the menu on the left to add/edit your app identifier. If your app identifier is not created yet you can click on the **plus** icon to add your app identifier. In this case you must register an **App ID**, specify the **Bundle ID** and check the option for **Push Notifications**. If your App ID already exists select it and go to **Push Notifications** and check it. Then select the **configure** button.

![Image](/images/configureIdentifier.png)

Then select the **Create Certificate** button under the **Development SSL Certificate** option.

![Image](/images/DevelopmentSSLCertificate.png)

From the new screen you must upload the **Certificate Signing Request** file, generated previously.

![Image](/images/uploadCertificateRequest.png)

Then click on **continue** to generate the certificate and download it. Open the downloaded certificate by double-clicking it to import it into **Keychain Access** and have it installed in our environment.

After the certificate is imported into Keychain Access, open the iOS project from Xcode and select the tab **Signing & Capabilities** and then the option **+ Capability**. Add the capabilities **Push Notifications** and **Background Modes** and check the option **Remote notifications**.

![Image](/images/xcodeCapability.png)

**3. Generate and import a .p8 key**

To start sending push notifications from the **FCM** service it is necessary to first generate a .p8 key to sign the messages. To do this you must go back to [**Certificates, IDs & Profiles**![icon](/images/ext-link.png)](https://developer.apple.com/account/resources/certificates/list){:target="_blank"} from the **Apple Developer Program** site and select the **Keys** menu and click on the **plus** icon to add a new key. Specify a name for the new key, check the **APNs** option and click **continue**.

![Image](/images/newAPNsKey.png)

Download the key once it is generated. Note that you can only download once, so you must store it securely. Similarly copies the value of the **Key ID**, although you can always access this value from the **Keys** menu.

This .p8 key must be added to **Firebase Cloud Messaging** in order to issue push notifications. To do this you must go to the **Firebase Console** and access to the **Project Settings** menu.

![Image](/images/FirebaseSettings.png)

Then access the **Cloud Messaging** tab and go down to the **iOS app configuration** option to upload the .p8 file.

![Image](/images/iOSAppConfig.png)

From the upload dialog you must to specify the **Key ID** related to the key and the **Team ID**; this last value is available from [Membership Details![icon](/images/ext-link.png)](https://developer.apple.com/account/#/membership){:target="_blank"}.

Once you successfully upload the key you are ready to broadcast push notifications!

### Broadcasting Push Notifications from FCM

Una vez que tengas configurada la app para emitir notificaciones push puedes empezar con una prueba simple para valir la configuracion. Recuerda que para notificiaciones en dispositivos iOS debes primero [Configurar APNs](/docs/backend-config/#apple-push-notifications-service-apns-configuration), no obstante sera posible recibir notificaciones como modals en cualquier simulador mientras la app este activa. Si, mientras la app este activa podra recibir las notifiaciones y mostrarlas en un dialog o cualquier componente que quieras diseñar para eso. Esto es posible por el metodo `onMessage` incluido en _/src/screens/splashScreen/index.js_ for handling the new messages, with the app in active mode. Si la app esta en modo `background` o `inactive` la notificacion sera manejada por el sistema.

Para comenzar con una prueba simple ejecuta el proyecto de React Native y busca la consola para copiar el valor de la clave token generada para tu dispositivo, impresa en la consola de React Native.

![Image](/images/reactNativeConsole.png)

Con este valor copiado dirigete a la Consola de Firebase y selecciona el menu **Cloud Messaging** y luego da click en el boton **Send your first message**.