# Changelog of Statement Tap SDK

All notable changes to this project will be documented in this file.

## 4.2.0 - 2024-01-08

### Added

- **hasPdfUpload** parameter in **StatementTapRequest** to enable PDF Upload within Tap Web App

### Changed

- **Kotlin** version from **1.7.1** to **1.9.10**
- **rxjava** version from **3.0.6** to **3.1.6**
- **rxandroid** version from **3.0.0** to **3.0.2**
- **core-ktx** version from **1.9.0** to **1.12.0**
- **gRPC** version from **1.45.1** to **1.51.1**
- **targetSDKVersion** from **33** to **34**
- **protobuf-javalite** version from **3.20.0** to **3.21.7**

## 4.1.0 - 2023-02-07

### Added

- **showBackButton** parameter in **checkout()** function to enable showing of a back button within the ActionBar
- function **closeTap()** to deliberately close the Tap Web App
- function **refreshTap()** to reload the current web page being shown within Tap Web App
- auto-refresh mechanism wherein the web page automatically reloads when Internet Connection has been restored

## 4.0.0 - 2023-01-16

### Added

- **Balance Retrieval** Support (set **includeBalance** in **StatementTapRequest** to true)
- optional internal logging to monitor API Service Calls and Tap Web App Screen Flow
- **isLoggingEnabled** within **initialize()** function to choose whether to enable or disable sending of logs; default value is true

### Changed

- returned object after calling **checkout** function to **StatementResponse** which includes **statementId**, **statementList** (when **statementRetrievalRequest** is not null) and **accountList** (when **includeBalance** is set to true)
- **getEnabledBanks** to add support for Balance Retrieval (set **includeBalance** to true to get a combined set of banks that support both Statement and Balance Retrievals)
- Kotlin version from **1.5.1** to **1.7.1**

## 3.1.2 - 2022-10-12

### Fixed

- crash in Fragment Implementation (Can not perform this action after onSaveInstanceState)

## 3.1.1 - 2022-08-29

### Fixed

- crash when passing an unknown **BankCode** to **checkout()** function
- crash when a new **BankCode** is added from **getEnabledBanks()** function 

### Changed

- prohibited manual creation of **BankCode**

## 3.1.0 - 2022-07-08

### Added

- **pngLogoUrl** field  within **Bank** for the logo in PNG format

### Fixed

- crash when passing an unlisted **BankCode** gotten from **getEnabledBanks()** function to **checkout()** function

## 3.0.0 - 2022-05-27

### Added

- **initDownload()** function that is called in **onCreate()** function  to initialize and enable downloading of statement list in **CSV** format
- **downloadStatements()** function to return the statement data in byte array and option to directly download Statement List to the mobile phone in **CSV** format
- support for statement retrieval for Corporate Accounts
- **isCorporate** field in **Bank**

### Changed
-  minimum SDK version from **17** to **21**
-  sorted returned banks from **getEnabledBanks()** function
-  automatic bank selection when only 1 bank code is passed via **bankCodes** within **StatementTapRequest**
-  **rxjava** version from **3.0.0** to **3.0.6**
-  **protobufjavalite** version from **3.19.0-rc-1** to **3.20.0**
-  **gRPC** version from **1.41.0** to **1.45.1**

### Fixed

-  enabling of autoConsent within **StatementTapRequest**

## 2.2.0 - 2022-04-27

### Added

- updating of ActionBar text via **checkout** function; if null is sent, the ActionBar will be hidden

## 2.2.0 - 2022-04-27

### Added

- updating of ActionBar text via **checkout** function; if null is sent, the ActionBar will be hidden

## 2.1.0 - 2022-04-18

### Changed
-  package from **tap.common** to **tap.model**
-  getEnabledBanks parameter from CoreListener<List<BankCode>> to CoreListener<List<Bank>>

### Added

-  **Bank** class that includes **country**, **logo URL**, **name of the bank** and its corresponding **BankCode**

## 2.0.1 - 2022-02-21

### Fixed

- missing classes that are not generated properly

## 2.0.0 - 2022-02-17

### Added

- option to do Statement Retrieval via **StatementRetrievalRequest** passed inside **StatementTapRequest** after Tap Web Session
- added separate function **retrieveCheckoutURL** to get the checkout URL
- added **getEnabledBanks** function to retrieve available banks for statement retrieval

### Changed

- removed **showInBrowser** option from **StatementTapRequest** because just retrieving the URL is already in another function
- **bankCodes** from **StatementTapRequest** now accepts **null** value; if it is set to null, automatically all enabled banks will be shown in the Tap Web Application


## 1.4.0 - 2022-02-10

### Fixed

- crash when pressing the Home Button or PowerLock Button while Tap Web App is seen in the Host App (only applicable when Fragment Implementation is used)

### Changed

- renamed overloaded checkout() function for Fragment Implementation to **checkoutViaFragment** to avoid confusion with the Activity Implementation
- **useRememberMe** default value to **false** in both **checkout()** and **checkoutViaFragment()** options
- removed the interface **TapListener** and replaced with the existing **CoreListener** for returning transaction

## 1.3.2 - 2022-01-28

### Fixed

- closing of fragment when success, failed or cancelled URL's are detected
- showing of dismissal dialog when back button is pressed for fragment implementation

### Changed

- arrow color for fragment implementation as optional

## 1.3.1 - 2022-01-17

### Fixed

- automatic closing of Internal WebView when success or fail URL's do not start with *https*

## 1.3.0 - 2022-01-12

### Added

- function for retrieving the current SDK Version
- function for retrieving the Mobile Application Signature (either in SHA1, SHA256 or MD5 format)
- support for transactional retries within the WebView

## 1.2.0 - 2022-01-06

### Fixed

- End of transaction (success or failure) detection within the WebView for the new URL Format

## 1.1.0 - 2021-10-25

### Changed

- refactored and moved some classes (*StatementTapSDK*, *StatementTapRequest*) to statement-related packages (Do an Auto Import in Android Studio to fix previous imported packages).

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

## 1.0.0 - 2021-08-12

### Added

- initialize() function that accepts the API Key and the chosen environment to interact with (Sandbox or Production)
- checkout() function that is used to redirect the use the Tap Web Application for Statement Retrieval
- clearRememberMe() function that clears all credentials saved within the custom WebView
