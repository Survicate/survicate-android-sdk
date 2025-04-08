[![Platform](https://img.shields.io/badge/platform-Android-green.svg)](https://survicate.com/mobile-surveys/)
[![Maven](https://img.shields.io/maven-metadata/v?metadataUrl=http%3A%2F%2Frepo.survicate.com%2Fcom%2Fsurvicate%2Fsurvicate-sdk%2Fmaven-metadata.xml)](https://developers.survicate.com/mobile-sdk/#android)
# Mobile SDK - Android

The **Survicate Mobile SDK** for Android allows you to survey specific groups of your mobile app users to understand their needs, expectations, and objections. This SDK is actively maintained by [Survicate](https://survicate.com/software/mobile-app-surveys/).

For detailed guidance, refer to the [developer docs](https://developers.survicate.com/mobile-sdk/android/). If you're looking for the iOS version, visit [here](https://github.com/Survicate/survicate-ios-sdk). 

## Requirements

- Minimum SDK version: 21+
- Compile SDK version: 34+

To use this SDK, you need an account at [survicate.com](https://survicate.com).
[Sign up](https://panel.survicate.com/#/signup) for free and locate your workspace key in the Settings.

## Installation

Define Survicate's Maven repository in the top-level `build.gradle` file:
```groovy
allprojects {
    repositories {
        // ...
        maven { url 'https://repo.survicate.com' }
    }
}
```

or in the `settings.gradle`

```groovy
dependencyResolutionManagement {
    // ...
    repositories {
        // ...
        maven { url 'https://repo.survicate.com' }
    }
}
```

Then add the SDK dependency to your app's `build.gradle` file: 
```groovy
dependencies {
    // ...
    implementation 'com.survicate:survicate-sdk:{version}'
}
```

For other installation methods visit our [developer docs](https://developers.survicate.com/mobile-sdk/android/).

## Setup
Add workspace key to your `AndroidManifest.xml` file:
```xml
<application
    android:name=".MyApp"
>
    <!-- ... -->
    <meta-data android:name="com.survicate.surveys.workspaceKey" android:value="YOUR_WORKSPACE_KEY"/>
</application>
```

Initialize the SDK in the Application class:

```kotlin
import android.app.Application
import com.survicate.surveys.Survicate

class MyApp : Application() {
    override fun onCreate() {
      super.onCreate()
      Survicate.init(this)
    }  
}
```

### Displaying Surveys
Survicate lets you easily display targeted surveys to users directly within your app. In the Survicate Panel you can choose criteria that your users have to meet in order for the surveys to appear.

Available conditions:
- Screen
- Event
- User attributes
- Device language
- Operating system
- Screen orientation

### Application screens
A survey can appear when your application user is viewing a specific screen. As an example, a survey can be triggered to show up on the home screen of the application, after a user spends more than 10 seconds there.
To enable this behavior, you need to notify the SDK when the user enters and leaves a screen, and set up a screen trigger with delay in the Survicate Panel.

```kotlin
class MainActivity : Activity() {

    val SCREEN_NAME = "home"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // ...
        Survicate.enterScreen(SCREEN_NAME)
    }

    override fun onDestroy() {
        super.onDestroy()
        Survicate.leaveScreen(SCREEN_NAME)        
    }
}
```

In Jetpack Compose

```kotlin
const val SCREEN_NAME = "home"

@Composable
fun HomeScreen() {
    DisposableEffect(Unit) {
        Survicate.enterScreen(SCREEN_NAME)
        onDispose {
            Survicate.leaveScreen(SCREEN_NAME)
        }
    }
}
```

### Events
Log custom events to display surveys with a matching Event Trigger. Events can be invoked with or without additional properties:

```kotlin
button.setOnClickListener {
    // event without properties
    Survicate.invokeEvent("userPressedPurchase")

    // event with properties
    val eventProperties = mapOf(
        "property1" to "value1",
        "property2" to "value2"
    )
    Survicate.invokeEvent("userPressedPurchase", eventProperties)
}
```

### User traits
Assign custom attributes to users. These can be used to trigger surveys based on your Audience settings or to filter answers in the Survicate Panel.

```kotlin
// set a single trait
val trait = UserTrait("user_id", "YourUserID")
Survicate.setUserTrait(trait)

// or multiple traits at once
val textTrait = UserTrait("my_custom_attribute", "some value")
val numberTrait = UserTrait("age", 18)
val booleanTrait =  UserTrait("subscription_active", true)
val dateTrait = UserTrait("purchase_date", Date())

val traits = listOf(
    textTrait,
    numberTrait,
    booleanTrait,
    dateTrait
)
Survicate.setUserTraits(traits)
```

Traits are cached in `SharedPreferences`, so you don't need to provide them on every app start. For example, set the `user_id` once at login rather than on each initialization. 
You can also update traits at any time, which could trigger a survey.

### Listeners
Register a `SurvicateEventListener` to receive callbacks for the following events:
- Survey displayed
- Question answered
- Survey closed
- Survey completed

```kotlin
val listener = object : SurvicateEventListener() {
    override fun onSurveyDisplayed(event: SurveyDisplayedEvent) {
        // ...
    }
    override fun onQuestionAnswered(event: QuestionAnsweredEvent) {
        // ...
    }
    override fun onSurveyClosed(event: SurveyClosedEvent) {
        // ...
    }
    override fun onSurveyCompleted(event: SurveyCompletedEvent) {
        // ...
    }
}

// Register the listener (e.g. in Activity's onCreate method)
Survicate.addEventListener(listener)

// Remove the listener when no longer needed (e.g. in onDestroy)
Survicate.removeEventListener(listener) 
```

### Reset
If you need to test surveys on your device, the `reset()` method might be helpful. This method will clear all user data stored on the device (views, traits, answers).

```kotlin
Survicate.reset()
```

## Customer Support

👋 If you bump into any problems or need more support, contact us at hello@survicate.com
