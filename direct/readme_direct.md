# Direct Tap SDK for Android
***
*Version:* 2.4.0
***


## Table of Contents

  1. [Minimum Requirements](#requirements)
  2. [Installation](#installation)
  3. [Initialization](#initialization)
  4. [Usage](#usage)

***

## Minimum Requirements

1. **Android Studio 3.0** but preferably the latest version
2. Minimum Android SDK: **API 17** or **Android 4.2**

## Installation

This set of instructions assumes that the IDE being used is Android Studio

1. In your project build.gradle, ensure to add the URL of the repository under maven. Here is a sample:
	```
	allprojects {
    	repositories {
        		maven {
            		url = "https://maven.pkg.github.com/brankas/core-sdk-android"
            		credentials {
                			username = ""
                			password = ""
            		}
        		}
    	}
}
	```
**NOTE: You can use any GitHub Account in filling up the credentials**

2. In your app build.gradle file, add this line inside the dependencies configuration: **implementation "com.brankas.tap:direct-tap:2.4.0"** to set the SDK as a dependency for the application. This should look like:

	```gradle
	dependencies {
    	implementation "com.brankas.tap:direct-tap:2.4.0"
	}

3. Inside the the same dependencies configuration, insert the following lines to enable gRPC Connections which are needed by the SDK. Also, include RxJava for asynchronous listening to the results. **Do not forget to include compileOptions and kotlinOptions to use Java 8**

	```gradle
	dependencies {
 		implementation 'com.google.protobuf:protobuf-javalite:3.19.0-rc-1'
    		implementation 'io.grpc:grpc-okhttp:1.41.0'
    		implementation('io.grpc:grpc-protobuf-lite:1.41.0') {
        			exclude group: 'com.google.protobuf'
    		}
    		implementation 'io.grpc:grpc-stub:1.41.0'
			implementation 'io.reactivex.rxjava3:rxjava:3.0.0'
    		implementation 'io.reactivex.rxjava3:rxandroid:3.0.0'
			//implementation "org.jetbrains.kotlinx:kotlinx-coroutines-rx2:$kotlin_coroutines_version"
         implementation "androidx.security:security-crypto:1.1.0-alpha03"
	}

	compileOptions {
        		sourceCompatibility JavaVersion.VERSION_1_8
        		targetCompatibility JavaVersion.VERSION_1_8
    	}

    	kotlinOptions {
        		jvmTarget = "1.8"
    	}
	```
**NOTE: To mix coroutines and RxJava in the same project, include the optional dependency commented out**


4. Add the permission **android.permission.INTERNET** in your **AndroidManifest.xml** file to allow your application to access Internet, which is required to use Tap API Services.

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    ```
    
## Initialization

1. Call the initialize function from the DirectTapSDK and pass the context and api key provided by Brankas.

	```java

	import `as`.brank.sdk.tap.direct.DirectTapSDK

	DirectTapSDK.initialize(context, apiKey, null);

	```

***NOTE:*** It is better to call the initialize function inside the application class to avoid leaking of context or unexpected crash when other functions from the SDK are called in the background subsequently. Also, to use the **Sandbox** environment, set the optional **isDebug** option to **true**

2. The checkout function can now be called once the initialize function has been called.

## Usage

The SDK has a checkout function wherein it responds with a redirect url used to launch the Tap web application. An option is given either to use the url manually or let the SDK launch it through its internal WebView.

In order to use the checkout function, an **DirectTapRequest** is needed to be created and be passed. It has the following details:

1. **sourceAccount** - the account to be used as a sender of money for bank transfer. It consists of **BankCode** (code for a specific bank) and **Country** (country of origin)
<br/><br/>***NOTE:*** If **bankCode** is set to **null**, an internal bank selection screen will be shown inside Tap web application. If it has been filled up, that bank would automatically be selected instead.

2. **destinationAccountId** - the ID of the registered account of the receiver of money for bank transfer. This is provided by Brankas. Each registered account has a corresponding ID.

3. **amount** - the amount of money to be transferred. It consists of **Currency** (the currency of the money to be transferred) and the amount itself in centavos (e.g. If Php 1 would have been transferred, "100" should be passed)

4. **memo** - the note or description attached to the bank transfer

5. **customer** - pertains to the details of the sender of money. It consists of **firstName**, **lastName**, **email** and **mobileNumber**

6. **referenceId**

7. **client** - pertains to the customizations in the Tap Web Application and callback url once bank transfer is finished. It consists of **displayName** (name in the header to be shown in the Tap Web Application), **logoUrl** (URL of the logo to be shown), **returnUrl** (URL where Tap would be redirecting after bank transfer is finished), **failUrl** (optional URL where Tap would be redirecting if bank transfer has failed), **statementRetrieval** (optional Boolean that shows the list of statements after bank transfer is finished; its default value is false)

8. **showInBrowser** - option to let the SDK create the WebView in behalf of the Android Application

9. **dismissalDialog** - pertains to the showing of alert dialog when closing the WebView. It consists of **message**, **positiveButtonText** and **negativeButtonText**. Just set this value to null to remove the alert dialog when closing the application.

10. **expiryDate** -  refers to the expiry time of the created invoice, default value is null

11. **uniqueAmount** -  refers to the enabling of centavo reconciliation workaround logic, default value is UniqueAmount.NONE

Here is a sample on how to use it and call:

```java

import `as`.brank.sdk.core.CoreError
import `as`.brank.sdk.tap.TapListener
import `as`.brank.sdk.tap.direct.DirectTapSDK
import tap.common.*
import tap.common.direct.*
import tap.common.direct.Currency
import tap.direct.DirectTapRequest

DirectTapSDK.checkout(activity, 
	DirectTapRequest.Builder()
        	.sourceAccount(new Account(null, Country.PH))
        	.destinationAccountId("2149bhds-bb56-11rt-acdd-86667t74b165")
        	.amount(new Amount(Currency.PHP, "10000"))
        	.memo("Sample Bank Transfer")
        	.customer(new Customer("Owner", "Name", "sample@brankas.com", "63"))
        	.client(new Client("Sample Client", null, "www.google.com"))
        	.referenceId("sample-reference").build(),
	new CoreListener<String> {
        	@Override
        	public void onResult(String transactionId, CoreError error) {

        	}
	}, 1000)

// Used to retrieve the result from Tap Web Application
	@Override
	void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data)

        if(requestCode == 1000) {
        	// Transaction is successful
            if(resultCode == RESULT_OK) {
            	// Retrieve transaction id
                String transactionId = data.getStringExtra(DirectTapSDK.TRANSACTION_ID)
            }

		// Failed transaction
            else {
            	// Retrieve error message
                String error = data.getStringExtra(DirectTapSDK.ERROR)
                // Retrieve error status code
                String error = data.getStringExtra(DirectTapSDK.ERROR_CODE)
            }
        }
    }
```

***NOTE:*** If **showInBrowser** is set to **true**, the transactionId will be returned if bank transfer is successful else it would be null. If it has been set to **false**, the redirect URL for the Tap Web Application will be returned after checkout has been successful.<br/><br/>

The **useRememberMe** in the **checkout** function is set to true by default. To disable the usage of Remember Me inside the Tap Web Application, just pass false to the last parameter in the checkout function




