# Changelog of Statement Tap SDK

All notable changes to this project will be documented in this file.

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
