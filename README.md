[![Platform](https://img.shields.io/badge/platform-Android-green.svg)](https://survicate.com/mobile-surveys/)

# Mobile SDK - Android

The **Survicate Mobile SDK** for Android allows you to survey specific groups of your mobile app users to understand their needs, expectations, and objections. This SDK is maintained by Survicate.

If you're interested in iOS version go [here](https://github.com/Survicate/survicate-ios-sdk).

## Requirements

SDK works on Android at least on version 4.4.

To use this SDK you need an account at [survicate.com](https://survicate.com).
[Sign up](https://panel.survicate.com/#/signup) for free and find your workspace key in Tracking Code section.

## Installation
Define maven repository in top-level `build.gradle` file:
```groovy
allprojects {
    repositories {
        // ...
        maven { url 'http://repo.survicate.com' }
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

Java:
```java
import android.app.Applicaton;
import com.survicate.surveys.Survicate;

public class MyApp extends Application {
    @Override public void onCreate() {
      super.onCreate();
      Survicate.init(this);
    }
}
```

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

### Events
You can log custom user events throughout your application. They can later be used to trigger the survey. 

Java:
```java
purchaseBtn.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        // ...
        Survicate.invokeEvent("userPressedPurchase");
    }
});
```

Kotlin:
```kotlin
button.setOnClickListener {
    Survicate.invokeEvent("userPressedPurchase")
}
```

### User traits
You can assign custom attributes to your users. Those attributes can later be used to trigger the survey or even filter the survey results within Survicate panel. 

Java:
```java
List<UserTrait> traits = new ArrayList<>();
traits.add(new UserTrait.UserId("someUserId"));
traits.add(new UserTrait.FirstName("John"));
traits.add(new UserTrait("eyes", "blue"));
Survicate.setUserTraits(traits);

// or just
Survicate.setUserTrait(new UserTrait.UserId("someOtherUserId"));
```

Kotlin:
```kotlin
button.setOnClickListener {
    Survicate.invokeEvent("userPressedPurchase")
}
```

Please keep in mind that user traits are cached, you only have to provide them once, e.g. when user logs in, NOT after each init().
You can also change their values at any time (which may potentially trigger showing the survey).

## Customer Support

👋 If you bump into any problems or need more support contact us at hello@survicate.com
