# Changelog of Statement Tap SDK

All notable changes to this project will be documented in this file.

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
