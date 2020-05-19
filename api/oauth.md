# OAuth 2.0 / OpenID Connect

Authcore is complaint to [OAuth 2.0](http://oauth.net/documentation) and [OpenID Connect](http://openid.net/) standards. The OAuth 2.0 protocol provides API access control via scoped access tokens, and OpenID Connect provides user authentication and single sign-on \(SSO\) funcitonality.

## Endpoints

{% api-method method="post" host="https://auth.acme.com" path="/oauth/authorize" %}
{% api-method-summary %}
Authorization
{% endapi-method-summary %}

{% api-method-description %}
This is a starting point for browser-based OpenID Connect flows such as the implicit and authorization code flows. This request authenticates the user and returns tokens along with an authorization grant to the client application as a part of the callback response.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="state" type="string" required=true %}
A value to be returned in the token. The client application can use it to remember the state of its interaction with the end user at the time of the authentication call.
{% endapi-method-parameter %}

{% api-method-parameter name="scope" type="string" required=true %}
Currently empty.
{% endapi-method-parameter %}

{% api-method-parameter name="response\_type" type="string" required=true %}
Any combination of `code`, `token`, and `id_token`.
{% endapi-method-parameter %}

{% api-method-parameter name="redirect\_uri" type="string" required=true %}
Callback location where the authorization code or tokens should be sent. It must match the preset value in client application configuration.
{% endapi-method-parameter %}

{% api-method-parameter name="code\_challenge\_method" type="string" required=false %}
Method used to derive the code challenge for PKCE. Valid value: S256
{% endapi-method-parameter %}

{% api-method-parameter name="code\_challenge" type="string" required=false %}
A challenge for PKCE. The challenge is verified in the access token request.
{% endapi-method-parameter %}

{% api-method-parameter name="client\_id" type="string" required=true %}
The ID in client application configuration.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://auth.acme.com" path="/oauth/token" %}
{% api-method-summary %}
Token
{% endapi-method-summary %}

{% api-method-description %}
This endpoint returns access tokens, ID tokens, and refresh tokens, depending on the request parameters. For password, client credentials, and refresh token flows, calling `/token` is the only step of the flow. For the authorization code flow, calling `/token` is the second step of the flow.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="code" type="string" required=false %}
Required if grant\_type is authorization\_code. The value is what was returned from the authorization endpoint.
{% endapi-method-parameter %}

{% api-method-parameter name="code\_verifier" type="string" required=false %}
Required if grant\_type is authorization\_code and code\_challenge was specified in the original authorization request. This value is the code verifier for PKCE.
{% endapi-method-parameter %}

{% api-method-parameter name="grant\_type" type="string" required=true %}
Can be one of the following: authorization\_code, or refresh\_token.
{% endapi-method-parameter %}

{% api-method-parameter name="redirect\_uri" type="string" required=false %}
Required if grant\_type is authorization\_code. Specifies the callback location where the authorization was sent.
{% endapi-method-parameter %}

{% api-method-parameter name="refresh\_token" type="string" required=false %}
Required if grant\_type is refresh\_token.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "access_token": "access_token",
    "token_type": "token_type",
    "expires_in": 987654321,
    "scope": "",
    "refresh_token": "",
    "id_token": "id_token"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://auth.acme.com" path="/.well-known/openid-configuration" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}
Returns OpenID Connect metadata about your authorization server.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "issuer": "https://auth.acme.com",
  "authorization_endpoint": "https://auth.acme.com/oauth/authorize",
  "token_endpoint": "https://auth.acme.com/oauth/token",
  "jwks_uri": "https://auth.acme.com/.well-known/jwks.json",
  "response_types_supported": [
    "code",
    "code id_token",
    "id_token",
    "token id_token"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://auth.acme.com" path="/.well-known/jwks.json" %}
{% api-method-summary %}
JSON Web Key Set
{% endapi-method-summary %}

{% api-method-description %}
Returns a JSON Web Key Set \(JWKS\) that contains the public keys that can be used to verify the signatures of tokens that you receive from your authorization server.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "keys": [
    {
      "use": "sig",
      "kty": "EC",
      "kid": "hN351AH04N2BBba3N6PgNcVloRohu6KkDRDMcvr5k28",
      "crv": "P-256",
      "alg": "ES256",
      "x": "KY6MShC7UrSkekyczKKvZQXuxFKDRd0DEgV6r9XeDAY",
      "y": "aGDz074Md6DQU2rRSY0jif6kawW6r22Q4jzi6Se75Wk"
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



