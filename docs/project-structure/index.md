---
layout: default
title: Project structure
nav_order: 5
has_toc: true
---
# Project Structure
{: .no_toc }

In this session we will describe the structure designed for this project. With this design we intend to offer you a solution adaptable to any kind of management app. With the combination of Redux and Firebase Realtime Database technologies we can generate a synchronized system, which allows the update in real time with all your devices.

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Application modules

> A structure must be simple enough for new team members to quickly get on board and immerse themselves into the project.
{: .text-grey-dk-000 }

React’s ecosystem offers users complete control over everything, without being tied to any particular way of doing things. However, whenever we work on a React project it is necessary to use some kind of consensus to organize the source code. With this in mind, we believe that the ideal React project structure is the one that allows you to move around your code with the least amount of effort. Following this principle, **Adventure Travel** is made up of a simple project structure that allows you to easily scalate, adapt, reuse and create React Native components. In any case, you are welcome to adjust this structure for your own use case.

To get into context, let's start analyzing the project from the JavaScript source code located in the **/src** folder. Remember that React Native uses the JavaScript language to generate from the source code to iOS and Android platforms, both in the **/ios** and **android** folders respectively. Inside the **/src** folder all the modules of the application will be organized.

```
📂 src
 ┣ 📁 common
 ┣ 📁 components
 ┣ 📁 images
 ┣ 📁 navigation
 ┣ 📁 redux
 ┣ 📁 screens
 ┗ 📦 package.json
 ```

Module | Description
---------|----------
 _common_ | It contains the common elements such as global styles, colors, utils, database access, etc.
 _components_ | Here we place the application components and their related styles.
 _images_ | It contains the static images used in the project such as illustrations, logos, etc.
 _navigation_ | This module contains the components related to navigation. It contains the paths and references to screens, modals and tabs.
 _redux_ | It contains the elements related to Redux: reducers, middlewares and the store configuration.
 _screens_ | It contains the components relating to the application screens and their related styles.

You have probably noticed that in each of these modules the files `index.js` and `package.json` are included. Both files are used to export and define the module with global access within the project.

```
📂 src
 ┣ 📂 common
 ┃ ┣ Color.js
 ┃ ┣ Device.js
 ┃ ┣ Firestore.js
 ┃ ┣ GoogleAPIs.js
 ┃ ┣ Images.js
 ┃ ┣ index.js
 ┃ ┣ RealtimeDatabase.js
 ┃ ┣ Styles.js
 ┃ ┣ Util.js
 ┃ ┗ 📦 package.json
 ┣ 📁 components
 ┣ 📁 images
 ┣ 📁 redux
 ┣ 📁 screens
 ┗ 📦 package.json
```

In the `index.js` file we must include all the module reference (imports) and then export it to make it visible outside the folder.

_/src/common/index.js_
``` js
import {Styles, FontSize} from './Styles';
import Color from './Color';
import Device from './Device';
import Images from './Images';
import RealtimeDatabase from './RealtimeDatabase';
import Firestore from './Firestore';
import Util from './Util';
import GoogleAPIs from './GoogleAPIs';

export {
  Styles,
  Color,
  FontSize,
  Device,
  Images,
  RealtimeDatabase,
  Firestore,
  Util,
  GoogleAPIs,
};
```
Also using the `package.json` file we can define the name of the package as global and use it anywhere in the project.

_/src/common/package.json_
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

### Static images

In any mobile application project it is necessary to use static image files (embedded in the app) instead of always downloading them from the Internet. This applies mainly for image files such as illustrations, logos, icons, etc. For this reason we have included a single folder to store all the embedded image files, located in the path: **/src/images**. We have included also the `Images.js` file in the `@common` module to export the reference of all the static images, located in the images folder.

_/src/common/Images.js_
``` js
export default {
    logoWhite: require("@images/logos/logoWhite.png"),
    IllustSettings: require("@images/illustrations/settings.png"),
}
```
Whenever you need to use a static image just reference `Images` from the `@common` module.

``` js
import { Images } from "@common";

//....
return (
    <View>
        <Image source={Images.logoWhite} />
    </View>
);
```
---
## Redux, one state to rule them all

> Redux handles the entire application data flow within a single container while the previous state persists as well.
{: .text-grey-dk-000 }

As an important part of the architecture, we have included **Redux** for application status management. With Redux we can have one application state as a global state ("Single source of truth"), that includes all application data -like bookings and bookmarks- but also temporary states like search results, search history or popular destinations. The whole state of the app is stored in an object tree inside a single store. The only way to change the state tree is to emit an action, an object describing what happened. ([Redux Documentation![icon](/images/ext-link.png)](https://redux.js.org/introduction/getting-started){:target="_blank"})

![Image](/images/redux_states.png)<br>
_Tree view from [React Native Debugger![icon](/images/ext-link.png)](https://github.com/jhen0409/react-native-debugger){:target="_blank"}_
{: .my-5 .fs-3 }

To use this library (actually Redux is a library) a **redux** module was created that includes the **middlewares**, **reducers** folders and the `store.js` file. Following the same logic as above, we have also included the `package.json` file for the module name.

```
📂 src
 ┣ 📁 common
 ┣ 📁 components
 ┣ 📁 images
 ┣ 📂 redux
 ┃ ┣ 📁 middlewares
 ┃ ┣ 📁 reducers
 ┃ ┣ 📦 package.json
 ┃ ┗ store.js
 ┣ 📁 screens
 ┗ 📦 package.json
```

### Reducers

Reducers are the pure functions that take the current state and action and return the new state and tell the store how to do. To summarize, the reducers specify how the actions transform the state tree. We have included the **reducers** folder to group the reducers of each object.

_/src/redux/reducers/bookmarks.js_
``` js
const defaultState = [];
export const ADD_BOOKMARK = 'ADD_BOOKMARK';
export const DELETE_BOOKMARK = 'DELETE_BOOKMARK';
export const CLEAR_BOOKMARKS = 'CLEAR_BOOKMARKS';

function reducer(state = defaultState, {type, payload}) {
  switch (type) {
    case 'ADD_BOOKMARK': {
      return [...state, payload];
    }
    case 'DELETE_BOOKMARK': {
      return state.filter((b) => b.id !== payload);
    }
    case 'CLEAR_BOOKMARKS': {
      return defaultState;
    }
    default:
      return state;
  }
}

export default reducer;
```

### Dispatching actions

The only way to change the single source of data (state tree) is to emit an action. For this purpose we can use the React hook to trigger actions: `useDispatch` and the previous created reducers.

``` js
import {ADD_BOOKMARK} from '@redux/reducers/bookmarks';
import {useDispatch} from 'react-redux';

const dispatch = useDispatch();

const experience = {
    name: "Private Wine Country Tour",
    place: "Sonoma, US",
    destination: "San Francisco",
    duration: "1.5 hours",
    ...
}

dispatch({type: ADD_BOOKMARK, payload: experience});

```

### Middlewares

Basically a middleware is some code you can put between the framework receiving a request, and the framework generating a response. Redux middleware provides a third-party extension point between dispatching an action, and the moment it reaches the reducer. You can usually use Redux middleware for logging, crash reporting, talking to an asynchronous API, routing, and more. - [Redux Middleware![icon](/images/ext-link.png)](https://redux.js.org/advanced/middleware){:target="_blank"}

In this architecture we have included a **middleware** to manage the synchronization between remote data (Firebase Realtime Database) and local data (local store). For instance, when we dispatch the action **ADD_BOOKMARK** this middleware will be invoked, the action will be detected and the information will be sent to the remote database. At the same time the action is executed and the local store is updated from the corresponding **reducer**.

_/src/redux/middlewares/firebase.js_
``` js
import database from '@react-native-firebase/database';
import auth from '@react-native-firebase/auth';

const firebase = (store) => (next) => (action) => {
  switch (action.type) {
    case 'ADD_BOOKMARK':
      next(action);
      database()
        .ref(`/bookmarks/${auth().currentUser.uid}/${action.payload.id}`)
        .set({...action.payload, created: new Date().getTime()});
      break;
      //........
   }
};

export default firebase;
```
The above example applies to updating remote data, however when using Firebase Realtime Database, synchronization can occur in reverse. That is, if you update the remote data, either from another device or from another interface such as a website, you must also update the local store. To complete this cycle a **startListening** function has been added to "listen" all the time for remote Firebase updates and update the local store only if the information does not exist.

_/src/common/RealtimeDatabase.js_
```js
import database from '@react-native-firebase/database';
import {store} from '@redux/store';

const RealtimeDatabase = (() => {
  return {
    startListening: () => {
      const {user} = store.getState();
      /* Listening for new bookmark */
      database()
        .ref(`bookmarks/${user.uid}`)
        .orderByChild('created')
        .on('child_added', (snapshot) => {
          const newBookmark = snapshot.val();
          const {bookmarks} = store.getState();
          const exist = bookmarks.some((b) => b.id === newBookmark.id);
          // Add only if you are on another device (the bookmark doesn't exist)
          if (!exist) {
            store.dispatch({
              type: 'ADD_BOOKMARK',
              payload: newBookmark,
            });
          }
        });
    //........
})();

export default RealtimeDatabase;
```

### Store

The store is the object which holds the state of the application. We have created the `store.js` file to include the single store for the entire application, as recommended by the Redux documentation:
> It's important to note that you'll only have a single store in a Redux application. When you want to split your data handling logic, you'll use reducer composition instead of many stores. - [Redux Documentation![icon](/images/ext-link.png)](https://redux.js.org/basics/store){:target="_blank"}
{: .text-grey-dk-000 }

To start using the store instance you just need to import and call `createStore`. In our case we use multiple reducers, therefore it is necessary to include the `combineReducers()` function to combine several reducers into one.

_/src/redux/store.js_
``` js
import {createStore, combineReducers, compose, applyMiddleware} from 'redux';
import {persistStore, persistReducer} from 'redux-persist';
import thunk from 'redux-thunk';
import firebase from './middlewares/firebase';
// ...
import searchHistory from "./reducers/searchHistory";
import bookmarks from "./reducers/bookmarks";
import bookings from "./reducers/bookings";
import config from './reducers/config';

const reducer = combineReducers({
    // ...
    searchHistory,
    bookmarks,
    bookings,
    config,
});
```

We also use `Redux-Persist` to save the Redux store when the app is closed, and refer to the middlewares: `firebase` middleware (described above) and `Redux-Thunk` middleware to write Action Creators that return a function instead of an action. This last element is the one that allows us to create actions such as `addBookmark` as functions.

_/src/redux/store.js_
``` js
const persistConfig = {
  key: 'root',
  storage: AsyncStorage,
  whitelist: [
    'user',
    'explore',
    'categories',
    'popular',
    'searchHistory',
    'bookmarks',
    'bookings',
    'config',
  ],
  blacklist: [],
};

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const persistedReducer = persistReducer(persistConfig, reducer);
const store = createStore(
  persistedReducer,
  composeEnhancers(applyMiddleware(thunk, firebase)),
);
let persistor = persistStore(store);

export {persistor, store};
```

Below, we specify how the store is connected to the user interface.

---
# Components

As you probably know, React bases its architecture on components. That is: each piece of an app is handled as an isolated component (class or Hook) where its own states, properties, styles and the access to the store are handled. With **Adventure Travel** you have a variety ready-to-use components to create your own mobile application using <u>Hooks</u> approache. Components like `ButtonGradient`, `CardPopular` or `ImageCollage` can be found in the **/src/components** folder. Similarly we have created a folder to organize the components relating to the screens of the app: **/src/screens**. In this way we separate more atomic components like `ButtonGradient` from the more complex ones that compose a screen.

## Hooks approach

<u>Hooks</u> uses a function as a component with all properties included in a single object in the function parameter.

_/src/components/ButtonBookmark/index.js_
```js
import React, {useEffect} from "react";

const ButtonBookmark = ({experienceId, name, confirm = false}) => {
    useEffect(() => {
        // ...
    }, []);

    return (
        // ...
    );
};

const styles = StyleSheet.create({
    content: {
        alignSelf: "flex-end",
        alignItems: "flex-end",
        padding: 8
    },
    icon: {
        fontSize: 20
    }
});
```

Likewise, each component includes the reference to the component's own styles using `StyleSheet` within the same file. Using the constant `styles` you can access the styles inside each component.

### Connecting to the store

To connect the Redux store to the UI (components) we use the `useSelector` and `useDispatch` hooks from **react-redux**. The `useSelector` function provides to the component the data it requires from the store, and the `useDispatch` function it's used to send (dispatch) actions to the store. If the component requires the use of an action that handles the state of the application, such as `ADD_BOOKMARK`, you can import it from the **reducers** folder.

```js
import React from "react";
import {ADD_BOOKMARK, DELETE_BOOKMARK} from '@redux/reducers/bookmarks';
import {useSelector, useDispatch} from 'react-redux';
```

Then you can create a variable to access to any state of the application through the `useSelector` function. Also you can create a dispatch variable from the `useDispatch` hook to send an action to the store such as `ADD_BOOKMARK` or `DELETE_BOOKMARK`.

_/src/components/ButtonBookmark/index.js_
```js
import React from "react";
import {ADD_BOOKMARK, DELETE_BOOKMARK} from '@redux/reducers/bookmarks';
import {useSelector, useDispatch} from 'react-redux';

const ButtonBookmark = ({experienceId, name, confirm}) => {
  const bookmarks = useSelector((state) => state.bookmarks);
  const booked = bookmarks.some((b) => b.id === experienceId);
  const experiences = useSelector((state) => state.explore.experienceResults.experiences);

  const dispatch = useDispatch();

  const handleAddBookmark = () => {
    const experience = experiences.find((e) => e.id === experienceId);
    dispatch({type: ADD_BOOKMARK, payload: experience});
  };

  const handleRemoveBookmark = () => {
    if (confirm) {
      confirmRemoveBookmark();
    } else {
      dispatch({type: DELETE_BOOKMARK, payload: experienceId});
    }
  };

  return (
      // ...
  );
```

### Search component

From version 1.0.5 you can make dynamic searches of Experiences, we incorporated geolocation-based search methods for dynamic search results. We have added a simple algorithm to search for locations based on geohash coordinates, which you may find useful in your project.

**What is Geohashing anyway?**

Geohashing is a geocoding method used to encode geographic coordinates (latitude and longitude) into a short string of digits and letters delineating an area on a map, which is called a cell, with varying resolutions. The more characters in the string, the more precise the location. _[PubNub![icon](/images/ext-link.png)](https://www.pubnub.com/learn/glossary/what-is-geohashing/){:target="_blank"}_

In this case a **location** (latitude and longitude) and a **geohash** (the coding of **location**) has been added to each experience, stored in the _experiences_ collection.

![Image](/images/firestore_geohash.png)

To codify a geohash from coordinates you can use an online tool like [Geohashes Movable Type Scripts![icon](/images/ext-link.png)](https://www.movable-type.co.uk/scripts/geohash.html){:target="_blank"} or for React Native, use the **ngeohash** library:

```js
import geohash from 'ngeohash';

const lat = 38.7437396;
const lon = -9.230243;
const geohash = geohash.encode(lat, lon);
console.log(geohash); // output: eyckmv
```

With a range of geohash codes (_lower_ and _upper_) it is possible to do a search by a single _string_ index (`geohash`) in the database, where the closest geohashes in string indicate the proximity in distance:

```js
// Search of experiences in database according to a range of geohahes
const querySnapshot = await firestore()
    .collection('experiences')
    .where('geohash', '>=', geoRange.lower)
    .where('geohash', '<=', geoRange.upper)
    .get();
```