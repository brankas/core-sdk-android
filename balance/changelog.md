# Changelog of Balance Tap SDK

All notable changes to this project will be documented in this file.

## 1.1.0 - 2022-07-08

### Added

- support for balance retrieval for Corporate Accounts
- **pngLogoUrl** field  within **Bank** for the logo in PNG format

### Changed

- automatic bank selection when only 1 bank code is passed via **bankCodes** within **BalanceTapRequest**

### Fixed

- closing of Tap Web Application when back button is pressed or Yes is clicked from **DismissalDialog**
- crash when passing an unlisted **BankCode** gotten from **getEnabledBanks()** function to **checkout()** function

## 1.0.2 - 2022-05-30

### Fixed

- missing field crash

## 1.0.1 - 2022-05-30

### Fixed

- missing **Account** and **Bank** classes


## 1.0.0 - 2022-05-27

### Added

- **initialize()** function that accepts the API Key and the chosen environment to interact with (Sandbox or Production)
- **checkout()** function that is used to redirect the user to the Tap Web Application for Balance Retrieval inside another activity
- **checkoutViaFragment()** function that is used to redirect the user to the Tap Web Application for Balance Retrieval inside a Fragment
- **getEnabledBanks()** function to retrieve available banks for balance retrieval
- **retrieveCheckoutURL()** function to get the checkout URL for balance retrieval
- **getSDKVersion()** function for retrieving the current SDK Version
- **getSignature()** function for retrieving the Mobile Application Signature (either in SHA1, SHA256 or MD5 format)