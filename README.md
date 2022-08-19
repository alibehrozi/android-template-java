# Android template
An Android Studio templates for Android development.

Getting Started
---------------

##### Fetch code

```
git clone git clone https://github.com/alibehrozi/android-template-java.git
```

##### setup keystore
1. Copy your release.keystore into app/config
2. Fill out `RELEASE_KEY_PASSWORD`, `RELEASE_KEY_ALIAS`, `RELEASE_STORE_PASSWORD` in `gradle.properties` to access your  `release.keystore`

>**[gradle.properties](gradle.properties) :**
>```
>RELEASE_KEY_PASSWORD=YOUR_ANDROID_KEY
>RELEASE_KEY_ALIAS=YOUR_KEY_ALIAS
>RELEASE_STORE_PASSWORD=YOUR_PASSWORD
>```

##### Build project

4. Open the project in the Studio (note that it should be opened, NOT imported).
5. You are ready to compile project.


Implementations
---------------
##### Dependencies

>[Leakcanary](https://square.github.io/leakcanary/) is a memory leak detection library for Android.Memory leaks are very common in Android apps and the accumulation of small memory leaks causes apps to run out of memory and crash with an OOM. LeakCanary will help you find and fix these memory leaks during development. When Square engineers first enabled LeakCanary in the Square Point Of Sale app, they were able to fix several leaks and reduced the OOM crash rate by 94%.

```
debugImplementation 'com.squareup.leakcanary:leakcanary-android:2.9.1'
```

#### Signing Configs
>Android requires that all APKs be digitally signed with a certificate before they are installed on a device or updated.

So you need to sign your app for more info see more ([here](https://developer.android.com/studio/publish/app-signing)).

>**[build.gradle](app/build.gradle) :**
```
signingConfigs {
    debug {
        storeFile file("config/release.keystore")
        storePassword RELEASE_STORE_PASSWORD
        keyAlias RELEASE_KEY_ALIAS
        keyPassword RELEASE_KEY_PASSWORD
    }

    release {
        storeFile file("config/release.keystore")
        storePassword RELEASE_STORE_PASSWORD
        keyAlias RELEASE_KEY_ALIAS
        keyPassword RELEASE_KEY_PASSWORD
    }
}
```

#### BuildTypes

* `release` and `debug` buildTypes implemented for production and debug output

>**[build.gradle](app/build.gradle) :**
```
buildTypes {
    debug {
        debuggable true
        jniDebuggable true
        signingConfig signingConfigs.debug
        minifyEnabled false
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
    }
    release {
        debuggable false
        jniDebuggable false
        signingConfig signingConfigs.release
        minifyEnabled true
        shrinkResources true
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
    }
}
```

#### Restrict installation directories

One of the biggest problems that Android has faced in the last few years is not having enough memory to install a large number of applications. That was mostly due to the low storage capacity of the devices, but as technology has advanced and phones have become somewhat cheaper, most devices now have plenty of storage space for a large number of apps. However, to reduce insufficient memory, Android allows you to install apps on external storage. This method worked very well, but over the years, many security concerns have been raised about this approach. Installing apps on external SD cards is a cool way to conserve storage space, but it's a security flaw because anyone with access to the SD card has access to app data. And this data can hold sensitive information. This is why it is recommended to limit your app installation to internal storage.


**NOTE :** you have 3 choice  `auto`, `internalOnly` and `preferExternal`. chose by your project type.

To do this, open the `AndroidManifest.xml` file and add the following:

```
android:installLocation="internalOnly"
```

>**[AndroidManifest.xml](app/src/main/AndroidManifest.xml):**
```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.template"
    android:installLocation="internalOnly">
    .....
    .....
</manifest>
```


#### Disable backups

After previous step, the install location on the device is limited, but you can still backup the app and its data. This means that users can access the contents of the app's private data folder using adb backup. To disallow backups, find the line that reads `android:allowBackup=”true”` and replace the value with `“false”`.


#### Multi-window support

It's important that you always use this element in your application to specify the screen sizes your application supports.(readmore [here](https://developer.android.com/guide/topics/manifest/supports-screens-element)).


>**[AndroidManifest.xml](app/src/main/AndroidManifest.xml):**
```
<manifest...>
    
    <supports-screens
        android:anyDensity="true"
        android:largeScreens="true"
        android:normalScreens="true"
        android:resizeable="true"
        android:smallScreens="true"
        android:xlargeScreens="true" />
    .....
    .....
    <application...>
        ....
        ....
        <uses-library
            android:name="com.sec.android.app.multiwindow"
            android:required="false" />
        <meta-data
            android:name="com.sec.android.support.multiwindow"
            android:value="true" />
        <meta-data
            android:name="com.sec.android.multiwindow.DEFAULT_SIZE_W"
            android:value="632dp" />
        <meta-data
            android:name="com.sec.android.multiwindow.DEFAULT_SIZE_H"
            android:value="598dp" />
        <meta-data
            android:name="com.sec.android.multiwindow.MINIMUM_SIZE_W"
            android:value="632dp" />
        <meta-data
            android:name="com.sec.an*droid.multiwindow.MINIMUM_SIZE_H"
            android:value="598dp" />
    </application>
</manifest>
```

Support
-------

* **LinkedIn :** [https://www.linkedin.com/in/alibehrozi/](https://www.linkedin.com/in/alibehrozi/).

If you've found an error in this sample, please file an issue: https://github.com/alibehrozi/android-template-java/issues