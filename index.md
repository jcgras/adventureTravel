# Adventure Travel - React Native Template

With **Adventure Travel** template we want to offert more than a set of components and styles. We have carefully created a project where we aim to provide a simple structure for any kind of React Native project, allowing you to adapt, reuse and create react native components easily.

For this project we have been inspired by a travel agency, however, we intend that this template can also be used in a wide range of businesses. If you had a web page for your business, **Adventure Travel** supports you to convert/extend your current websites to mobile app efficiently.

### What do we include in general?

- Support for iOs and Android
- React Native with JSX
- Redux
- Backend integration with REST API/Firebase
- Push Notifications via Firebase
- Maps and Geolocation
- Google APIs integration
- Social logins via facebook or Google
- Flexible variable uses via config file

## Reference links

Try a demo App, avalilable download on Appstore and Google Play:

Purchase or more info | [Adventure Travel](https://1.envato.market/xxxx)
---------|----------
Documentation | https://docs.svalbard.dev
iOS demo | 	https://itunes.apple.com/in/app/adventureTravel/id999999999
Android demo | https://play.google.com/store/apps/details?id=com.svalvard.adventureTravel

## Quick start

Install for iOs / install for Android

## Configuration

If you need to use enviroment variables like URLs, API keys, usernames, passwords or any other parameter, we include in the project the `.env` file to store it. We use the `react-native-dotenv` [package](https://www.npmjs.com/package/react-native-dotenv) to import the configuration variables from a .env file. If you need to use the Google Map Service, for example, you must to include the `GOOGLE_MAPS_API_KEY` variable with the API key value. (_You can see how to get a Google Maps API key from this [link](https://developers.google.com/maps/documentation/embed/get-api-key)._)

##### **`/.env`**
``` js
GOOGLE_MAPS_API_KEY=YourApiKeyValueHere
ANOTHER_CONFIG=true
```
You can then import and use any of the defined variables.
``` js
import { GOOGLE_MAPS_API_KEY, ANOTHER_CONFIG } from 'react-native-dotenv';
```
If you have a separate development and production environment, that requires different configuration variables, you can use an `.env` file for the development/test environment variables and another `.env.production` file for the production environment variables.

## Project structure

> A structure must be simple enough for new team members to quickly get on board and immerse themselves into the project.

React's ecosystem offers users complete control over everything, without being tied to any particular way of doing things. However, whenever we work on a React project it is necessary to use some kind of consensus to organize the source code. With this in mind, we believe that the ideal React project structure is the one that allows you to move around your code with the least amount of effort. Following this principle, **Adventure Travel** is made up of a simple project structure that allows you to easily scalate, adapt, reuse and create React Native components. In any case, you are welcome to adjust it for your own use case.

### Application modules

In the project source (on the `src` folder) we have the main folders where all the elements of the application are organized.

Folder | Description
---------|----------
 `common` | It contains the common elements such as global styles, colors, utils, etc.
 `components` | components
 `images` | It contains the static images used in the project such as illustrations, logos, etc.
 `navigation` | navigation
 `redux` | redux
 `screens` | screens

You have probably noticed that in each of these folders the files `index.js` and `package.json` are included. Both files are used to export and define the module with global access within the project.

![Image](/images/package_structure.png)

In the `index.js` file we must include all the module reference (imports) and then export it to make it visible outside the folder.

##### **`/src/common/index.js`**
``` js
import Styles from "./Styles";
import Color from "./Color";
import Device from "./Device";
import Images from "./Images";
import Coordinates from "./Coordinates";
import Util from "./Util";

export { Styles, Color, Device, Images, Coordinates, Util }
```
Also using the `package.json` file we can define the name of the package as global and use it anywhere in the project.

##### **`/src/common/package.json`**
``` json
{
  "name": "@common"
}
```

Now if you need to import something (like `Color`) from the `common` module you can refered it using `@common` route instead of all the static route.

``` js
import { Color } from "@common";

const primaryColor = Color.primary;
```

### One place for static images

In any mobile application project it is necessary to use static image files (embedded in the app) instead of always downloading them from the Internet. This applies mainly for image files such as illustrations, logos, icons, etc. For this reason we have included a single folder to store all the embedded image files, located in the path: `/src/images`. We have included also the `Images.js` file in the `@common` module to export the reference of all the static images, located in the images folder.

![Image](/images/imageReference.png)

##### **`/src/common/Images.js`**
``` js
export default {
    logoWhite: require("@images/logos/logoWhite.png"),
    IllustSettings: require("@images/illustrations/settings.png"),
}
```
Whenever you need to use a static image just reference `Images` from the `@common` module.

``` js
import { Images } from "@common";

render() {
    return (
        <View>
            <Image source={Images.logoWhite} />
        </View>
    );
}
```

## We use Redux

Application state management

redux-thunk — Enabling asynchronous dispatching of actions
Redux Thunk middleware allows you to write action creators that return a function instead of an action. The thunk can be used to delay the dispatch of an action, or to dispatch only if a certain condition is met. The inner function receives the store methods dispatch and getState as parameters.

## Components

With **Adventure Travel** you have a variety ready-to-use components to create a modern and beautifull mobile application.

Uso de PropTypes para validaciones, uso de StyleSheet dentro de archivo de componente. Crear archivos package.json y index para incluir archivos a exportar y rehusar. 
To avoid having to import components like this

import { TinyButton } from ‘./TinyButton/TinyButton'

make an index.js in that components folder which export the named component.

// components/TinyButton/index.js
export * from ‘./TinyButton'

Component’s file name should be in Pascal Case.
Component names should be like TabSwitcher and not like tabSwitcher, tab-switcher etc. Also no other type of file should be in Pascal Case. This way when we see a filename in Pascal Case, it is immediately clear that the file is a react component.
Use FlatList.

## Screens/Pages

Filename for Page Component should be lowercase
The page name corresponds to the route name. Route names in url are lowercase, it makes sense to have the file name lowercase also.

react-navigation — Application navigation
Navigation structure

---

Also we include:

- Easy customization for your brand
- Firebase Synchronize
- Instant Search
- Support filter by category, tab and pricing
- Search history and clean up
- Bookmarks and sync across devices
- User Profile
- Walkthrough animation
- Header animation
- Easy to change colors and styles
- Support to build app via Expo or react-native-cli
- Regular feature update and free bug fix
