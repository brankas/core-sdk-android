# Direct Tap SDK for Android
***
*Version:* 4.2.1
***

### What is Direct Tap SDK?
- **Direct Tap SDK** is a development kit used to launch the interface for Tap Web Application via **Direct API** (Application Programming Interface). 
- This kit helps mobile developers to integrate with Brankas Direct API Services with less setup needed and code implementation. 
- With the embedded WebView that is provided within the SDK, users can perform logging in and bank transfers. 
- The SDK also provides the **Transaction** object after bank transfer has been successful or has failed

### Benefits of Using Direct Tap SDK
- **No need to setup HTTPURLConnection or any similar third-party library.**<br/> Everything is already built within the SDK. Just call the appropriate functions and the needed data will be returned
- **No need to create a WebView or launch an external Mobile Web Browser.**<br/>The SDK already provides an embedded WebView wherein built-in functions are done to detect successful or failed transactions
- **The SDK provides freedom and flexibility.**<br/>The developer has the option not to use the embedded WebView and create his own: the checkout URL can be used.<br/>The embedded WebView can be launched via another **Activity** or be embedded inside a **Fragment**
- **The SDK provides convenience.**<br/>The needed API Services are called sequentially and polling of transactions is handled internally. The transaction object will be returned automatically after Tap Web Application Session
- **The SDK provides greater speed.**<br/>The SDK uses gRPC (Remote Procedure Call) mechanism to communicate with the API Services faster. Using gRPC is roughly 7 times faster than REST (Representational State Transfer) when receiving data and roughly 10 times faster when sending data)
 

# Statement Tap SDK for Android
***
*Version:* 4.2.1
***

### What is Statement Tap SDK?
- **Statement Tap SDK** is a development kit used to launch the interface for Tap Web Application via **Statement API** (Application Programming Interface). 
- This kit helps mobile developers to integrate with Brankas Statement API Services with less setup needed and code implementation. 
- With the embedded WebView that is provided within the SDK, users can perform logging in and transaction history retrieval. 
- The SDK also provides the **Transaction** list object after statement retrieval has been initiated
- The SDK also gives an option to download the transaction history in CSV format

### Benefits of Using Statement Tap SDK
- **No need to setup HTTPURLConnection or any similar third-party library.**<br/> Everything is already built within the SDK. Just call the appropriate functions and the needed data will be returned
- **No need to create a WebView or launch an external Mobile Web Browser.**<br/>The SDK already provides an embedded WebView wherein built-in functions are done to detect successful or failed transactions
- **The SDK provides freedom and flexibility.**<br/>The developer has the option not to use the embedded WebView and create his own: the checkout URL can be used.<br/>The embedded WebView can be launched via another **Activity** or be embedded inside a **Fragment**
- **The SDK provides convenience.**<br/>The needed API Services are called sequentially and polling of transactions is handled internally. The transaction list object will be returned automatically after Tap Web Application Session
- **The SDK provides greater speed.**<br/>The SDK uses gRPC (Remote Procedure Call) mechanism to communicate with the API Services faster. Using gRPC is roughly 7 times faster than REST (Representational State Transfer) when receiving data and roughly 10 times faster when sending data)

