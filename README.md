[![Platform](https://img.shields.io/badge/platform-Android-green.svg)](https://survicate.com/mobile-surveys/)

# Mobile SDK - Android

The Survicate Mobile SDK for Android allows you to survey specific groups of your mobile app users to understand their needs, expectations, and objections. This SDK is maintained by Survicate. If you're interested in iOS version go [here](https://github.com/Survicate/survicate-ios-sdk).

## Requirements

SDK works on Android at least on version 4.4.

## Installation
Declare maven repository:
```groovy
repositories {
    // ...
    maven { url 'http://repo.survicate.com' }
}
```

And add the following dependency to your app's `build.gradle` file: 
```groovy
dependencies {
    // ...
    implementation 'com.survicate:survicate-sdk:1.+'
}
```

## Setup
To use this SDK you need an account at [survicate.com](https://survicate.com).
[Sign up](https://panel.survicate.com/#/signup) for free and find your workspace key in Tracking Code section.
Add workspace key to your `AndroidManifest.xml` file:
```xml
<application>
    <!-- ... -->
    <meta-data android:name="com.survicate.surveys.workspaceKey" android:value="YOUR_WORKSPACE_KEY"/>
</application>
```

You should initialize the SDK in your application class.
```java
public class App extends Application {
  @Override public void onCreate() {
    Survicate.init(this, false);
  }
}   
```

If you need to debug your surveys in application, enable logger by changing second parameter to `true`:
```java
Survicate.init(this, true);
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
```java
purchaseBtn.setOnClickListener((view -> {
    ...
    Survicate.invokeEvent("userPressedPurchase");
}));
```

### User traits
You can assign custom attributes to your users. Those attributes can later be used to trigger the survey or even filter the survey results within Survicate panel. 
```java
List<UserTrait> traits = new ArrayList<>(
  UserId("someUserId"),
  UserTrait("eyes", "blue")
)

Survicate.setUserTraits(traits)
// or just
Survicate.setUserTrait(UserId("someOtherUserId"))
```

Please keep in mind that user traits are cached, you only have to provide them once, e.g. when user logs in, NOT after each init().
You can also change their values at any time (which may potentially trigger showing the survey).

## Customer Support

👋 If you bump into any problems or need more support contact us at hello@survicate.com
