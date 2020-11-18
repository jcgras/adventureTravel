---
layout: default
title: Firebase Database Rules
nav_order: 7
has_toc: true
---
# Firebase Database Rules
{: .no_toc }

Firebase Database Rules (in both scenarios: Realtime Database and Firestore) determine who has read and write access to your database, how your data is structured, and what indexes exist. These rules live on the Firebase servers and are enforced automatically at all times. Every read and write request will only be completed if your rules allow it.

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Realtime Database Rules

To manage the security rules of Realtime Databases you must go to the **Realtime Database** menu from [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"} and select the **Rules** tab. If you have created a database in `test mode`, you probably have a security rules configuration where you allow anyone to do read and write operations to the database.

_Default security rules in **test mode**:_
```js
{
    "rules": {
        ".write": true,
        ".read": true,
    }
}
```
This configuration is appropriate for development and testing environments, but for a production environment it is recommended to add more restrictive rules. For instance we could add rules where for each collection that you try to read or modify is necessary before being authenticated. For that we could use rules like the following:

```js
{
  "rules": {
    "users": {
      "$uid": {
        ".write": "$uid === auth.uid",
        ".read": "$uid === auth.uid",
      }
    },
    "bookmarks": {
      "$uid": {
        ".write": "$uid === auth.uid",
        ".read": "$uid === auth.uid",
      }
    },
    "bookings": {
      "$uid": {
        ".write": "$uid === auth.uid",
        ".read": "$uid === auth.uid",
      }
    }
  }
}
```

To deepen the security rules of Realtime Database you can review the [official documentation![icon](/images/ext-link.png)](https://firebase.google.com/docs/database/security#section-overview){:target="_blank"}.

## Cloud Firestore Security Rules

Similar to Realtime Database, **Firestore** handles security rules for database operations. To manage these security rules you must go to the **Cloud Firestore** menu from [Firebase Console![icon](/images/ext-link.png)](https://console.firebase.google.com){:target="_blank"} and select the **Rules** tab. If you have created a database in `test mode`, you probably have a security rules configuration to allows anyone on the internet to view, edit, and delete all data in your Firestore database. This configuration may also have an expiration time of 30 days, after which all client requests to your Firestore database will be denied.

_Default security rules in **test mode**:_
```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.time < timestamp.date(2020, 8, 2);
    }
  }
}
```

As you can see, this configuration leaves your app open to attackers. Therefore it is necessary to add rules that guarantee a minimum of security. It would be enough to begin that all the solitudes are of reading only and that the user has to be authenticated to be able to access the documents.

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read: if request.auth != null;
    }
  }
}
```

In this session we only intend to introduce the concept of **Firebase Database Rules** so you can add a set of rules that guarantee a minimum of safety. If you want to go deeper and add other more complex configurations, you can access the [Cloud Firestore Security Rules documentation![icon](/images/ext-link.png)](https://firebase.google.com/docs/firestore/security/rules-structure){:target="_blank"}.