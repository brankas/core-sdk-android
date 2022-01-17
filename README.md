# Direct Tap SDK for Android
***
*Version:* 2.6.0
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

2. In your app build.gradle file, add this line inside the dependencies configuration: **implementation "com.brankas.tap:direct-tap:2.6.0"** to set the SDK as a dependency for the application. This should look like:

	```gradle
	dependencies {
    	implementation "com.brankas.tap:direct-tap:2.6.0"
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

	import as.brank.sdk.tap.direct.DirectTapSDK;

	DirectTapSDK.INSTANCE.initialize(context, apiKey, null, false);

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

import as.brank.sdk.core.CoreError;
import as.brank.sdk.tap.TapListener;
import as.brank.sdk.tap.direct.DirectTapSDK;
import tap.common.*;
import tap.common.direct.*;
import tap.common.direct.Currency;
import tap.direct.DirectTapRequest;

DirectTapSDK.INSTANCE.checkout(activity, 
	new DirectTapRequest.Builder()
        	.sourceAccount(new Account(null, Country.PH))
        	.destinationAccountId("2149bhds-bb56-11rt-acdd-86667t74b165")
        	.amount(new Amount(Currency.PHP, "10000"))
        	.memo("Sample Bank Transfer")
        	.customer(new Customer("Owner", "Name", "sample@brankas.com", "63"))
        	.client(new Client("Sample Client", null, "www.google.com"))
        	.referenceId("sample-reference").build(),
	new TapListener<String> {
	@Override
            public void onTapStarted() {

            }

            @Override
            public void onTapEnded() {

            }

            @Override
            public void onResult(@Nullable String str, @Nullable CoreError coreError) {
                if(coreError != null) {
                    showProgress(false);
                    showMessage(coreError.getErrorMessage());
                }
                else
                    showMessage("Transaction Successful! Here is the transaction id: "+str);
            }
        	@Override
        	public void onResult(String transactionId, CoreError error) {

        	}
	}, 1000);

// Used to retrieve the result from Tap Web Application
	@Override
	void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if(requestCode == 1000) {
        	// Transaction is successful
            if(resultCode == RESULT_OK) {
            	// Retrieve transaction id
                String transactionId = data.getStringExtra(DirectTapSDK.TRANSACTION_ID);
            }

		// Failed transaction
            else {
            	// Retrieve error message
                String error = data.getStringExtra(DirectTapSDK.ERROR);
                // Retrieve error status code
                String error = data.getStringExtra(DirectTapSDK.ERROR_CODE);
            }
        }
    }
```

***NOTE:*** If **showInBrowser** is set to **true**, the transactionId will be returned if bank transfer is successful else it would be null. If it has been set to **false**, the redirect URL for the Tap Web Application will be returned after checkout has been successful.<br/><br/>

The **useRememberMe** in the **checkout** function is set to true by default. To disable the usage of Remember Me inside the Tap Web Application, just pass false to the last parameter in the checkout function

# Statement Tap SDK for Android
***
*Version:* 1.3.0
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

2. In your app build.gradle file, add this line inside the dependencies configuration: **implementation "com.brankas.tap:statement-tap:1.3.0"** to set the SDK as a dependency for the application. This should look like:

	```gradle
	dependencies {
    	implementation "com.brankas.tap:statement-tap:1.3.0"
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

1. Call the initialize function from the TapSDK and pass the context and api key provided by Brankas.

	```java

	import as.brank.sdk.tap.statement.StatementTapSDK;

	StatementTapSDK.INSTANCE.initialize(context, apiKey, null, false);

	```

***NOTE:*** It is better to call the initialize function inside the application class to avoid leaking of context or unexpected crash when other functions from the SDK are called in the background subsequently. Also, to use the **Sandbox** environment, set the optional **isDebug** option to **true**

2. The checkout function can now be called once the initialize function has been called.

## Usage

The SDK has a checkout function wherein it responds with a redirect url used to launch the Tap web application. An option is given either to use the url manually or let the SDK launch it through its internal WebView.

In order to use the checkout function, an **StatementTapRequest** is needed to be created and be passed. It has the following details:

1. **country** - refers to the country of origin of the bank you wanted to do statement retrieval with. There are three countries currently supported: *Philippines (PH)*, *Indonesia (ID)* and *Thailand (TH)*

2. **bankCodes** - refers to the list of banks to be shown within the Tap Web Application. If *null* value is passed, the SDK automatically fills up all the available banks depending on the country passed

3. **externalId** - refers to the identifier passed to track the request

4. **successURL** - refers to the URL where the user will be redirected to after a successful statement retrieval

5. **failURL** - refers to the URL where the user will be redirected to after a failed statement retrieval

6. **organizationName** - refers to the name of the organization that will be displayed while doing statement retrieval

7. **redirectDuration** - refers to the time in seconds when the user should be redirected upon finishing statement retrieval. The default value is *60 seconds*.

8. **showInBrowser** - option to let the SDK create the WebView in behalf of the Android Application. The default value is *true*

9. **dismissalDialog** - pertains to the showing of alert dialog when closing the WebView. It consists of **message**, **positiveButtonText** and **negativeButtonText**. Just set this value to *null** to remove the alert dialog when closing the application.

Here is a sample on how to use it and call:

```java

import as.brank.sdk.tap.statement.StatementTapSDK;
import as.brank.sdk.core.CoreError;
import as.brank.sdk.tap.TapListener;
import tap.common*;

StatementTapRequest.Builder request = new StatementTapRequest.Builder()
            .country(Country.PH)
            .externalId("External ID")
            .successURL("https://google.com")
            .failURL("https://hello.com")
            .organizationName("Organization Name");

StatementTapSDK.INSTANCE.checkout(this, request.build(), new TapListener<String>() {

	@Override
    public void onResult(data: String?, error: CoreError?) {
    	if(data != null) {
		System.out.println("Transaction Successful! Here is the transaction id: " + data);
	}
    }

	@Override
    public void onTapStarted() {

    }

	@Override
    public void onTapEnded() {

    }

}, requestCode, false, true);


@Override
public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
            if(resultCode == RESULT_OK) {
                String statementId = data.getStringExtra(StatementTapSDK.STATEMENT_ID);
                System.out.println("Transaction Successful! Here is the statement id: " + statementId);
            }

            else {
                System.out.println("ERROR: "+ data.getStringExtra(StatementTapSDK.ERROR);
            }
    }
```

***NOTE:*** If **showInBrowser** is set to **true**, the transactionId will be returned if bank transfer is successful else it would be null. If it has been set to **false**, the redirect URL for the Tap Web Application will be returned after checkout has been successful.<br/><br/>
If the internal WebView is opted not to be used (**showInBrowser** is **false**), **do not forget to call *terminate()* function to ensure previous Tap session is closed**

The **isAutoConsent** in the **checkout** function is set to false by default. To enable its usage, just set the 2nd to the last parameter to true

The **useRememberMe** in the **checkout** function is set to true by default. To disable the usage of Remember Me inside the Tap Web Application, just pass false to the last parameter in the checkout function









