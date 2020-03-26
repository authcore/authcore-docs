---
description: >-
  This guide shows how to use Authcore for in mobile application using React
  Native framework.
---

# React Native Widgets

## Overview

The module provides an in-app browser to sign in through Authcore in return of the access token, which can be used for accessing information or secret stored in Authcore.

## Installation

{% hint style="warning" %}
Not published to npm until 1.0
{% endhint %}

~~Install via npm:~~

```text
$ sh npm install react-native-authcore
```

~~Install via yarn:~~

```bash
$ sh yarn add react-native-authcore
```

Install package directly from Gitlab repository:

```bash
yarn add https://gitlab.com/blocksq/react-native-authcore
```

## Set Up

### Link to native module

To add the functionality of the React Native Authcore module to your project you need to link it.

```bash
react-native link react-native-authcore
```

### Configure server Callback URLs

A callback URL is a URL in your application where Authcore redirects the user back to the app after they have sign in.

#### Android Callback

```text
{YOUR_APP_PACKAGE_NAME}://
```

You can find this at the top of your `AndroidManifest.xml` file located in the `android/app/src/main/` folder.

Below is the example of that, in this case package name is `com.authcoresamples`.

```markup
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.authcoresamples"
```

#### iOS Callback

```text
{PRODUCT_BUNDLE_IDENTIFIER}://
```

In latest iOS app `PRODUCT_BUNDLE_IDENTIFIER` can be found in `project.pbxproj`, whereas `PRODUCT_NAME` should be the project name

> Current workaround: provide the callback link to add to whitelist manuelly

### Integrate Authcore to your Application

#### Configure Android

In the file `android/app/src/main/AndroidManifest.xml` you must make sure the `MainActivity` of the app has a launchMode value of `singleTask` and that it has the following intent filter:

```markup
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data
        android:host=""
        android:scheme="${applicationId}" />
</intent-filter>
```

`MainActivity` should look like this:

```markup
<activity
    android:name=".MainActivity"
    android:label="@string/app_name"
    android:launchMode="singleTask"
    android:configChanges="keyboard|keyboardHidden|orientation|screenSize"
    android:windowSoftInputMode="adjustResize">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
    <!-- General deep link or app link used in your application -->
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data
            android:host="example.com"
            android:scheme="https" />
    <!-- Exclusive for Authcore -->
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data
            android:host=""
            android:scheme="${applicationId}" />
    </intent-filter>
</activity>
```

The rule to interpret deep link in `intent-filter` is to include all combinations of their combined attributes from `<data>` tag. To ensure starting Authcore for sign in without showing app picker, use a separated `intent-filter` for the app link. Check [https://developer.android.com/training/app-links/deep-linking\#adding-filters](https://developer.android.com/training/app-links/deep-linking#adding-filters) for more details about deep link in Android.

#### Configure iOS

In the file `ios/<YOUR PROJECT>/AppDelegate.m`, add the following:

```objectivec
#import <React/RCTLinkingManager.h>

- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication annotation:(id)annotation
{
  return [RCTLinkingManager application:application openURL:url
                      sourceApplication:sourceApplication annotation:annotation];
}
```

Next you will need to add a URLScheme using your App's bundle identifier to allow redirection back to the app.

In the file `ios/<YOUR PROJECT>/Info.plist`, locate the value of CFBundleIdentifier. In latest version for iOS app, this should be `$(PRODUCT_BUNDLE_IDENTIFIER)`

```markup
<key>CFBundleIdentifier</key>
<string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
```

and then register a URL type entry using the value of `CFBundleIdentifier` as the value for the `CFBundleURLSchemes`

```markup
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>None</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER).authcore</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        </array>
    </dict>
</array>
```

Update the podfile used in iOS, in `/ios`, run the following:

```bash
pod install
```

## Sign In

First import the `Authcore` module and create a new `Authcore` instance.

```javascript
import Authcore from 'react-native-authcore'
const authcore = new Authcore({
    baseUrl: 'https://authcore.example.com', // Authcore is hosted on the sub-domain of example.com
    socialLoginPaneStyle: 'bottom', // Location for social login pane, either `top` or `bottom`, default to be `bottom`
    language: 'en' // Language for sign in page. Currently allow `en` or `zh-hk` only
})
```

Then presented the hosted login screen:

```javascript
authcore
    .webAuth
    .signin()
    .then(credentials => {
        console.log(credentials)
        // Show the credentials return from signin method, should return Object like the following
        // {
        //    accessToken: '', // Access token
        //    refreshToken: '',  // Refresh token
        //    idToken: '',  // ID token in JWT format.
        //    currentUser: {}  // User information, check `GetCurrentUser` from API docs for the format.
        // }

        // Store the access token to a secure storage for further usage
    })
    .catch(error => console.log(error))
```

### Storing the access token

Please be reminded that access token should be stored in secure storage. Developers are adviced to have such solution to store the access token returned from the sign in page.

## Settings Widget

The module also provides widgets for setting the user profile and security settings.

Widgets require Authcore instance setting for initialization. In common case the instance will be in store like `redux` using state management. To use widget inside the page, use the component from the instance.

For user profile, use component `ProfileScreen`.

For settings widget it is shown by an in-app browser. To instantiate it, run `settings.show` function. See the example below for detail.

## Parameters

| Prop | Type | Description |
| :--- | :--- | :--- |
| accessToken | REQUIRED, string | The access token for getting the user information. This is required for the widgets. Passing invalid access token will still show the widget but not showing correct information. |
| company | OPTIONAL, string | The company name shown in the widget. |
| logo | OPTIONAL, string | The logo shown in the widget. Should be in absolute path format. |
| primaryColour | OPTIONAL, string | The primary colour of the widget. Primary colour mainly consists of general button colour, link colour and border colour when the field box is in focus. Allow colour code, rgb colour value or named colour. |
| successColour | OPTIONAL, string | The success colour of the widget. Success colour mainly consists of verified message and icon. Allow colour code, rgb colour value or named colour. |
| dangerColour | OPTIONAL, string | The danger colour of the widget. Danger colour mainly consists of error message, button colour for destructive action \(e.g. Remove contact\) and invalid field box border. Allow colour code, rgb colour value or named colour. |
| socialLoginPaneStyle | OPTIONAL, string | The flag to decide position where social login pane should located, either `top` or `bottom`, default to be `bottom`. |

```javascript
// Assume Authcore instance is instantiated and named as `authcore`. Check sign in section to see how to create new instance.
// Assume state contains access token named as `accessToken`, access token should be stored in secure storage

render () {
  return <authcore.ProfileScreen accessToken={ this.state.accessToken } />
}
```

Example for settings widget

```javascript
// Component in react-native

// Function for the component

// _showSettings execute function in Authcore and shows Settings widget in an in-app browser.
_showSettings = async () => {
  this.state.authcore.settings.show({
    // Assume state contains access token named as `accessToken`, access token should be stored in secure storage
    accessToken: this.state.accessToken,
    // Optional parameters to set the widget layout
    primaryColour: '#66ff00',
    company: 'LikeCoin'
  })
}

render() {
  return (
    <View style={styles.container}>
      // Click the button and shows the settings widget in in-app browser
      <Button onPress = { this._showSettings } title={ 'Show settings widget' } />
    </View>
  );
}
```

### Styling

It is possible to constraint the widget widget and height to fit the app design, to do that apply `containerStyle` with `maxWidget` or `maxHeight` on the widget.

```javascript
render () {
  return <authcore.ProfileScreen accessToken={ this.state.accessToken } containerStyle={{ maxHeight: 300 }} />
}\
```

## Sign out using access token

> TODO: Handle sign out case with direct API access

