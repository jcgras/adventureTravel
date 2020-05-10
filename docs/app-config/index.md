---
layout: default
title: App configuration
nav_order: 3
has_toc: true
---
# App configuration
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Enviroment variables

If you need to use enviroment variables like URLs, API keys, usernames, passwords or any other parameter in your app, we include in the project the `.env` file to store it. We use the **react-native-dotenv** [package![icon](/images/ext-link.png)](https://www.npmjs.com/package/react-native-dotenv){:target="_blank"} to import the configuration variables from a `.env` file. If you need to use the Google Map Service, for example, you must to include the `GOOGLE_MAPS_API_KEY` variable with the API key value. You can see how to get a Google Maps API key from this [link![icon](/images/ext-link.png)](https://developers.google.com/maps/documentation/embed/get-api-key){:target="_blank"}.

_/.env_
``` js
GOOGLE_MAPS_API_KEY=YourApiKeyValueHere
ANOTHER_CONFIG=true
```
You can then import and use any of the defined variables.
``` js
import {GOOGLE_MAPS_API_KEY, ANOTHER_CONFIG} from 'react-native-dotenv';
```
If you have a separate development and production environment, that requires different configuration variables, you can use an `.env` file for the development/test environment variables and another `.env.production` file for the production environment variables.

---
## Script commands

In addition to the default commands, included in the global `package.json` file, we have added a few additional **yarn** commands to provide a quick access to useful tools for code maintenance. You can use, for example, the command `yarn clean` to delete the *node_modules*, auto-generated files and reinstall the *node_modules* again. This command can be useful when you have building errors or incompatibility between new installed modules. As we always say: feel free to add or change what you need, in this case any command.

_/package.json_
 ```js
 // .......
 "scripts": {
    "android": "react-native run-android",
    "ios": "react-native run-ios",
    "start": "react-native start",
    "test": "jest",
    "lint": "eslint .",
    "lint:fix": "eslint --fix .",
    "clean": "\\rm -fr ./node_modules dist/* ios/build ios/Pods ios/KScoreApp.xcarchive android/build android/app/build public/js public/assets && yarn"
  },
  // .......
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
Then, if you execute the command `yarn lint` from console a report is generated with the warnings and errors found in the whole project.

![Image](/images/eslint_terminal.png)

You can also use the command `yarn lint:fix` to try to fix as many issues as possible. The fixes are made to the actual files themselves and only the remaining unfixed issues are output. You should keep in mind that not all problems can be fixed using this option.

### Formatting the code

One way to maintain a common style of code in the project is by using a code formatting tool. In this project we have used *Prettier*, which is also integrated with *ESLint*. Prettier supports JavaScript but also other formats like JSX or JSON.

To include Prettier integrated with ESLint we have added `plugin:prettier/recommended` to ESLint configuration file `.eslintrc`.

_/.eslintrc_
```js
{
  "extends": ["@react-native-community", "plugin:prettier/recommended"]
}
```

Now by extending from `plugin:prettier/recommended` we can also get the unformatted errors using the same `yarn lint` command.

![Image](/images/eslint_terminal1.png)

This time, with the use of the `yarn lint:fix` command, the code will take the proper format, keeping the standard in all the source code.