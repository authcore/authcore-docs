---
description: All notable changes to this project will be documented in this file.
---

# Release Notes

### Unreleased

#### Added

* API 2.0
* Pull user information from social login platform

### [0.3.1](https://gitlab.com/blocksq/authcore/-/tags/v0.3.1) - 2020-03-14

#### Added

-Provide option for changing logo and company name in React Native widget \(\#845\)

#### Changed

* Fix layout issues in sign in widget in small screens
* Fix layout issues in list style social login buttons

### [0.3](https://gitlab.com/blocksq/authcore/-/tags/v0.3) - 2020-03-13

#### Added

* New and cleaner widget style
* List style social login buttons
* Ability to separate configurations for multiple apps \(\#60\)
* Automate first time deployment \(\#186\)
* Add navigation event for analytics \(\#820\)

#### Changed

* Improve widget loading speed by 7 folds \(\#218\)
* Remove screen clutters in registration flow \(\#809\)
* Simplify email/phone mobile; users now have one email and phone only \(\#642\)
* Remove an extra step to confirm email when registering with social login accounts
* Optimize loading spinner style \(\#663\)
* Fix occasional widget white screen during deployment \(\#770\)
* Fix User Update API returning 400 if no changes was made \(\#650\)
* Change to allow display\_name to be empty instead of fallback to email address \(authcore-js\#69\)
* Simplify production service configuration:
  * Use server to serve static web assets in production
  * Remove the use of Nginx for web and widgets

### 0.2.6 - 2020-03-12

#### Changed

* Add ingress caching to avoid white screen issue during deployment

### 0.2.5 - 2020-02-20

#### Changed

* Add content-type and cache-control header to swagger file endpoints

### 0.2.4 - 2020-01-29

#### Changed

* Fix redundant escaping of redirection URL

### 0.2.3 - 2020-01-29

#### Changed

* Fix unable to complete registration using oauth

### 0.2.2 - 2020-01-21

#### Changed

* Fix an old version of swagger.json being cached by browsers

### 0.2.1 - 2020-01-21

#### Changed

* Fix a bug that cause login with React Native widget to fail due to unhandled postMessage

### 0.2 - 2020-01-21

#### Added

* Add filtering and sorting ability to user list \(\#586\)
* Add analytics events hook \(\#380\)
* Add social login buttons in create user widget \(\#719\)
* Add ability to edit email and SMS template to Management Web UI \(\#587\)
* Use ID Token in Matters OAuth authentication \(\#687\)
* Add an option to prevent users from unlinking Matters account binding \(\#668\)

#### Changed

* Do not autofocus input fields inside sign-in widget on mobile browser
* Fix TOTP QR became invalid in some cases \(\#726\)
* Fix not able to complete registration if user use social login and opt out providing email address \(\#661\)
* Improve the layout of sign in and create user widgets \(multiple issues\)
* Improve loading performance in widgets \(multiple issues\)

### 0.1.3 - 2019-12-24

#### Changed

* Fix a potential security issue in email parameters processing

### 0.1.2 - 2019-12-24

#### Changed

* Fix email sender not shown when using SES email delegate \(\#657\)

### 0.1.1 - 2019-12-19

#### Added

* Support using AWS SES to deliver transactional emails \(\#584\)

#### Changed

* Fix phone number validation rejecting numbers outside Hong Kong \(\#639\)
* Fix various functional issues related to email and phone input field



