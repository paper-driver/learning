# Security
## JWT
it is an encoded string that can contain unlimited amount of data.
It is cryptographically signed.

There is no middleman so server can trust it.

However, JWT does not guarantee the encryption although
it is good at guarantee data ownership. Anyone who can intercept
the token.

JWT is a good technology for API authentication and server to server authorization.
It is a not a good choice for sessions.

### Usage
Basically when you create your service, you register your service
to an API dashboard to get a secret token.
This token will be used to sign your jwt token created on the client side.

### Components
JWT is composed of a header, payload, and signature.

![img_2.png](img_2.png)

If the JWT header is a JWS header, the claims are signed and if the JWT header is a JWE header, the claims are encrypted.

![img_3.png](img_3.png)

### Summary
JWT is useful when it comes to its stateless characteristics.
It is designed to be used to ensure the authenticity of the source, and one
token can be used in different applications.

However, the token cannot be revoked before its expiry time. Regardless you changed
your password and id, the token that has already been generated will still be valid.
Even you deleted token in browser, the token itself is still valid until its expiry time.

## Authentication vs Authorization
### Authentication
is the process of verifying the identity of a user by obtaining some sort of credentials for example his username password combination.

### Authorization
is the process of allowing an authenticated user to access his resources by checking whether the user has access rights to the system.
You can control access rights by granting or denying specific permissions to an authenticated user.

### OAuth
OAuth is not an API or a service. it is an open standard for authorization.

It is a standard to securely access stuff with randomized tokens.

OAuth (Open Authorization) is an open standard for token-based authentication and authorization which is used to provide single sign-on (SSO). OAuth allows an end user's account information to be used by third-party services, such as Facebook, without exposing the user's password.

#### OAuth Grant Types
##### Authorization code grant
##### implicit grant
##### Resource owner credentials grant
##### Client Credentials grant
##### Refresh token grant

You will have two tokens when you use OAuth.
* access token
* refresh token

Access token is used to validate your identity to access some content.
Refresh token is used to renew your access token when your token expired.

![img_4.png](img_4.png)

## JWT vs OAuth
JWT is just a token format. OAuth is a protocol.
In other words, OAuth can use JWT as a token to authorize clients.

## Single Sign-On (SSO)
SSO is not a protocol. It is more of a high-level conceppt used by a wide range of service providers.
SSO is an authentication / authorization flow through which a user can log into multiple services using the same credentials.
