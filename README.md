# Geddit Commerce SDK

* [Get Started](#get-started)
	* [Installation](#installation)
		* [Gradle](#gradle)
	* [Initialize Commerce SDK](#initialize-commerce-sdk)
	* [Display Live Stream](#display-live-stream)
	* [Close Live Stream](#close-live-stream)
	* [Picture In Picture](#picture-in-picture)
		* [Expand To Go Back To Live Stream View](#expand-to-go-back-to-live-stream-view)
		* [Enter PIP View](#enter-pip-view)
* [Delegate](#delegate)
	* [Product View Delegate](#product-view-delegate)
	* [Picture In Picture Delegate](#picture-in-picture-delegate)
	* [Voucher Delegate](#voucher-delegate)
* [Dependencies](#dependencies)
	* [3rd party](#3rd-party)
* [Requirements](#requirements)
* [Release Process](#release)

## Get Started

### Installation

#### Gradle

Add the following dependency to your `app.gradle` file:

```kotlin
implementation 'com.geddit.live.sdk:core:1.0.14'(Version may vary)
implementation 'com.geddit.live.sdk:commerce:1.0.21'(Version may vary)
```

### Initialize Commerce SDK

```kotlin
val sdk = GedditLiveCommerce.Builder(
    context,
    "12359616-26f7-4b21-9e26-2ce766d5c6ed",
    "ba822b68b152f015730380224c525021",
    "12345",
    "TestUser",
    "THB",
    object: GedditLiveCommerce.GedditLiveCommerceListener {
        override fun onCollectionImageClicked(context: Context, product: Product) {
            
        }
        override fun onCollectionAddToBagClicked(context: Context, product: Product) {
            
        }
        override fun onReady(helper:  GedditLiveCommerce.GedditLiveCommerceActionListener) {
            
        }
    },
    object: VoucherDelegate {
        override fun didWinVoucher(voucher: Voucher) {
            // Called when user won a voucher.
        }
    },
    object : QuizDelegate {
        override fun didAnswerCorrectlyQuestion(question: String, answer: String) {
            // Called when user answered correctly.

        }
        override fun didAnswerIncorrectlyQuestion(question: String, answer: String) {
            // Called when user answered incorrectly.
        }
    }
    )
    .email("test@gmail.com")
    .gender(Gender.MALE)
    .mobileNumber("+66612345678")
    .languageCode("en")
    .fcmToken("")
    .debuggable(BuildConfig.DEBUG)
    
GedditLiveCommerce.with(sdk.build())
```

### Display Live Stream

```kotlin
GedditLiveCommerce.with(sdk.build())
```

### Close Live Stream

```kotlin
GedditHelper.helper?.dismissLiveStream()
```

### Picture In Picture

#### Expand To Go Back To Live Stream View

```kotlin
GedditHelper.helper?.exitPIP()
```

#### Enter PIP View

```kotlin
GedditHelper.helper?.enterPIP()
```

## Delegate

### Product View Delegate

`GedditLiveCommerceListener` allows you to create your custom product view and manage its life cycle.

```kotlin
{
    override fun onCollectionImageClicked(context: Context, product: Product) {
        
    }
    override fun onCollectionAddToBagClicked(context: Context, product: Product) {
        
    }
    override fun onReady(helper: GedditLiveCommerceActionListener) {
        
    }
}
```

You can initialize the SDK with your product delegate implementation.

```kotlin
    object: GedditLiveCommerce.GedditLiveCommerceListener {
        override fun onCollectionImageClicked(context: Context, product: Product) {
            
        }
        override fun onCollectionAddToBagClicked(context: Context, product: Product) {
            
        }
        override fun onReady(helper:  GedditLiveCommerce.GedditLiveCommerceActionListener) {
            
        }
    }
```

### Picture In Picture Delegate

`GedditLiveCommerceActionListener` allows you to control when picture in picture view is
closed or tapped.

```kotlin
{
     override fun enterPIP() {
     
     }
     override fun exitPIP() {
     
     }
     override fun exitPIP(product: Product) {
     
     }
}
```

You can call  PIP delegate implementation.

```kotlin
GedditLiveCommerce.helper?.enterPIP()
```

By default clicking on PIP brings you to the live stream and closing the PIP causes closing 
SDK. If you want to change this behaviour, please conform to 
`GedditLiveCommerceActionListener`.

### Voucher Delegate

`GedditCommerceVoucherDelegate` allows you to get called back when user won a voucher.

```kotlin
{
    override fun didWinVoucher(voucher: Voucher) {
        // Called when user won a voucher.
    }
}
```

You can initialize SDK with your voucher delegate implementation.

```kotlin
 object: VoucherDelegate {
        override fun didWinVoucher(voucher: Voucher) {
            // Called when user won a voucher.
        }
    }
```

## Dependencies

We make heavy use of the following projects, and so it can be helpful to be 
familiar with them:

### 3rd Party

* [`rajawali`](https://github.com/Rajawali/Rajawali/): Rajawali is a 3D engine for Android based on OpenGL ES 2.0/3.0. It can be used for normal apps as well as live wallpapers.
* [`Konfetti`](https://github.com/DanielMartinus/Konfetti): Celebrate more with this lightweight confetti particle system.
* [`Glide`](https://github.com/bumptech/glide): Glide is a fast and efficient open source media management and image loading framework for Android that wraps media decoding, memory and disk caching, and resource pooling into a simple and easy to use interface.
* [`Retrofit`](https://github.com/square/retrofit): A type-safe HTTP client for Android and Java.
* [`OkHttp`](https://github.com/square/okhttp): OkHttp is an HTTP client that’s efficient by default.
* [`Koin`](https://github.com/InsertKoinIO/koin): A pragmatic lightweight dependency injection framework for Kotlin developers.
* [`RxJava`](https://github.com/ReactiveX/RxJava): RxJava is a Java VM implementation of Reactive Extensions: a library for composing asynchronous and event-based programs by using observable sequences.
* [`Socket.IO`](https://github.com/socketio/socket.io-client-java): This is the Socket.IO Client Library for Java, which is simply ported from the JavaScript client.
* [`logger`](https://github.com/orhanobut/logger): Simple, pretty and powerful logger for android.
* [`AnimationEasingFunctions`](https://github.com/daimajia/AnimationEasingFunctions): This project is originally from my another project, AndroidViewAnimation, which is an animation collection, to help you make animation easier.
* [`AndroidViewAnimations`](https://github.com/daimajia/AndroidViewAnimations): Android basic animations.
* [`Shimmer`](https://github.com/facebook/shimmer-android): Shimmer is an Android library that provides an easy way to add a shimmer effect to any view in your Android app.
* [`Push Down Animation Click`](https://github.com/TheKhaeng/pushdown-anim-click): A library for Android developers who want to create "push down animation click" for view like spotify application.

## Requirements

* Android Studio 4.0.1+
* Gradle 6.1.1+
* Kotlin 1.3.50+.
