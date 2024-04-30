[![Platform](https://img.shields.io/badge/platform-Android-green.svg)](https://survicate.com/mobile-surveys/)
[![Maven](https://img.shields.io/maven-metadata/v?metadataUrl=http%3A%2F%2Frepo.survicate.com%2Fcom%2Fsurvicate%2Fsurvicate-sdk%2Fmaven-metadata.xml)](https://developers.survicate.com/mobile-sdk/#android)
# Mobile SDK - Android

The **Survicate Mobile SDK** for Android allows you to survey specific groups of your mobile app users to understand their needs, expectations, and objections. This SDK is maintained by Survicate.

Check also the [developer docs](https://developers.survicate.com/mobile-sdk/android/). If you're interested in iOS version go [here](https://github.com/Survicate/survicate-ios-sdk). 

## Requirements

SDK works on Android at least on version 5.

To use this SDK you need an account at [survicate.com](https://survicate.com).
[Sign up](https://panel.survicate.com/#/signup) for free and find your workspace key in Tracking Code section.

## Installation

### Maven installation (recommended)
Define maven repository in top-level `build.gradle` file:
```groovy
allprojects {
    repositories {
        // ...
        maven { url 'https://repo.survicate.com' }
    }
}
```

And add the following dependency to your app's `build.gradle` file: 
```groovy
dependencies {
    // ...
    implementation 'com.survicate:survicate-sdk:1.+'
}
```

*For production environment, it is a good practice to define specific SDK version. Just replace last part of dependency `1.+` with version you want to use.*

### Manual installation

[Download Survicate for Android](https://repo.survicate.com/latest/android/Survicate.aar) and copy it to `app/libs` directory of your project.

Add the following dependencies to your app's `build.gradle` file:
```groovy
dependencies {
    // ...
    implementation files('libs/Survicate.aar')
    implementation "androidx.appcompat:appcompat:1.6.1"
    implementation "androidx.cardview:cardview:1.0.0"
    implementation "androidx.recyclerview:recyclerview:1.3.2"
    implementation "androidx.constraintlayout:constraintlayout:2.1.4"
    implementation "androidx.transition:transition:1.4.1"
    implementation "androidx.annotation:annotation:1.7.1"
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.8.0'
    implementation "com.squareup.moshi:moshi:1.15.1"
    implementation "com.squareup.moshi:moshi-kotlin:1.15.1"

    // since SDK version 2.0.0
    implementation "io.coil-kt:coil-base:2.4.0"
    implementation "io.coil-kt:coil-gif:2.4.0"
    implementation "io.coil-kt:coil-svg:2.4.0"
}
```

Keep in mind that with updating version by manual installation you need to update SDK dependencies versions too.

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

You should initialize the SDK in your application class.

Kotlin:
```kotlin
import android.app.Applicaton
import com.survicate.surveys.Survicate

class MyApp : Application() {
    override fun onCreate() {
      super.onCreate()
      Survicate.init(this)
    }  
}
```

Java:
```java
import android.app.Applicaton;
import com.survicate.surveys.Survicate;

public class MyApp extends Application {
    @Override
    public void onCreate() {
      super.onCreate();
      Survicate.init(this);
    }
}
```

### Displaying Surveys
Survicate gives you the ability to send targeted surveys to your users within your app in a simple, easy, and fast way for you as well as Survicate application users.
Within Survicate Panel you can choose criteria that your users have to meet in order for the surveys to appear in different ways.
The users matching the conditions will see the survey automatically. You can set the criteria to be custom user attributes or user events you created.

Available conditions:
- Screen
- Event
- User attributes
- Language
- Known user
- Operating system

Make sure to list all the screens and events described in your application.
Once you got this covered, you or any person responsible for creating and managing surveys will be able to trigger them from Survicate panel with no need for you to update the application.

### Application screens
A survey can appear when your application user is viewing a specific screen.
As an example, a survey can be triggered to show up on the home screen of the application, after a user spends there more than 10 seconds.
To achieve such effect, you need to send information to Survicate about user entering and leaving a screen. 

Kotlin:
```kotlin
class PurchaseSuccessActivity : Activity() {

    val SCREEN_NAME = "purchaseSuccess"

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

Java:
```java
public class PurchaseSuccessActivity extends Activity {

    public static final String SCREEN_KEY = "purchaseSuccess"

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_survey);
        Survicate.enterScreen(SCREEN_KEY);
    }

    @Override
    protected void onDestroy(){
        super.onDestroy();
        Survicate.leaveScreen(SCREEN_KEY);
    }

}
```

### Events
You can log custom user events throughout your application. They can later be used to trigger the survey. 

Kotlin:
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

Java:
```java
purchaseBtn.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        // event without properties
        Survicate.invokeEvent("userPressedPurchase");

        // event with properties
        Map<String, String> eventProperties = new HashMap<>();
        eventProperties.put("property1", "value1");
        eventProperties.put("property2", "value2");
        Survicate.invokeEvent("userPressedPurchase", eventProperties);
    }
});
```

### User traits
You can assign custom attributes to your users. Those attributes can later be used to trigger the survey or even filter the survey results within Survicate panel. See details in the [docs](https://developers.survicate.com/mobile-sdk/android/).

Kotlin:
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

Java:
```java
// set a single trait
UserTrait trait = new UserTrait("user_id", "YourUserID");
Survicate.setUserTrait(trait);

// or multiple traits at once
UserTrait textTrait = new UserTrait("my_custom_attribute", "some value");
UserTrait numberTrait = new UserTrait("age", 18);
UserTrait booleanTrait = new UserTrait("subscription_active", true);
UserTrait dateTrait = new UserTrait("purchase_date", new Date());

List<UserTrait> traits = Arrays.asList(
    textTrait,
    numberTrait,
    booleanTrait,
    dateTrait
);
Survicate.setUserTraits(traits);
```

Please keep in mind that user traits are cached, you only have to provide them once, e.g. when user logs in, NOT after each init().
You can also change their values at any time (which may potentially trigger showing the survey).

### Reset

If you need to test surveys on your device, `reset()` method might be helpful. This method will reset all user data stored on your device (views, traits, answers).

```kotlin
Survicate.reset()
```

## Customer Support

👋 If you bump into any problems or need more support contact us at hello@survicate.com
