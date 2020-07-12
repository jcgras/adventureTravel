---
layout: default
title: Push Notifications
nav_order: 6
has_toc: true
---
# Push Notifications
{: .no_toc }

We have added **push notifications** to this project, allowing you to send alerts to users using **Firebase Cloud Messaging (FCM)** service. FCM is a cross-platform messaging solution that lets you reliably send messages at no cost. Notifications can be sent directly from Firebase Console or through third party applications integrated with FCM. If you want to set up push notifications for your project on iOS you need to make some security settings first. But for Android projects you are not required to do anything else and you can start testing by [broadcasting push notifications from FCM](/docs/push-notifications/#broadcasting-push-notifications-from-fcm).

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Apple Push Notifications service (APNs) configuration

> As a first requirement you should keep in mind that push notifications for iOS is only possible on **physical devices** and does not apply to simulators. Therefore, if you want to test push notifications in iOS, you should connect a physical device to your computer and run the app from there. Also you must have a paid Apple Developer Program account to create certificates for push notifications.
{: .text-grey-dk-000 }

Security certificates are required to start push notifications on iOS-based devices. Below we will describe the steps required to obtain and configure development certificates, which although indicated as being for development, it is also possible to use it in production applications. However, it is recommended to use certificates issued by certified authorities for production applications.

### Generate a Certificate Request

We must start by generating a certificate request, for that we must open the **Keychain Access** program, accessible from _Finder->Applications->Utilities_ in a MAC environment. From the main menu of Keychain Access, select the menu _Certificate Assistant->Request a Certificate From a Certificate Authority..._

![Image](/images/KeychainAccessMenu.png)

In the assistant you must fill out an applicant's email address, the name and email address of the authority, in this case as it is a development environment you can put any value. Then select **save to disk** and **continue** to generate a certificate request file that you should save for the next action.

![Image](/images/CertificateAssistant.png)

### Generate a certificate from Apple Developer Program

With the certificate request file, generated previously, you can get a certificate for the push notifications. To do that, you must log in to the [Apple Developer Program![icon](/images/ext-link.png)](https://developer.apple.com){:target="_blank"} site using your apple developer account and go to the **Account** menu and then to [**Certificates, IDs & Profiles**![icon](/images/ext-link.png)](https://developer.apple.com/account/resources/certificates/list){:target="_blank"}. Select **Identifiers** from the menu on the left to add/edit your app identifier.

![Image](/images/identifiers.png)

If your app identifier is not created yet you can click on the **plus** icon to add your app identifier. In this case you must register an **App ID**, specify the **Bundle ID** and check the option for **Push Notifications**. If your App ID already exists select it and go to **Push Notifications** and check it. Then select the **configure** button.

![Image](/images/configureIdentifier.png)

Then click on the **Create Certificate** button under the **Development SSL Certificate** option.

![Image](/images/DevelopmentSSLCertificate.png)

From the new screen you must upload the **Certificate Signing Request** file, generated previously.

![Image](/images/uploadCertificateRequest.png)

Finally click on **continue** to generate the certificate and download it. Open the downloaded certificate by double-clicking it to import it into **Keychain Access** and have it installed in your environment.

After the certificate is imported into Keychain Access, open the iOS project from Xcode and select the tab **Signing & Capabilities** and then the option **+ Capability**. Add the capabilities **Push Notifications** and **Background Modes** and check the option **Remote notifications**.

![Image](/images/xcodeCapability.png)

### Generate and import a .p8 key

To start sending push notifications from the **FCM** service it is necessary to first generate a .p8 key to sign the messages. To do this you must go back to [**Certificates, IDs & Profiles**![icon](/images/ext-link.png)](https://developer.apple.com/account/resources/certificates/list){:target="_blank"} from the **Apple Developer Program** site and select the **Keys** menu and click on the **plus** icon to add a new key. Specify a name for the new key, check the **APNs** option and click **continue**.

![Image](/images/newAPNsKey.png)

Download the key once it is generated. Note that you can only download once, so you must store it securely. Similarly copies the value of the **Key ID**, although you can always access this value from the **Keys** menu.

This .p8 key must be added to **Firebase Cloud Messaging** in order to issue push notifications. To do this you must go to the **Firebase Console** and access to the **Project Settings** menu.

![Image](/images/FirebaseSettings.png)

Then access to the **Cloud Messaging** tab and go down to the **iOS app configuration** option to upload the .p8 file.

![Image](/images/iOSAppConfig.png)

From the upload dialog you must to specify the **Key ID** related to the key and the **Team ID**; this last value is available from [Membership Details![icon](/images/ext-link.png)](https://developer.apple.com/account/#/membership){:target="_blank"}.

Once you successfully upload the key you are ready to broadcast push notifications!

## Broadcasting Push Notifications from FCM

Once you have the app configured to send push notifications you can start with a simple test to validate this. Remember that for notifications on iOS devices you must first [configure APNs](/docs/push-notifications/#apple-push-notifications-service-apns-configuration), however it will be possible to receive notifications as modals in any simulator while the app is active. That is, while the app is active you can receive notifications and show them in a dialog or any component you want to design for that. This behavior is defined by the `onMessage` method included in _/src/screens/splashScreen/index.js_ for handling new messages, while the app is in **active** mode. If the app is in **background** or **inactive** mode the notification will be handled by the system.

To start with a simple test run the React Native project and go to the console to copy the value of the **token key** generated for your device, printed on the React Native console.

![Image](/images/reactNativeConsole.png)

With this copied value go to the Firebase Console and select the **Cloud Messaging** menu and then click on the **Send your first message** button to compose the notification. From the **Compose notification** screen enter the **Notification title** and the **Notification text**, then click on the **Send test message** button.

![Image](/images/composeNotification.png)

In the new modal enter the **token key** value, copied previously, and click on **Test** button to send the notification to your device.

![Image](/images/modalTestNotification.png)

If you have a physical device connected and the app is not in **active** mode, you should see the push notification in the message box.

![Image](/images/deviceNotification.jpg)

If you are using a simulator or a connected physical device and the app is in **active** mode, you should see the notification from an Alert Dialog.

![Image](/images/AlertDialog.png)