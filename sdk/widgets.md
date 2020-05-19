# Web Widgets

Web Widgets are embeddable UI elements for common user management interactions including register, sign in, user settings and account recovery. The widgets are configurable to your needs.

{% hint style="info" %}
Widgets are embedded elements that use iframes. Your app is required to add `frame-src` exception in content security policy if the domain of Authcore is not on the same origin domain.
{% endhint %}

## Installation

{% hint style="warning" %}
The packages will be published to npm when v1.0 is released
{% endhint %}

Install via npm:

```bash
$ npm install authcore-js
```

Install via yarn:

```bash
$ yarn add authcore-js
```

## Usage

### User Registration

```markup
<!-- Element for the widget to be injected -->
<div id="authcore-register-container"></div>

<script>
let privateStore = {};

// Instantiate Register widget
new AuthCoreWidgets.Login({
    container: 'authcore-register-container', // Id of the HTML element for the widget
    root: 'https://authcore.example.com/widgets', // Authcore is hosted on the sub-domain of example.com,
    initialScreen: 'register', // Set initial screen to register
    logo: 'default', // Default to show Authcore logo
    company: 'Authcore',
    successRedirectUrl: 'https://application.example.com',
    analyticsHook: (type, data) => {
        // type is a string describing the event. It is contained in the event list. All Authcore events are typed with prefix `Authcore_`.
        // data is a object which is the associated data of an event. By default, if no data is associated, it will be a empty object `{}`.

        // code for analytics
    }
});
</script>
```

### User Profile

```markup
<!-- Element for the widget to be injected -->
<div id="authcore-profile-container"></div>

<script>
// Instantiate Profile widget
new AuthCoreWidgets.Profile({
    container: 'authcore-profile-container', // Id of the HTML element for the widget
    root: 'https://authcore.example.com/widgets', // Authcore is hosted on the sub-domain of example.com
    accessToken: 'ACCESS_TOKEN_TO_BE_PROVIDED',
    callbacks: {
        // Handle the unauthenticated case returns from the widget
        unauthenticated: () => {};
    }
});
</script>
```

## API

### Constructor Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| container | REQUIRED, string | The `id` of the HTML element where the widget will be rendered. |
| root | OPTIONAL, string | The domain where the Authcore widgets hosts, if it is not set it will refer to the window origin location with `/widgets`. |
| logo | OPTIONAL, string | The logo shown in the widget. If no value is passed this section is hidden. Should be in absolute path format, also accept value "default" to serve as showcase of the widget style for `Login` widget. |
| company | OPTIONAL, string | The company name shown in the widget. If no value is passed this section is hidden. |
| accessToken | REQUIRED except for `Login` or `RefreshToken`widget, string | The access token for API request requires authentication. This is done by using authorization code returned with `successRedirectUrl`from Login widget to get access token from [create access token API](https://blocksq.gitlab.io/authcore/authapi.html#operation/CreateAccessToken). |
| primaryColour | OPTIONAL, string | The primary colour of the widget. Primary colour mainly consists of general button colour, link colour and border colour when the field box is in focus. Allow colour code, RGB colour value or named colour. |
| successColour | OPTIONAL, string | The success colour of the widget. Success colour mainly consists of verified message and icon. Allow colour code, RGB colour value or named colour. |
| dangerColour | OPTIONAL, string | The danger colour of the widget. Danger colour mainly consists of error message, button colour for destructive action \(e.g. Remove contact\) and invalid field box border. Allow colour code, RGB colour value or named colour. |
| requireUsername | OPTIONAL, boolean | The flag whether username is included in registration and sign in. |
| language | OPTIONAL, string | Widget language when it is loaded, default to be English when it is not set or the value is invalid or unavailable. |
| onSuccess | OPTIONAL, function | Callback hook after the certain action from widgets is success. Return the result including `action` as key in object format, use destructing assignment to get the data required. |
| onLoaded | OPTIONAL, function | Callback after the widgets is loaded and mounted to the site. |
| analyticsHook | OPTIONAL, function | Hooks for analytics to recieve events. Two parameters describing the event are given for the hook, including `type` and `data`. |
| unauthenticated | OPTIONAL, function | Callback when the widget returns unauthenticated error. |

Action list for `onSuccess` callback:

{% hint style="warning" %}
Success callback is now DEPRECATED, use successRedirectUrl for registration or sign in flow.
{% endhint %}

### Login Widgets

These widgets provides a simple UI widget for authenticating or registering users.

#### AuthCoreWidgets.Login

For register page, the field with `Email or mobile` label refer as contact field. For sign in page, the input field refer as handle field.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| successRedirectUrl | REQUIRED, string | The URL to be redirected with authorization code when registration or sign in flow is success. |
| contact | OPTIONAL, string | The pre-fill contact\(Register screen\) or handle field value for Signin widget. |
| fixedContact | OPTIONAL, boolean | The flag indicates contact\(Register screen\) or handle field is fixed or not. If it is set to be fixed value should be provided on `contact` param, otherwise error will be throwed as it is impossible for user to sign in. |
| initialScreen | OPTIONAL, string | The flag indicates the initial screen, either `register` or `signin`, default to be `signin`. |
| socialLoginPaneStyle | OPTIONAL, string | The flag to decide position where social login pane should located, either `top`or `bottom`, default to be `bottom`. |
| socialLoginPaneOption | OPTIONAL, string | The flag to decide option where social login pane in grid or list stye, either `grid` or `list`, default to be `grid`. |
| buttonSize | OPTIONAL, string | The flag to decide button size, either `normal` or `large`, default to be `large`. |

For `register` as initialScreen:

![design-register](https://blocksq.gitlab.io/authcore-docs/docs/assets/register.png)

When registration is completed, the screen will show loading spinner and redirect to `successRedirectUrl` set in widget instance.

For `signin` as initialScreen:

![design-signin](https://blocksq.gitlab.io/authcore-docs/docs/assets/signin.png)

Social login pane list:

![list-social-login](https://blocksq.gitlab.io/authcore-docs/docs/assets/signin-list-social-login-pane.png)

Normal button size:

![normal-button-size](https://blocksq.gitlab.io/authcore-docs/docs/assets/signin-normal-button.png)

### User Widgets

Authcore provides some user widgets for updating a logged in user. These widgets require the `accessToken` parameter, which can be obtained from the `access_token` parameter from `onSuccess` callback in login widgets.

#### AuthCoreWidgets.Profile

| Parameter | Type | Description |
| :--- | :--- | :--- |
| showAvatar | OPTIONAL, boolean | The flag indicates to show avatar/monogram in profile widget, default to be `false`. |

Show and change the profile of the current user.

![design-profile](https://blocksq.gitlab.io/authcore-docs/docs/assets/profile.png)

#### AuthCoreWidgets.Settings

Show the settings of current login user, including password, 2-steps verification, devices \(i.e. sessions\) and social login sections.

![design-setting](https://blocksq.gitlab.io/authcore-docs/docs/assets/settings.png)

### Others

#### AuthCoreWidgets.RefreshToken

Using new `AuthCoreWidgets.RefreshToken(..)` to handle refresh access token by hidden iframe element. Callbacks from `onTokenUpdated` to renew accessToken. Callbacks from`onTokenUpdatedFail`will be triggered if the update is failed.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| onTokenUpdated | OPTIONAL, function | Callback after the token is updated successfully. |
| onTokenUpdatedFail | OPTIONAL, function | Callback after the token update is failed. |



## Analytics

We provide multiple events for analytics purpose. All events shown below are prefixed with `Authcore_`.

The events are show below:

| Type | Data \(if any\) | Description |
| :--- | :--- | :--- |
| loginWidgetLoaded | - | When a login widget \(e.g. register / login\) page is loaded |
| registerStarted | contactType: "email" / "phone" | When a user triggered register |
| oauthStarted | service: "google" / "facebook" / "matters" / "twitter" / "apple" | When a user triggered oauth from social platform |
| loginStarted | method: "password" | When a user login using password |
| navigation | from/to: Page name\("Register"/"SignIn"\) | When a user switch between register/sign in page within Login widget |

Because of page reload in redirection, the widget cannot emit loginSuccess and registerSuccess events to the original page. Therefore, it is advised to record the success events manually in the redirected pages \(from `successRedirectUrl`\), and pages after signin.



