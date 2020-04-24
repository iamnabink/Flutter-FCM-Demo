# flutter notification

A demo Flutter application for Firebase Cloud Messaging 

## Steps to integrate FCM in flutter in (2020)

1. Create flutter project
2. Create project in firebase console.
...To get sh1 key go to java/jdk/bin & run this cmnd in cmd:
... `keytool -list -v -keystore "%USERPROFILE%\.android\debug.keystore" -alias androiddebugkey storepass android -keypass android`
)
[or check this answer for easier way to get SH1](https://stackoverflow.com/a/54342861/12030116)

3. In android/app -> add google-service.json
4. In `pubspec.yaml`
...add,
```dev_dependencies:
   flutter_test:
     sdk: flutter
   firebase_messaging: ^6.0.13
   firebase_analytics: ^5.0.11
  ```


5. add Application.kt in android/app/src/main/kotlin/com/yourdomain/appname/

`Application.kt` (ignore the error)

```
import io.flutter.app.FlutterApplication
import io.flutter.plugin.common.PluginRegistry
import io.flutter.plugin.common.PluginRegistry.PluginRegistrantCallback
import io.flutter.plugins.GeneratedPluginRegistrant
import io.flutter.plugins.firebasemessaging.FlutterFirebaseMessagingService
//import com.google.firebase.messaging.FirebaseMessagingService

class Application : FlutterApplication() , PluginRegistrantCallback {

    override fun onCreate() {
        super.onCreate();
        FlutterFirebaseMessagingService.setPluginRegistrant(this);
    }

    override fun registerWith( registry: PluginRegistry) {
        GeneratedPluginRegistrant.registerWith(registry);
    }
}
```

..if error -> replace
    ```override fun registerWith(registry: PluginRegistry?) {
        registry?.registrarFor("io.flutter.plugins.firebasemessaging.FirebaseMessagingPlugin");
    }```
6. In src/main/manifest
...add,
  1 -->  <application
        android:name=".Application"

  2-->    
     <activity        
     <intent-filter>
             <action android:name="FLUTTER_NOTIFICATION_CLICK" />
             <category android:name="android.intent.category.DEFAULT" />
      </intent-filter>

7. In `app/build.gradle`
...add,
    `implementation 'com.google.firebase:firebase-messaging:20.1.5'`
`apply plugin: 'com.google.gms.google-services'` addd this at the bottom 

8. In `android/build.gradle`
..add,
    ```dependencies {
        classpath 'com.google.gms:google-services:4.3.3'
        ```

9. To get token and suscribe to a topic use following code in your `main.dart`:
`main.dart`

```
import 'package:firebase_messaging/firebase_messaging.dart';
 final FirebaseMessaging _messaging = FirebaseMessaging();
  @override
  void initState() {
    super.initState();
    _messaging.subscribeToTopic("general");
    _messaging.getToken().then((token) {
      print(token);
    });
  } 
  ```

10. Invalidate cache and restart android studio
11. run the application
12. enjoy.

[For more help read documentation](https://pub.dev/packages/firebase_messaging)
