Loyalty Coin [![Language](https://img.shields.io/badge/Kotlin-1.3-%234c20f0.svg)]() [![Release](https://img.shields.io/badge/Release-0.1.0-%234c20f0.svg)]()
=====
![PhunCoin logo](https://s3-us-west-1.amazonaws.com/honeybadger.phunware.com/phuncoin.png)

`Loyalty Coin` is an iOS and Android SDK that allows you to reward your users digital currency that can be exchanged for digital goods in your own Loyalty Coin powered ecosystem.

> ***<sub>note</sub>*** <br/>
>  `Loyalty Coin SDK` currently only supports Android for  **SDK 21**+. 
  
<a id="installation"></a>
## Using Loyalty Coin SDK

### **Gradle**

If using Gradle, add the following to your build.gradle:

```gradle
maven {
    url "https://nexus.phunware.com/content/groups/public"
}
```

and the following to your app/build.gradle

```gradle
implementation "com.phunware.crypto:loyalty:0.1.0"
```

***
<a id="usage-overview"></a>
## Usage Overview

### **Initialization**
The Loyalty Coin SDK is initialized along with your current [MaaS SDK integration](https://github.com/phunware/maas-core-android-sdk).  That being said, the important integration steps are setting up MaaS Core with relevant appId, accessKey, and secret.

***
### **Opt In**
Loyalty Coin is opt in by default and based around your own system of registered users.  To opt your user into Loyalty Coin you will need to provide the SDK with a unique identifier for the user that maps cleanly to the representation of the user in your own system (such as the email they used to register with your app). It is expected that you will track the opt in state in your own app.


```kotlin
Loyalty.linkManager().linkUser("joe@phunware.com", object : LinkManager.LinkResultListener {
                override fun onSuccess() {
                    Log("Success!")
                }

                override fun onFailure(message: String) {
                    Log("Failure")
                }
            })
```

***
### **Checking the users balance**

Phunware will maintain a pending balance for your previously opted in users on our servers.  You can request this balance at any time (after opt in).

The actual user balance will be returned along with the provided image and metadata you have previously setup in the portal for your loyalty coin.

```kotlin
Loyalty.assetManager().balance(object : AssetManager.BalanceListener {
                override fun onSuccess(asset: Asset) {
                    Log("Balance is ${asset.balance}")
                }

                override fun onFailure(message: String) {
                    Log("Failed to get balance")
                }
            })
```

***
### **Events**
Instrumented Events can be used to reward your user with new Loyalty Coins or to deduct from their balance to unlock digital content.

Your instrumented events must be setup a head of time on the portal, along with any additional thresholds and limits.


```kotlin
// Share a recipe.  This earns the user two Dessert Coins.
Loyalty.assetManager().fireEvent(
    eventName = "share_recipe",
    parameters = listOf("donut", "eclair"),
    callback = object : AssetManager.FireEventListener {
        override fun onSuccess(sideEffects: List<SideEffect>) {
            // List of SideEffects will have displayable values
            // setup on the portal.
            Log("Success!")
        }

        override fun onFailure(message: String) {
           Log("Failure")
        }
    }
)
```

***
### **Linking to PhunCoin**
You have the option of letting your users link their Loyalty Coin balance to the Phunware PhunCoin app.  If you have enabled conversion of your loytalty coin into PhunCoin, your users will be able to convert at the threshold you specify.

```kotlin
Loyalty.linkManager().linkWallet(
    object : LinkManager.LinkResultListener {
        override fun onSuccess() {
            Log("PhunCoin app installed and linking is in progress.")
        }

        override fun onFailure(message: String) {
            Log("PhunCoin app not installed or other failure.")
        }
    })
```

***
<a id="class"></a>
## Class Reference Documentation
The [Reference Documentation](https://phunware.github.io/maas-loyalty-android-sdk/index.html) has all of the detailed usage information including all the public methods, parameters, and convenience initializers.

***
<a id="attribution"></a>
## Attribution

PhunCoin SDK uses the following 3rd party components.

| Component     | Version  | Description   | License  |
| ------------- | -------  |:-------------:| -----:|
| [OkHttp](https://github.com/square/okhttp) |3.12.1| An HTTP & SPDY client for Android and Java applications. | [Apache 2.0]
| [Gson](https://github.com/google/gson)  2.8.5| Gson is a Java library that can be used to convert Java Objects into their JSON representation. | [Apache 2.0]
| [Picasso](https://github.com/square/picasso) |2.71828| A powerful image downloading and caching library for Android. | [Apache 2.0]
| [Timber](https://github.com/JakeWharton/timber) |4.7.1| A logger with a small, extensible API which provides utility on top of Android's normal Log class. | [Apache 2.0]

***
<a id="privacy"></a>
## Privacy
You understand and consent to Phunware’s Privacy Policy located at www.phunware.com/privacy. If your use of Phunware’s software requires a Privacy Policy of your own, you also agree to include the terms of Phunware’s Privacy Policy in your Privacy Policy to your end users.
***
<a id="terms"></a>
## Terms
Use of this software requires review and acceptance of our terms and conditions for developer use located at http://www.phunware.com/terms/
