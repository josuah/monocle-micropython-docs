.. _phone_app:

Phone Application Architecture
==============================
This is a React Native app, and the libraries most pertinent to software architecture are the following:
 - React Native 0.67.4 https://www.npmjs.com/package/react-native/v/0.67.4
 - React Native Navigation 7.26.0 https://www.npmjs.com/package/react-native-navigation/v/7.26.0
 - Redux Saga 1.1.3 https://www.npmjs.com/package/redux-saga/v/1.1.3
 - React Native SQLite Storage 6.0.1 https://www.npmjs.com/package/react-native-sqlite-storage/v/6.0.1

Coding convention notes:
 - Class components are used as opposed to hooks and functional components.
 - Promise Resolve is used as opposed to async await.
 - Events are processed using Redux Saga.

Please see RNN docs on https://wix.github.io/react-native-navigation/docs/before-you-start for navigation architecture. Navigation entrypoint is https://gitlab.com/brilliant_dave/monocle-phone-app/-/blob/main/src/screens/Navigation.tsx

Database operations are all local, as opposed to API calls to a web backend:
 - SQL queries are contained in https://gitlab.com/brilliant_dave/monocle-phone-app/-/blob/main/src/database/MainDao.ts
 - MainDao.ts runs queries by calling functions in https://gitlab.com/brilliant_dave/monocle-phone-app/-/blob/main/src/database/DatabaseManager.ts
 - Some queries trigger migrations and transactions which are mostly called from DatabaseManager.ts and mainly configured in https://gitlab.com/brilliant_dave/monocle-phone-app/-/blob/main/src/database/migrations/DatabaseMigrationManager.ts

Custom Native Modules:
 - Image/audio/video/media assets are processed for compression/decompressing/encoding/decoding (collectively "AssetProcessing") using custom Java and Obective-C code that is linked by React Native's native modules interface.
 - AssetProcessing interfaced with React Native via https://gitlab.com/brilliant_dave/monocle-phone-app/-/blob/main/src/natives/AssetProcessing.ts


Android Application Architecture
--------------------------------
See the contents of this directory for related Java code https://gitlab.com/brilliant_dave/monocle-phone-app/-/tree/main/android/app/src/main/java/com/monocle 

iPhone Application Architecture
-------------------------------
See the content of this directory https://gitlab.com/brilliant_dave/monocle-phone-app/-/tree/main/ios/AssetProcessing and this directory https://gitlab.com/brilliant_dave/monocle-phone-app/-/tree/main/ios/Encoder

Phone Application API References
--------------------------------
Currently there is no web/cloud API for operations such as checking for Monocle firmware updates. These need to be added.

A custom BLE library is currently being built and is due to be integrated into this app. As such, API details for that can be added to this doc when implementation is complete.
