# Statement Tap SDK for Android
***
*Version:* 1.4.0
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

2. In your app build.gradle file, add this line inside the dependencies configuration: **implementation "com.brankas.tap:statement-tap:1.4.0"** to set the SDK as a dependency for the application. This should look like:

	```gradle
	dependencies {
    	implementation "com.brankas.tap:statement-tap:1.4.0"
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

1. Call the initialize function from the DirectTapSDK and pass the context and api key provided by Brankas.<br/><br/>**Java:**

	```java

	import as.brank.sdk.tap.direct.StatementTapSDK;

	StatementTapSDK.INSTANCE.initialize(context, apiKey, null, false);

	```

	**Kotlin:**

	```kotlin

	import `as`.brank.sdk.tap.direct.StatementTapSDK

	StatementTapSDK.initialize(context, apiKey, null, false)

	```
***NOTE:*** To use the **Sandbox** environment, set the optional **isDebug** option to **true**

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
<br/><br/> **Kotlin:**

```kotlin

import `as`.brank.sdk.tap.statement.StatementTapSDK
import `as`.brank.sdk.core.CoreError
import `as`.brank.sdk.tap.CoreListener
import tap.common.BankCode
import tap.common.Country
import tap.common.DismissalDialog
import tap.statement.StatementTapRequest

val request = StatementTapRequest.Builder()
            .country(Country.PH)
            .externalId("External ID")
            .successURL("https://google.com")
            .failURL("https://hello.com")
            .organizationName("Organization Name")

StatementTapSDK.checkout(this, request.build(), object: CoreListener<String>() {

        override fun onResult(data: String?, error: CoreError?) {
                println("DATA: "+data)
        }

}, 3000, false, true);

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if(requestCode == 2000) {
            if(resultCode == RESULT_OK) {
                val statementId = data?.getStringExtra(StatementTapSDK.STATEMENT_ID)
                Toast.makeText(this,
                    "Statement Retrieval Successful! Here is the statement id: $statementId",
                    Toast.LENGTH_SHORT).show()
            }
            else {
                val error = data?.getStringExtra(StatementTapSDK.ERROR)
                val errorCode = data?.getStringExtra(StatementTapSDK.ERROR_CODE)
                Toast.makeText(this, "$error ($errorCode)", Toast.LENGTH_LONG).show()
            }
        }
	}
```

<br/><br/> **Kotlin:**

```kotlin

import `as`.brank.sdk.tap.statement.StatementTapSDK
import `as`.brank.sdk.core.CoreError
import `as`.brank.sdk.tap.CoreListener
import tap.common.BankCode
import tap.common.Country
import tap.statement.StatementTapRequest

val request = StatementTapRequest.Builder()
            .country(Country.PH)
            .externalId("External ID")
            .successURL("https://google.com")
            .failURL("https://hello.com")
            .organizationName("Organization Name")

StatementTapSDK.checkout(this, request.build(), object: CoreListener<String>() {

        override fun onResult(data: String?, error: CoreError?) {
                println("DATA: "+data)
        }

}, 3000, false, true);

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if(requestCode == 2000) {
            if(resultCode == RESULT_OK) {
                val statementId = data?.getStringExtra(StatementTapSDK.STATEMENT_ID)
                Toast.makeText(this,
                    "Statement Retrieval Successful! Here is the statement id: $statementId",
                    Toast.LENGTH_SHORT).show()
            }
            else {
                val error = data?.getStringExtra(StatementTapSDK.ERROR)
                val errorCode = data?.getStringExtra(StatementTapSDK.ERROR_CODE)
                Toast.makeText(this, "$error ($errorCode)", Toast.LENGTH_LONG).show()
            }
        }
	}
```

<br/><br/> **Java:**

```java

import as.brank.sdk.tap.statement.StatementTapSDK;
import as.brank.sdk.core.CoreError;
import as.brank.sdk.tap.CoreListener;
import tap.common.BankCode;
import tap.common.Country;
import tap.statement.StatementTapRequest;

StatementTapRequest.Builder request = new StatementTapRequest.Builder()
	.country(Country.PH)
     	.externalId("External ID")
        .successURL("https://google.com")
        .failURL("https://hello.com")
        .organizationName("Organization Name");

StatementTapSDK.INSTANCE.checkout(this, request.build(), new CoreListener<String>() {
	@Override
        public void onResult(String data, CoreError error) {
                System.out.println("DATA: "+data);
        }

}, 3000, false, true);

@Override
public void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if(requestCode == 3000) {
            if(resultCode == RESULT_OK) {
                String statementId = data.getStringExtra(StatementTapSDK.STATEMENT_ID);
                Toast.makeText(this,
                    "Statement Retrieval Successful! Here is the statement id: "+statementId,
                    Toast.LENGTH_SHORT).show();
            }
            else {
                String error = data.getStringExtra(StatementTapSDK.ERROR);
                String errorCode = data.getStringExtra(StatementTapSDK.ERROR_CODE);
                Toast.makeText(this, error + " " +errorCode, Toast.LENGTH_LONG).show();
            }
        }
	}
```


***NOTE:*** If **showInBrowser** is set to **true**, the transactionId will be returned if bank transfer is successful else it would be null. If it has been set to **false**, the redirect URL for the Tap Web Application will be returned after checkout has been successful.<br/><br/>
If the internal WebView is opted not to be used (**showInBrowser** is **false**), **do not forget to call *terminate()* function to ensure previous Tap session is closed**

The **isAutoConsent** in the **checkout** function is set to false by default. To enable its usage, just set the 2nd to the last parameter to true

The **useRememberMe** in the **checkout** function is set to true by default. To disable the usage of Remember Me inside the Tap Web Application, just pass false to the last parameter in the checkout function




