> Motivation: Performance and Data Persistence

|                                 |                    |               |                 |
| ------------------------------- | ------------------ | ------------- | --------------- |
|                                 | Cookies            | Local Storage | Session Storage |
| Capacity                        | 4kb                | 10mb          | 5mb             |
| Expiration                      | Manually set       | On tab close  | Never           |
| Accessible From                 | Any window         | Any window    | Same tab        |
| Storage Location                | Browser and server | Browser only  | Browser only    |
| Sent with requests              | Yes                | No            | No              |
| Editable and Blockable by users | Yes                | Yes           | Yes             |
Others: **CacheStorage**, **IndexedDB Storage**
## Cookies and Sessions

> Motivation: preserve the application's state between different requests the browser makes.
- **Cookies**: key/value pairs sent back and forth in HTTP requests and responses. Cookies are sent in headers, Cookie in requests, Set-Cookie in responses.
    Stored in HTTP headers
    Bound to a domain name and possibly a path
    Can be manipulated on the client side unless `**HttpOnly**` flag is set: (`Document.cookie`)
    Can be manipulated on the server side: express middleware `cookie`
    Have a 4-kilobyte limit (no crazy large data)
- Cookie Flags
    `Secure` cookie can only be sent over HTTPS, not HTTP
    `HttpOnly` cookie can not be modified in the frontend
    `SameSite` cookie cannot be sent to cross domain website
- **Session**: **a** **session ID** stored in cookies (`connect.sid` by default) and **key/value pairs** stored in the server (`express-session`:`req.session`)
    **Session id** is a long random number or hash so that it can be unique and unforgeable