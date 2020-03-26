# Server-to-server Authentication

Instead of obtaining access token using the OAuth 2.0 flow, you can call Management API using a signed JWT directly as a bearer token, . You can avoid having to make a network request before making an API call. To do so:

1. Generate an EC private key, of size 256, and output it to a file named `key.pem`:

   ```text
   $ openssl ecparam -name prime256v1 -genkey -noout -out key.pem
   ```

2. Extract the public key from the key pair:

   ```text
   $ openssl ec -in key.pem -pubout -out public.pem
   ```

3. Update Authcore config file, set `service_account_public_key` to the public key and `service_account_id` to the ID of the user that act as a service account.
4. Using any standard JWT library, such as one found at jwt.io, create a JWT with ES256 algorithm and payload like the following example:

   ```javascript
   {
   "iss": "serviceaccount:<service_account_id>",
   "iat": 1511900000,
   "exp": 1511903600
   }
   ```

   Sign the JWT with `prime256v1` using the above private key.

   For example \(Javascript\):

   ```javascript
   var jwt = require('jsonwebtoken')
   var opts = {algorithm: "ES256", issuer: "serviceaccount:1", expiresIn: 60}
   var token = jwt.sign({}, privateKeyPEM, opts)
   ```

5. Call the API, using the signed JWT as the bearer token:

   ```http
   GET /api/management/users HTTP/1.1
   Host: testing.authcore.io
   Authorization: Bearer <SIGNED_JWT>
   Content-Type: application/json
   ```

