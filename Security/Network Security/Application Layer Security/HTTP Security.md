## TLS Transport Layer Security
**Confidentiality & Integrity** Issue: Attackers can eavesdrop http requests and responses or even worse tamper with them by impersonation.
 Solution:
	 **Confidentiality**: end-to-end secure channel
	 **Integrity**: authentication handshake

**HTTPS = HTTP + TLS/SSL**
**FTPS = FTP + TLS**
> SSL (Secure Sockets Layer) is the predecessor of TLS (Transport Layer Security)

TLS/SSL handshakes happen after TCP connection and use asymmetric encryption to set up conversation and session key. Later conversation will use the session key for symmetric encryption.
SSL Certificate from CA (certificate authority): Contains the public key and information of the server

**CA Certificate Authority**: a trusted third party that holds the public keys of the websites. After websites receive digital certificates issued by CA, browsers can extract public keys from the certificates to verify a website when visiting it.
## Frontend Vulnerability
**Incomplete Mediation**: the mistake to trust sensitive data from the front end or do sensitive operation in the frontend
**SQL Injection**: attackers can inject SQL/NoSQL code inside HTTP requests to be executed by the database
**CSRF Cross-Site Request Forgery**: exploit a site’s trust in the browser and submit requests using the authenticated session (Cookie for example)
**XSS Cross-Site Scripting**: attackers can inject arbitrary JS code inside the web page to be executed by the browsers
That means the attackers can inject illegitimate content (content spoofing), perform illegitimate HTTP requests (CSRF), access user cookies, and steal user password by modifying the page to forge a scam.
**XSS worm**: injected JS code can spread on social networks
- Variations
    **Reflected XSS**: injected JS code is immediately sent back from backend and inserted into the DOM
    **Stored XSS**: injected JS code is stored in database and later-on sent back from backend to be inserted into the DOM
    **DOM-based XSS**: attack payload is executed as a result of modifying the DOM “environment” in the victim’s browser used by the original client side script, so that the client side code runs in an “unexpected” manner (like inject JS code in a URL link)
- Lessons:
    Don’t trust the frontend for any sensitive data or operations
    Be careful of data inserted into DOM
    Sanitize data from frontend
    Authentication Cookies should have `Secure` , `HttpOnly` and `SameSite` flags
### Same-Origin Policy
> Motivation: we don’t want just any website can just access our backend
> Used with `SameSite` flag or CSRF tokens
- **Same-Origin Policy**: a policy of browser to prevent scripts from reading responses from a different origin.
    origin: scheme + host+ port
    elements covered by Same Origin Policy: Ajax requests, Form actions(still submitted, just no response)
    elements NOT covered by Same Origin Policy: JS scripts, CSS, images/videos/sound, Plugins
    does NOT block sending anything to backend by default
- **Relax the Same-Origin Policy**
    `iframe`: usually for advertisements
    JSONP
    **CORS** **Cross-Origin Resource Sharing:** a common way to relax Same-Origin Policy
	    backend set a header: `Access-Control-Allow-Origin: https://example.com`