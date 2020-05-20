# Web App

## Configuring Authcore

You will need to specify some details about your application that communicate with Authcore. You need the following information:

* **Client ID.** An identifier for the application.
* **Application Name.** The name of the application.
* **Logo URL.** A URL to the application logo.
* **Callback URLs.** URLs where Authcore redirects the user after successful authentication.
* **App URLs.** URLs of your app that use Authcore's Web Widgets.

## Implement login

Choose one of the two methods to add user login to your application.

### OAuth 2.0/OpenID Connect

OAuth 2.0 is a standard that apps use to provide client applications with access. If you would like to add user login to your application quickly without, then you want to use the OAuth 2.0 protocol.

OpenID Connect is an authentication standard built on top of OAuth 2.0. It adds an additional token called an ID token. 

Authcore implements the Authorization Code Flow with Proof Key for Code Exchange \(PKCE\):

* Your application generates a code verifier followed by a code challenge.
* Your application directs the browser to the OAuth 2.0 Authorization endpoint, along with the generated code challenge, and the user authenticates.
* Authcore redirects back to your application with an authorization code.
* Your application sends this code, along with the code verifier, to the OAuth 2.0 Token endpoint. Authcore returns access and ID tokens, and optionally a refresh token.
* Your application can now use these tokens to call the resource server \(for example an API\) on behalf of the user.

{% page-ref page="../api/oauth.md" %}

### Web Widgets

Authcore provides Web Widgets to embed the login UI element to your web application to provide a more customizable user expierence.

{% page-ref page="../sdk/widgets.md" %}



