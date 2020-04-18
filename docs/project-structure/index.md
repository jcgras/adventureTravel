---
layout: default
title: Project structure
nav_order: 4
has_toc: true
---
# Project Structure
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Application modules

> A structure must be simple enough for new team members to quickly get on board and immerse themselves into the project.
{: .text-grey-dk-000 }

React’s ecosystem offers users complete control over everything, without being tied to any particular way of doing things. However, whenever we work on a React project it is necessary to use some kind of consensus to organize the source code. With this in mind, we believe that the ideal React project structure is the one that allows you to move around your code with the least amount of effort. Following this principle, **Adventure Travel** is made up of a simple project structure that allows you to easily scalate, adapt, reuse and create React Native components. In any case, you are welcome to adjust it for your own use case.

React Native uses the JavaScript language to generate from the source code to iOS and Android platforms. We start from the JavaScript source code located in the **/src** folder of the project, inside this folder all the modules of the application will be organized.

![Image](/images/modules.png)

Folder | Description
---------|----------
 _common_ | It contains the common elements such as global styles, colors, utils, etc.
 _components_ | Here we place the application components and their related styles.
 _images_ | It contains the static images used in the project such as illustrations, logos, etc.
 _navigation_ | navigation
 _redux_ | redux
 _screens_ | It contains the components relating to the application screens and their related styles.

You have probably noticed that in each of these folders the files `index.js` and `package.json` are included. Both files are used to export and define the module with global access within the project.

![Image](/images/package_structure.png)

In the `index.js` file we must include all the module reference (imports) and then export it to make it visible outside the folder.

_/src/common/index.js_
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

![Image](/images/imageReference.png)

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

render() {
    return (
        <View>
            <Image source={Images.logoWhite} />
        </View>
    );
}
```
---
## Redux, one state to rule them all

> Redux handles the entire application data flow within a single container while the previous state persists as well.
{: .text-grey-dk-000 }

As an important part of our architecture, we include Redux for application status management. With Redux we can have one application state as a global state ("Single source of truth"), that includes all application data -like bookings and bookmarks- but also temporary states like search results, search history or popular destinations.

![Image](/images/redux_states.png){: .my-0 }
_Tree view from [React Native Debugger![icon](/images/ext-link.png)](https://github.com/jhen0409/react-native-debugger){:target="_blank"}_
{: .mx-8 .my-5 }

To use this library (actually Redux is a library) a **redux** module was created that includes the **actions** and **reducers** folders and the `store.js` file. Following the same logic as above, we have also included the `package.json` file for the module name.

![Image](/images/redux_module.png)

### Actions

The whole state of the app is stored in an object tree inside a single store. The only way to change the state tree is to emit an action, an object describing what happened. ([Redux Documentation![icon](/images/ext-link.png)](https://redux.js.org/introduction/getting-started){:target="_blank"})

Following this principle, the **actions** folder has been created as a container for the action files of each data object. For example, we have included the `bookmarks.js` file for all actions that manage bookmarks objects (saved items). Inside this file we can find a set of functions (Action Creators) to manage each action, for example, adding a new bookmark. So actions are the information (Objects) and action creator are functions that return these actions.

_/src/redux/actions/bookmarks.js_
``` js
const addBookmark = bookmark => {
    return {
        type: "ADD_BOOKMARK",
        payload: bookmark
    }
}

// .......

export {
    // .......
    addBookmark
};
```

### Reducers

Actions only tell what to do, but they don’t tell how to do, so reducers are the pure functions that take the current state and action and return the new state and tell the store how to do. To summarize, the reducers specify how the actions transform the state tree. We have included the **reducers** folder to group the reducers of each object.

_/src/redux/reducers/bookmarks.js_
``` js
const defaultState = [];

function reducer(state = defaultState, { type, payload }) {
    switch (type) {
        case "SET_BOOKMARKS": {
            return payload;
        }
        case "ADD_BOOKMARK": {
            return [...state, payload];
        }
        case "DELETE_BOOKMARK": {
            return state.filter(b => b.id !== payload);
        }
        case "CLEAR_BOOKMARKS": {
            return defaultState;
        }
        default:
            return state;
    }
}

export default reducer;
```

### Store

Basically the store is the object which holds the state of the application. We have created the `store.js` file to include the single store for the entire application, as recommended by the Redux documentation:
> It's important to note that you'll only have a single store in a Redux application. When you want to split your data handling logic, you'll use reducer composition instead of many stores. - [Redux Documentation![icon](/images/ext-link.png)](https://redux.js.org/basics/store){:target="_blank"}
{: .text-grey-dk-000 }

To start using the store instance you just need to import and call `createStore`.

``` js
import { createStore } from 'redux'
import todoApp from './reducers'

const store = createStore(todoApp)
```

But in our case we use multiple reducers and it is necessary to include the `combineReducers()` function to combine several reducers into one.

_/src/redux/store.js_
``` js
import { createStore, combineReducers } from "redux";
// ...
import searchHistory from "./reducers/searchHistory";
import bookmarks from "./reducers/bookmarks";
import bookings from "./reducers/bookings";

const reducer = combineReducers({
    // ...
    searchHistory,
    bookmarks,
    bookings
});
```

We also use `Redux-Persist` to save the Redux store when the app is closed and `Redux-Thunk` middleware to write Action Creators that return a function instead of an action. This last element is the one that allows us to create actions such as `addBookmark` as functions.

_/src/redux/store.js_
``` js
const persistConfig = {
    key: "root",
    storage: AsyncStorage,
    whitelist: ["user", "explore", "categories", "popular", "searchHistory", "bookmarks", "bookings", "config"],
    blacklist: []
};

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const persistedReducer = persistReducer(persistConfig, reducer);
const store = createStore(persistedReducer, composeEnhancers(applyMiddleware(thunk)));
let persistor = persistStore(store);

export { persistor, store };
```

How the store is connected to the UI is specified below.

---
## Components

As you probably know, React bases its architecture on components. That is: each piece of an app is handled as an isolated component (class or Hooks) where its own states, properties, styles and the access to the store are handled. With **Adventure Travel** you have a variety ready-to-use components to create your own mobile application. Components like `ButtonGradient`, `CardPopular` or `ImageCollage` can be found in the **/src/components** folder. Similarly we have created a folder to organize the components relating to the screens of the app: **/src/screens**. In this way we separate more atomic components like `ButtonGradient` from the more complex ones that compose a screen.

Basically each component extends from `React.PureComponent` and implements a `render()` method where the UI is returned in `jsx` format.

_/src/components/ButtonBookmark/index.js_
```js
import React from "react";
import PropTypes from "prop-types";

class ButtonBookmark extends React.PureComponent {
    constructor(props) {
        super(props);
    }

    render() {
        // ...
    }
}

ButtonBookmark.propTypes = {
    experienceId: PropTypes.number
}

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

In each component we also include the use of `propTypes` to declare the properties required by the component and its data type. When props are passed to a component, they are checked against the type definitions configured in the propTypes property. When an invalid value is passed for a prop, a warning is displayed on the console. To learn more about how you can use `prop-types` and all the available validators, see their [documentation![icon](/images/ext-link.png)](https://github.com/facebook/prop-types/blob/master/README.md){:target="_blank"}.

Likewise, each component includes the reference to the component's own styles using `StyleSheet` within the same file. Using the constant `styles` you can access the styles inside each component.

### Connecting the components to the store

To connect the Redux store to the UI (components) we use the `connect()` function from **react-redux**. This function provides to the connected component the data it requires from the store, and the functions it can use to send actions to the store. If the component requires the use of an action that handles the state of the application, such as `addBookmark`, we can import the functions that we had already created in the **actions** folder.

```js
import React from "react";
import { addBookmark, deleteBookmark } from "@redux/actions/bookmarks";
import { connect } from "react-redux";
```

Then we must create the `mapStateToProps` object to specify the properties we want to use from the global state and the `mapDispatchToProps` object to link the imported functions that act on the global state. We can then invoke the `connect` function using these objects as parameters and connect the component (`ButtonBookmark` in this example) to export it. This way you can access the declared properties to read the global state values and the functions to modify the global state.

_/src/components/ButtonBookmark/index.js_
```js
import React from "react";
import PropTypes from "prop-types";
import { addBookmark, deleteBookmark } from "@redux/actions/bookmarks";
import { connect } from "react-redux";

class ButtonBookmark extends React.PureComponent {
    // ...
}

// ...

const mapStateToProps = state => {
    return {
        bookmarks: state.bookmarks,
        experiences: state.explore.experienceResults.experiences
    };
};

const mapDispatchToProps = {
    addBookmark,
    deleteBookmark
};

export default connect(mapStateToProps, mapDispatchToProps)(ButtonBookmark);
```

Now within the functions of the component it is possible to access as a property to the global state of `bookmarks` (`this.props.bookmarks`) or `experiences` (`this.props.experiences`). Similarly, the functions `this.props.addBookmark` and `this.props.deleteBookmark` can be accessed as properties.

```js
render() {
    const booked = this.props.bookmarks.some(b => b.id == this.props.experienceId);
    return (
        <TouchableOpacity
            style={styles.content}
            onPress={booked ? this.removeBookmark : this.addBookmark}
        >
            <Icon
                name={booked ? "heart" : "hearto"}
                color={booked ? Color.heart : Color.background}
                style={styles.icon}
            />
        </TouchableOpacity>
    );
}

addBookmark = () => {
    const experience = this.props.experiences.find(e => e.id == this.props.experienceId);
    this.props.addBookmark(experience);
}

removeBookmark = () => {
    this.props.deleteBookmark(this.props.experienceId)
}
```
---
## Screens/Pages

Use FlatList.

react-navigation — Application navigation
Navigation structure