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

If you need to use enviroment variables like URLs, API keys, usernames, passwords or any other parameter in your app, we include in the project the `.env` file to store it. We use the `react-native-dotenv` [package](https://www.npmjs.com/package/react-native-dotenv) to import the configuration variables from a `.env` file. If you need to use the Google Map Service, for example, you must to include the `GOOGLE_MAPS_API_KEY` variable with the API key value. (_You can see how to get a Google Maps API key from this [link](https://developers.google.com/maps/documentation/embed/get-api-key)._)

_/.env_
``` js
GOOGLE_MAPS_API_KEY=YourApiKeyValueHere
ANOTHER_CONFIG=true
```
You can then import and use any of the defined variables.
``` js
import { GOOGLE_MAPS_API_KEY, ANOTHER_CONFIG } from 'react-native-dotenv';
```
If you have a separate development and production environment, that requires different configuration variables, you can use an `.env` file for the development/test environment variables and another `.env.production` file for the production environment variables.
