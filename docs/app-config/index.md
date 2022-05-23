---
layout: default
title: App configuration
nav_order: 4
has_toc: true
---
# App configuration
{: .no_toc }

Once the backend is configured, we will make some adjustments to build and execute the app.

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Google APIs setup

Google API offers an indispensable set of features that can be very useful as a complement or essence of many modern applications. Services such as Search, Gmail, Translate, Calendar or Google Maps are included here and we can take advantage of them for use in our apps. In this project we focus on two of these services:

* Google Places (for the searches)
* Google Maps

![Image](/images/goople_apis.png)

To start using the Google API services you must first create a (free) [Google Cloud Platform![icon](/images/ext-link.png)](https://console.cloud.google.com){:target="_blank"} account and follow these [steps![icon](/images/ext-link.png)](https://developers.google.com/maps/documentation/embed/get-api-key){:target="_blank"} to get, restrict and enable a Google API key. Once you get your API key, continue with the following statement to add it to the environment file.

![Image](/images/configGoogleKey.gif)

### Enviroment variables

If you need to use enviroment variables like URLs, API keys, usernames, passwords or any other parameter in your app, we include the `.env` file to store it. We use the **react-native-dotenv** [package![icon](/images/ext-link.png)](https://www.npmjs.com/package/react-native-dotenv){:target="_blank"} to import the configuration variables from a `.env` file. If you need to use the Google Map Service, for example, you must to include the `GOOGLE_API_KEY` variable with the API key value.

_/.env_
``` js
GOOGLE_API_KEY=YOUR_API_KEY_VALUE_HERE
ANOTHER_CONFIG=true
```
You can then import and use any of the defined variables.
``` js
import {GOOGLE_API_KEY, ANOTHER_CONFIG} from 'react-native-dotenv';
```
If you have a separate development and production environment, that requires different configuration variables, you can use an `.env` file for the development/test environment variables and another `.env.production` file for the production environment variables that are used in releases.

### Additional settings for Google Maps

In any case that you need to use Google Maps in your project, you must also add the value of the api key (previously obtained from [Google Cloud Platform Console![icon](/images/ext-link.png)](https://developers.google.com/maps/documentation/embed/get-api-key){:target="_blank"}) in a couple of additional files.

For **IOS** based applications you must edit **/ios/adventureTravelApp/AppDelegate.m** (line: 23) file and add the value of the api key in the line indicated:

_/ios/adventureTravelApp/AppDelegate.m_

```diff
 ...
 #import <React/RCTRootView.h>
 #import <GoogleMaps/GoogleMaps.h>

 @implementation AppDelegate

 - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
 {
   if ([FIRApp defaultApp] == nil) {
     [FIRApp configure];
   }
+  [GMSServices provideAPIKey:@"YOUR_API_KEY_VALUE_HERE"];
   RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
   ...
```

For **Android** based applications you must edit **/android/app/src/main/AndroidManifest.xml** file and add the value of the api key in the line indicated:

_/android/app/src/main/AndroidManifest.xml_

``` diff
     ...
     <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" />
+    <meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR_API_KEY_VALUE_HERE"/>
     <uses-library android:name="org.apache.http.legacy" android:required="false"/>
 </application>
 ...
```

---
## Building the app 🚀

Once you have all the previous configurations ready, we can then start the build and execution of the project. Let's start by installing the **node modules**, to do this go to the project folder from the terminal and run:
```
npm install
```
and wait for all the packages to be installed.

### Running on iOS environment

For iOS environment it's necesary to install the Podfile. The Podfile it's located in **/ios/Podfile** project route and contains the dependencies of the node modules that target the Xcode project. To install this references, access from the terminal to the _ios_ folder and install the pod files:
```
cd ios/
pod install
```
Note that every time you add or update a node module you must execute this action to update the references in the iOS project. After the references are installed, go back to the project root folder and run the command to start the application:
```
cd ..
npm run ios
```
With this command you must generate the application for iOS and raise the simulator you have configured. If you have any problem when you start the app for the first time, try to run it from Xcode to validate the project configuration.

### Running on Android environment

If you're working in an Android environment, to start the app you only have to run from the terminal:
```
npm run android
```
Remember that you must have an Android simulator installed correctly for the application to run successfully. In case you have any problem running the app for the first time open with **Android Studio** the generated android project, located in the **/android** folder of the project. From Android Studio verify that the sync runs correctly from: **File->Sync Project with Gradle Files** menu and also that you have a simulator configured. 

### Running for the first time

The first time you run the application, sample data will be registered in the **Cloud Firestore database** (previously registered in the [backend configuration](/docs/backend-config/#cloud-firestore)). This way, experiences, categories and popular places will be registered in the database, which can be used as a starting point to add your own data, either from the **Firebase Console** or from a third App.

![Image](/images/cloudFirestoreData.png)

---
## Script commands

In addition to the default commands, included in the global `package.json` file, we have added a few additional **npm** commands to provide a quick access to useful tools for code maintenance. You can use, for example, the command `npm run clean` to delete the *node_modules*, auto-generated files and reinstall the *node_modules* again. This command can be useful when you have building errors or incompatibility between new installed modules. As we always say: feel free to add or change what you need, in this case any command.

_/package.json_
 ```js
 ...
 "scripts": {
    "android": "react-native run-android",
    "ios": "react-native run-ios",
    "start": "react-native start",
    "test": "jest",
    "lint": "eslint .",
    "lint:fix": "eslint --fix .",
    "clean": "\\rm -fr ./node_modules dist/* ios/build ios/Pods ios/KScoreApp.xcarchive android/build android/app/build public/js public/assets && npm install"
  },
  ...
 ```

---
## Ensuring code quality

As you may have noticed, in the previous scripts, we have also included the `lint` command to run the *ESLint* tool. With this tool we can keep our code according to the ECMAScript standard and other rules you want to use to detect and avoid possible errors and inconsistencies.

> ESLint is a tool for identifying and reporting on patterns found in ECMAScript/JavaScript code, with the goal of making code more consistent and avoiding bugs. - [ESLint Documentation![icon](/images/ext-link.png)](https://eslint.org/docs/user-guide/getting-started){:target="_blank"}
{: .text-grey-dk-000 }

To configure *ESLint* you can open the `.eslintrc` file from the main path. In this file you can include the rules for code validation, however in our case we will extends the rules from `react-native-community`, the [official package![icon](/images/ext-link.png)](https://github.com/facebook/react-native/tree/master/packages/eslint-config-react-native-community#readme){:target="_blank"} for react-native. If you want to extend from another configuration or include your own rules, you can consult the [official ESLint documentation![icon](/images/ext-link.png)](https://eslint.org/docs/user-guide/getting-started){:target="_blank"}.

_/.eslintrc_
```js
{
    "extends": "@react-native-community"
}
```
Then, if you execute the command `npm run lint` from console a report is generated with the warnings and errors found in the whole project.

![Image](/images/eslint_terminal.png)

You can also use the command `npm run lint:fix` to try to fix as many issues as possible. The fixes are made to the actual files themselves and only the remaining unfixed issues are output. You should keep in mind that not all problems can be fixed using this option.

### Formatting the code

One way to maintain a common style of code in the project is by using a code formatting tool. In this project we have used *Prettier*, which is also integrated with *ESLint*. Prettier supports JavaScript but also other formats like JSX or JSON. To include Prettier integrated with ESLint we have added `plugin:prettier/recommended` to the ESLint configuration file.

_/.eslintrc_
```js
{
  "extends": ["@react-native-community", "plugin:prettier/recommended"]
}
```

We have also added the rules for Prettier in the `.prettierrc` configuration file. Here we can add the rules that we estimate to validate the coding style. You can see more details of the Prettier configuration from the [documentation site![icon](/images/ext-link.png)](https://prettier.io/docs/en/configuration.html){:target="_blank"}.

*/.prettierrc*
```js
{
  "bracketSpacing": false,
  "jsxBracketSameLine": true,
  "singleQuote": true,
  "trailingComma": "all"
}
```

Now with this last configuration we can also get the unformatted errors using the same `npm run lint` command.

![Image](/images/eslint_terminal1.png)

This time, with the use of the `npm run lint:fix` command, the code will take the proper format, keeping the standard in all the source code.

**If you are using Visual Studio Code you should try the [ESLint extension![icon](/images/ext-link.png)](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint){:target="_blank"}. The extension will highlight the linter error right inside the editor, and you can disable or tweak the rule however you want.*
{: .text-grey-dk-000 }