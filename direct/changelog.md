# Changelog of Direct Tap SDK

All notable changes to this project will be documented in this file.

## 4.2.1 - 2024-01-24

### Fixed

- crash when calling **checkoutViaFragment** function due to uninitialized view error

## 4.2.0 - 2024-01-08

### Added

- **Maya** Bank Code as Source Account

### Changed

- **Danamon** Bank Code Value
- **Kotlin** version from **1.7.1** to **1.9.10**
- **rxjava** version from **3.0.6** to **3.1.6**
- **rxandroid** version from **3.0.0** to **3.0.2**
- **core-ktx** version from **1.9.0** to **1.12.0**
- **gRPC** version from **1.45.1** to **1.51.1**
- **protobuf-javalite** version from **3.20.0** to **3.21.7**
- **targetSDKVersion** from **33** to **34**

## 4.1.0 - 2023-02-07

### Added

- **showBackButton** parameter in **checkout()** function to enable showing of a back button within the ActionBar
- function **closeTap()** to deliberately close the Tap Web App
- function **refreshTap()** to reload the current web page being shown within Tap Web App
- auto-refresh mechanism wherein the web page automatically reloads when Internet Connection has been restored

## 4.0.0 - 2023-01-16

### Added

-  **language** under **Client** in **DirectTapRequest** to be able to change the language within Tap Web App (default value is English)
-  optional internal logging to monitor API Service Calls and Tap Web Screen Flow
-  **isLoggingEnabled** within **initialize()** function to choose whether to enable or disable sending of logs; default value is true

### Changed

- Kotlin version from **1.5.1** to **1.7.1**

## 3.5.2 - 2022-10-21

### Fixed

- crash when a new Destination **BankCode** is added from **getSourceBanks()** function  
- crash Application attempted to call on a destroyed WebView for some Android devices

### Added

- added **isEnabled** field in **Bank** to determine if bank is enabled or not as a source bank for bank transfer

## 3.5.1 - 2022-08-29

### Fixed

- crash when passing an unknown **BankCode** to **checkout()** function
- crash when a new **BankCode** is added from **getSourceBanks()** function 

### Changed

- prohibited manual creation of **BankCode**

## 3.5.0 - 2022-07-25

### Fixed

- crash when passing an unlisted **BankCode** gotten from **getSourceBanks()** function to **checkout()** function
- empty checkout URL when calling **retrieveCheckoutURL()** function

### Added
- **getTransactionById()** function to search for a transaction by passing the transaction id
- **getTransactionByReferenceId()** function to search for a transaction by passing the reference id
- **getLastTransaction()** function to retrieve the last transaction performed by the user  (internal only; thus, when user changes device or clears the cache, last transaction will not be returned)

## 3.4.0 - 2022-05-27

### Added
- **isCorporate** field in **Bank**

### Changed
-  minimum SDK version from **17** to **21**
-  sorted returned banks from **getSourceBanks()** function
-  **rxjava** version from **3.0.0** to **3.0.6**
-  **protobufjavalite** version from **3.19.0-rc-1** to **3.20.0**
-  **gRPC** version from **1.41.0** to **1.45.1**

## 3.3.0 - 2022-04-27

### Added

-  **LandBank** and **EastWest** Banks in **BankCode** enum list
-  updating of ActionBar text via **checkout** function; if null is sent, the ActionBar will be hidden

## 3.2.0 - 2022-04-08

### Changed

-  package from **tap.common** to **tap.model**
-  **Bank logoUrl** format from **SVG** to **PNG** 

## 3.1.0 - 2022-03-28

### Changed

-  getSourceBanks parameter from CoreListener<List<BankCode>> to CoreListener<List<Bank>>

### Added

-  **Bank** class that includes **FundTransferLimit** (pertains to bank transfer minimum and maximum amounts), **FundTransferFee** (pertains to interbank and interbank transfer fees, if there are), **country**, **logo URL**, **name of the bank** and its corresponding **BankCode**


## 3.0.0 - 2022-02-04

### Fixed

-  crash within the Fragment if device goes to sleep mode or has been locked
-  minimized calling of the terminate() function

### Changed

- removed the interface **TapListener** and replaced with the existing **CoreListener** for returning transaction
- transaction id to **Transaction** object for the CoreListener callback
- renamed overloaded **checkout** function for launching the internal WebView within Fragment to **checkoutViaFragment** to avoid confusion of using the Activity alternative
- removed **showInBrowser** option within DirectTapRequest because a dedicated method has been created to accommodate the retrieval of checkout URL

### Added

-  internal function to call the retrieve transaction API Service to return the **Transaction** object every after Tap Web Session
-  a separate function called **retrieveCheckoutURL** for returning the checkout URL if client opts to launch a WebView manually
-  **getSourceBanks()** function to retrieve available banks for a specified destination bank

## 2.7.1 - 2022-01-28

### Fixed

- closing of fragment when success, failed or cancelled URL's are detected
- showing of dismissal dialog when back button is pressed for fragment implementation

### Changed

- arrow color for fragment implementation as optional

### Added

-  private **terminate()** function which is explicitly called when Internal WebView is destroyed

## 2.7.0 - 2022-01-25

### Added

-  private **terminate()** function which is explicitly called when Internal WebView is destroyed

## 2.6.1 - 2022-01-17

### Fixed

- automatic closing of Internal WebView when success or fail URL's do not start with *https*

## 2.6.0 - 2022-01-12

### Added

- function for retrieving the current SDK Version
- function for retrieving the Mobile Application Signature (either in SHA1, SHA256 or MD5 format)
- support for transactional retries within the WebView

## 2.5.0 - 2022-01-06

### Fixed

- End of transaction (success or failure) detection within the WebView for the new URL Format

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
