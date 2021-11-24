# Changelog of Direct Tap SDK

All notable changes to this project will be documented in this file.

## 2.4.0 - 2021-10-25

### Changed

- refactored and moved some classes (*DirectTapSDK*, *DirectTapRequest*, *Account*, *Address*, *Amount*, *Client*, *Currency*, *Customer* and *UniqueAmount*) to direct-related packages (Do an Auto Import in Android Studio to fix previous imported packages).
- gradle dependencies:

Upgrade these libraries:
````
implementation 'com.google.protobuf:protobuf-javalite:3.14.0'
implementation 'io.grpc:grpc-okhttp:1.30.2'
implementation('io.grpc:grpc-protobuf-lite:1.30.2') {
         exclude group: 'com.google.protobuf'
}
implementation 'io.grpc:grpc-stub:1.30.2'
`````

to:
````
implementation 'com.google.protobuf:protobuf-javalite:3.19.0-rc-1'
implementation 'io.grpc:grpc-okhttp:1.41.0'
implementation('io.grpc:grpc-protobuf-lite:1.41.0') {
      exclude group: 'com.google.protobuf'
}
implementation 'io.grpc:grpc-stub:1.41.0'
````

### Added
-  error codes and messages when using BDO and BPI Transfers
- **ERROR_CODE** support on onActivityResult

## 2.3.0 - 2021-09-13

### Changed

- renamed all classes, methods and packages containing the name 'Tap' to 'DirectTap'
- TapSDK -> DirectTapSDK
- TapRequest -> DirectTapRequest

## 2.2.0 - 2021-08-09

### Added

- **expiryDate** parameter inside TapRequest to change expiry date of created invoice
- **uniqueAmount** parameter to enable centavo reconciliation workaround logic

### Fixed
- *Remember Me* when switching environments

## 2.1.0 - 2021-06-10

### Added

- **initSecurityCheck()** function to help detect if the host mobile application is tampered (Function is inaccessible)

### Fixed
- cancel button not functioning inside Tap Web Application when checkout function has been called

## 2.0.0 - 2021-05-28

### Changed

- renamed all classes, methods and packages containing the name 'IDP' to 'Tap'
- IdpSDK -> TapSDK
- IdpRequest -> TapRequest
- IDPListener -> TapListner
- onIdpStarted() -> onTapStarted()
- onIdpEnded() -> onTapEnded()

## 1.6.4 - 2021-05-24

### Fixed

- WebView SSL Error Handling (possible warning from PlayStore)

## 1.6.3 - 2021-05-07

### Changed

- packaging of classes and compressed the .aar

## 1.6.2 - 2021-04-30

### Changed

- webView Settings regarding cache and file access

## 1.6.1 - 2021-03-18

### Fixed

- crash when closing the WebView quickly after checkout()

## 1.6.0 - 2021-02-22

### Added

- support for *'Remember Me'* feature of pIDP Web Application
- support for encrypting credentials - **androidx.security:security-crypto:1.1.0-alpha03** should be added in gradle
- **useRememberMe** parameter in **checkout()** functions to optionally use *Remember Me*  Feature
- **clearRememberMe** function to clear the currently saved encrypted credentials

## 1.5.3 - 2021-02-03

### Changed

- Updated packaging of the SDK to avoid duplicate classes error when it is integrated together with other third-party libraries

## 1.5.2 - 2021-02-01

### Fixed

- Automatic closing of WebView when **Done** Button is clicked within the pIDP Web Application

## 1.5.1 - 2021-01-12

### Removed

- Dokka Integration (Java Docs) to eliminate conflicts with other third-party libraries

## 1.5.0 - 2021-01-08

### Added

- alternate **checkout()** function that supports the usage of Fragment instead of opening an Activity

## 1.4.0 - 2021-01-04

### Changed

- Updated checkout function and added **requestCode** parameter  to be used when starting the IDP Web Application for result
- Updated the way of retrieving the transaction id and error after IDP Web Application is finished - **onActivityResult** should now be used instead of **CoreListener**
- **CoreListener** can now only be used for getting the checkout URL if **showInBrowser** is set to **false**

## 1.3.0 - 2020-12-16

### Added

- **cancel()** function to prevent WebView from showing after calling **checkout()**
- option to show Alert Dialog when closing the WebView via **dismissalDialog** component of **IDPRequest**

### Fixed
- callback returning twice after successful transaction

## 1.2.2 - 2020-12-11

### Changed

- Updated the closing of Internal WebView when back button is pressed
- Updated the detection of successful transaction

## 1.2.1 - 2020-12-09

### Added

- Automatic closing of internal WebView when back button is pressed

### Changed

- **num** to **numInCents** and its format from **Float** to **String** from **Amount** class

## 1.2.0 - 2020-12-07

### Added

- Support for debug mode or usage of sandbox environment

### Changed

- **initialize()** function now accepts an optional Boolean **isDebug** (default value is false) to change the environment
- updated RxJava2 dependency to RxJava3
- changed **CountryISO3166** to **Country**
- changed **BankCode**
- changed **CurrencyISO4217** to **Currency**

## 1.1.0 - 2020-11-27

### Added

- Multi-tenant support

### Changed

- **initialize()** function can now accept API key besides the client name and secret

## 1.0.0 - 2020

### Added

- The whole IDP SDK (includes **checkout()** function and **IDPRequest** components - **Account**, **Address**, **Amount**, **BankCode**, **Client**,  **Customer**)
