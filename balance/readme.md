# Balance Tap SDK for Android
***
*Version:* 1.0.0
***


## Table of Contents

  1. [About SDK](#about-sdk)
  2. [Minimum Requirements](#minimum-requirements)
  3. [Installation](#installation)
  4. [Initialization](#initialization)
  5. [Usage](#usage)

***

<a name="about-sdk">
## About SDK
</a>

### What is Balance Tap SDK?
- **Balance Tap SDK** is a development kit used to launch the interface for Tap Web Application via **Balance API** (Application Programming Interface). 
- This kit helps mobile developers to integrate with Brankas Balance API Services with less setup needed and code implementation. 
- With the embedded WebView that is provided within the SDK, users can perform logging in and balance retrieval. 
- The SDK also provides the **BalanceAccount** list object after balance retrieval has been successful or has failed

### Benefits of Using Balance Tap SDK
- **No need to setup HTTPURLConnection or any similar third-party library.**<br/> Everything is already built within the SDK. Just call the appropriate functions and the needed data will be returned
- **No need to create a WebView or launch an external Mobile Web Browser.**<br/>The SDK already provides an embedded WebView wherein built-in functions are done to detect successful or failed transactions
- **The SDK provides freedom and flexibility.**<br/>The developer has the option not to use the embedded WebView and create his own: the checkout URL can be used.<br/>The embedded WebView can be launched via another **Activity** or be embedded inside a **Fragment**
- **The SDK provides convenience.**<br/>The needed API Services are called sequentially and polling of transactions is handled internally. The Balance Account List object will be returned automatically after Tap Web Application Session
- **The SDK provides greater speed.**<br/>The SDK uses gRPC (Remote Procedure Call) mechanism to communicate with the API Services faster. Using gRPC is roughly 7 times faster than REST (Representational State Transfer) when receiving data and roughly 10 times faster when sending data


<a name="minimum-requirements">
## Minimum Requirements
</a>

1. **Android Studio 3.0** but preferably the latest version
2. Minimum Android SDK: **API 21** or **Android 5.0**

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

2. In your app build.gradle file, add this line inside the dependencies configuration: **implementation "com.brankas.tap:balance-tap:1.0.0"** to set the SDK as a dependency for the application. This should look like:

	```gradle
	dependencies {
    	implementation "com.brankas.tap:balance-tap:1.0.0"
	}


3. Inside the the same dependencies configuration, insert the following lines to enable gRPC Connections which are needed by the SDK. Also, include RxJava for asynchronous listening to the results. **Do not forget to include compileOptions and kotlinOptions to use Java 8**

	```gradle
	dependencies {
 		implementation 'com.google.protobuf:protobuf-javalite:3.20.0'
    		implementation 'io.grpc:grpc-okhttp:1.45.1'
    		implementation('io.grpc:grpc-protobuf-lite:1.45.1') {
        			exclude group: 'com.google.protobuf'
    		}
    		implementation 'io.grpc:grpc-stub:1.45.1'
			implementation 'io.reactivex.rxjava3:rxjava:3.0.6'
    		implementation 'io.reactivex.rxjava3:rxandroid:3.0.0'
			//implementation "org.jetbrains.kotlinx:kotlinx-coroutines-rx2:$kotlin_coroutines_version"
			implementation 'org.jetbrains.kotlinx:kotlinx-serialization-json:1.3.2'
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

4. In the plugins section, add these:

	````gradle
plugins {
    	id 'com.android.application'
    	id 'kotlin-android'
    	id 'kotlin-parcelize'
    	id 'org.jetbrains.kotlin.plugin.serialization' version '1.6.10'
}
	````

5. Add the permission **android.permission.INTERNET** in your **AndroidManifest.xml** file to allow your application to access Internet, which is required to use Tap API Services.

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    ```
    
## Initialization

1. Call the initialize function from the BalanceTapSDK and pass the context and api key provided by Brankas.<br/><br/>**Java:**

	```java

	import as.brank.sdk.tap.balance.BalanceTapSDK;

	BalanceTapSDK.INSTANCE.initialize(context, apiKey, null, false);

	```

	**Kotlin:**

	```kotlin

	import `as`.brank.sdk.tap.balance.BalanceTapSDK

	BalanceTapSDK.initialize(context, apiKey, null, false)

	```
***NOTE:*** To use the **Sandbox** environment, set the optional **isDebug** option to **true**

2. The checkout function can now be called once the initialize function has been called.

## Usage

The SDK has a checkout function wherein it responds with a redirect url used to launch the Tap web application. An option is given either to use the url manually (via **retrieveCheckoutURL()** function) or let the SDK launch it through its internal WebView.

In order to use the checkout function, a **BalanceTapRequest** is needed to be created and be passed. It has the following details:

1. **country** - refers to the country of origin of the bank you wanted to do balance retrieval with. There are three countries currently supported: *Philippines (PH)*, *Indonesia (ID)* and *Thailand (TH)*

2. **bankCodes** - refers to the list of banks to be shown within the Tap Web Application. If *null* value is passed, the SDK automatically fills up all the available banks depending on the country passed

3. **externalId** - refers to the identifier passed to track the request

4. **successURL** - refers to the URL where the user will be redirected to after a successful balance retrieval

5. **failURL** - refers to the URL where the user will be redirected to after a failed balance retrieval

6. **organizationName** - refers to the name of the organization that will be displayed while doing balance retrieval

7. **redirectDuration** - refers to the time in seconds when the user should be redirected upon finishing balance retrieval. The default value is *60 seconds*.

8. **dismissalDialog** - pertains to the showing of alert dialog when closing the WebView. It consists of **message**, **positiveButtonText** and **negativeButtonText**. Just set this value to *null** to remove the alert dialog when closing the application.

Here is a sample on how to use it and call:

<br/><br/> **Kotlin:**

```kotlin

import `as`.brank.sdk.tap.balance.BalanceTapSDK
import `as`.brank.sdk.core.CoreError
import `as`.brank.sdk.tap.CoreListener
import tap.model.BankCode
import tap.model.Country
import tap.request.balance.BalanceTapRequest

val request = BalanceTapRequest.Builder()
            .country(Country.PH)
            .externalId("External ID")
            .successURL("https://google.com")
            .failURL("https://hello.com")
            .organizationName("Organization Name")

BalanceTapSDK.checkout(this, request.build(), object: CoreListener<String>() {

        override fun onResult(data: String?, error: CoreError?) {
                println("DATA: "+data)
        }

}, 3000, false, true);

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if(requestCode == 2000) {
            if(resultCode == RESULT_OK) {
                val balanceId = data?.getStringExtra(BalanceTapSDK.BALANCE_ID)
                Toast.makeText(this,
                    "Balance Retrieval Successful! Here is the balance id: $balanceId",
                    Toast.LENGTH_SHORT).show()
            }
            else {
                val error = data?.getStringExtra(BalanceTapSDK.ERROR)
                val errorCode = data?.getStringExtra(BalanceTapSDK.ERROR_CODE)
                Toast.makeText(this, "$error ($errorCode)", Toast.LENGTH_LONG).show()
            }
        }
	}
```

<br/><br/> **Java:**

```java

import as.brank.sdk.tap.balance.BalanceTapSDK;
import as.brank.sdk.core.CoreError;
import as.brank.sdk.tap.CoreListener;
import tap.model.BankCode;
import tap.model.Country;
import tap.request.balance.BalanceTapRequest;

BalanceTapRequest.Builder request = new BalanceTapRequest.Builder()
        .country(Country.PH)
        .externalId("External ID")
        .successURL("https://google.com")
        .failURL("https://hello.com")
        .organizationName("Organization Name");

        BalanceTapSDK.INSTANCE.checkout(this,request.build(),new CoreListener<String>(){
@Override
public void onResult(String data,CoreError error){
        System.out.println("DATA: "+data);
        }

        },3000,false,true);

@Override
public void onActivityResult(int requestCode, int resultCode, Intent data){
        super.onActivityResult(requestCode,resultCode,data);
        if(requestCode == 3000){
        if(resultCode == RESULT_OK){
        String balanceId = data.getStringExtra(BalanceTapSDK.BALANCE_ID);
        Toast.makeText(this,
        "Balance Retrieval Successful! Here is the balance id: "+balanceId,
        Toast.LENGTH_SHORT).show();
        }
        else{
        String error = data.getStringExtra(BalanceTapSDK.ERROR);
        String errorCode = data.getStringExtra(BalanceTapSDK.ERROR_CODE);
        Toast.makeText(this,error+" "+errorCode,Toast.LENGTH_LONG).show();
        }
        }
        }
```


***NOTE:*** 

The **isAutoConsent** in the **checkout** function is set to false by default. To enable its usage, just set the 2nd to the last parameter to true

The **useRememberMe** in the **checkout** function is set to false by default. To enable the usage of Remember Me inside the Tap Web Application, just pass false to the last parameter in the checkout function

The **actionBarText** in the **checkout** function is set to null by default - thus, the ActionBar gets hidden. To show it, just pass a String to it



