# react-native-gps

Native GPS location support for React Native for Android and IOS. Was inspired in project of [timfpark](https://github.com/timfpark/react-native-location) and  [syarul](https://github.com/syarul/react-native-android-location)

## Installation
#### Install the npm package
```bash
npm i --save react-native-gps
```

### Add it to your android project

* In `android/settings.gradle`

```gradle
...
include ':RNLocation'
project(':RNLocation').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-gps')
```

* In `android/app/build.gradle`

```gradle
...
dependencies {
    ...
    compile project(':RNLocation')
}
```

* register module (in MainActivity.java)

```java
import com.syarul.rnalocation.RNLocation;  // <--- import

public class MainActivity extends Activity implements DefaultHardwareBackBtnHandler {
  ......

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mReactRootView = new ReactRootView(this);

    mReactInstanceManager = ReactInstanceManager.builder()
      .setApplication(getApplication())
      .setBundleAssetName("index.android.bundle")
      .setJSMainModuleName("index.android")
      .addPackage(new MainReactPackage())
      .addPackage(new RNLocation()) // <-- Register package here
      .setUseDeveloperSupport(BuildConfig.DEBUG)
      .setInitialLifecycleState(LifecycleState.RESUMED)
      .build();

    mReactRootView.startReactApplication(mReactInstanceManager, "example", null);

    setContentView(mReactRootView);
  }

  ......

}
```

#### Add permissions to your Project

Add this to your AndroidManifest file;

``` xml
// file: android/app/src/main/AndroidManifest.xml

<uses-permission android:name="android.permission.ACCESS_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

## Location Usage
```javascript
var React = require('react-native');
var { DeviceEventEmitter } = React;

var { RNLocation: Location } = require('NativeModules');

Location.startUpdatingLocation();

var subscription = DeviceEventEmitter.addListener(
    'locationUpdated',
    (location) => {
        /* Example location returned
        {
          speed: -1,
          longitude: -0.1337,
          latitude: 51.50998,
          accuracy: 5,
          heading: -1,
          altitude: 0,
          altitudeAccuracy: -1
          timestamp: 1446007304457.029
        }
        */
    }
);
```


## Methods

To access the methods, you need import the `react-native-location` module. This is done through `var Beacons = require('react-native-location')`.

### Location.requestWhenInUseAuthorization
```javascript
Location.requestWhenInUseAuthorization();
```

This method should be called before anything else. It requests location updates while the application is open. If the application is in the background, you will not get location updates.

### Location.startUpdatingLocation
```javascript
Location.startUpdatingLocation();
var subscription = DeviceEventEmitter.addListener(
    'locationUpdated',
    (location) => {
        // do something with the location
    }
);
```
## License
MIT, for more information see `LICENSE`
