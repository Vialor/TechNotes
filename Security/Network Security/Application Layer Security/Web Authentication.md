>local authentication: Authentication in app itself (Basic & Token-Based)
>social authentication: Third Party Authentication
## Basic Authentication (stateless):
> Standard RFC 2617: all private requests are sent with a username and password

- Security notes on storing and verifying password:
    **Clear**: Once the server is hacked, it is leaked.
    **Encrypted**: Need a key to decrypt. Once the key is compromised, passwords will be leaked.
    **Hash**: Weak passwords are vulnerable to known hash in a **Rainbow Table**.
    **Salted Hash**: Still use Hash, but add some value (a salt) to a password to strengthen it.
	    Salt and Hash must be stored in the database.
	    express middleware `bcrypt`
	    Store password in headers or body, but NOT in url. urls are saved in browser logs, so it can be accessed by browsers and proxies.
## Token-Based
### Session Authentication (stateful, managed and stored on the server side)
> Standard RFC 6265: servers generate session ids, stored them in the database (until log out) and set them to the user cookies; all private requests are sent with a session id in cookies

A session id is sent in the cookies which is in the headers.

`req.session.userId =` when log in with `express-session`, or you need to generate and set the session id in cookies yourself
`req.session.destroy()` when log out, or you need to remove the session id in cookies yourself
### JWT: JSON Web Token (stateless, managed and stored on the client side)
> JWT uses the algorithm, HMAC Hash based message authentication code, which is defined in the document Standard RFC 2104

A JWT consists of:
	*header*: token type, algorithm for signing and encoding
	*payload*: session metadata
	*signature*: encoding the header and payload using Base64url Encoding and concatenating them with a period separator, and then running a cryptographic algorithm on the result
A JWT is sent in the request headers: `Authorization: Bearer <your_jwt_token>`

```JavaScript
// Backend sign-in handler
jwt.sign(username, secret);
// then put the returned token in Cookies
// Authentication checking middleware
jwt.verify(cookies.token, secret);
```
- Pros:
    Can authenticate without saving in the database and hence more convenient for micro-services
- Cons
    Revoking tokens (completely logging out users) can be complicated. Users can always login in as long as they have a copy of cookies.
## IdP (Third Party Authentication)
**IdP** Identity provider (Authorization Server): Google, Facebook…
**SSO** Single-Sign-On, can use protocols such as: OpenID, OAuth, SAML…
- How
    Prerequisite: Backend needs a credential file from the IdP, and need to set up a registry there.
    Users log in in the third party website
    Third party sends the SSO token to backend, and backend verifies it
`passport` middleware