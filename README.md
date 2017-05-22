## Issues and Solutions after upgrading Gradle to 4.0-milestone and Gradle plugin to 3.0.0.-alpha1



### unmock plugin cannot find Android jar.

Upgrade to version 0.6.1 of umock plugin.

See https://github.com/bjoernQ/unmock-plugin/issues/31



### CircleCI cannot find Play Services, Support Library and Contraint Layout

Grab them manually thru android tools.

```
echo y | android update sdk --no-ui --all --filter "tools,extra-android-m2repository,extra-android-support,extra-google-google_play_services,extra-google-m2repository"
```
The `android` tool does not have access to ConstrainsLayout package but `sdkmanager` does.

```
echo y | /usr/local/android-sdk-linux/tools/bin/sdkmanager "extras;m2repository;com;android;support;constraint;constraint-layout;1.0.2"
```


### Apollo Android Library: class file for com.apollographql.apollo.api.Operation not found

https://github.com/apollographql/apollo-android/

Our shared module has the generated GraphQL classes. However, when you using GraphQL in the our app module, we are getting the error. The solution was to apply the plugin to the `build.gradle` file of the app module as well.

```
app/build.gradle

apply plugin: 'com.android.application'
apply plugin: 'com.apollographql.android'
```


### Error with Autovalue Annotations

Add workaround to android > default config in the app's `build.gradle`

```
android {
    ...
    defaultConfig {
        minSdkVersion minSdkVersionValue
        targetSdkVersion compileSdkVersionValue
        versionCode 1
        versionName "1.0"
        multiDexEnabled true

        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"

        javaCompileOptions {
            annotationProcessorOptions {
                includeCompileClasspath false
            }
        }
    }
    ...
}
```

