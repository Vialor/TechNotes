
> **World Wide Web** is a global collection of documents and other resources, linked by hyperlinks and URIs. It is distributed on worldwide servers, and accessed through a special application, **browsers**.

page is the basic unit of www
**hyperlink**: link that connects to web pages
**hypertext**: text that contains hyperlinks
**HTML Hypertext Markup Language**
# **URL Universe Resource Locator**
![[Untitled 3.png|Untitled 3.png]]
### How does a browser process a URL
1. Perform a DNS lookup on the hostname to get an IP address
    
    > Note that one IP address could be hosting many domains
    
2. Open TCP socket to the IP address on port 80(the HTTP port)
3. Send an HTTP request with desired path
4. Read HTTP response from the socket
5. Parse the HTML into the DOM
6. Render the page based on the DOM in the browser
7. Repeat step 1 to 4 for any pending external resources, and then render those resources
# HTTP Hypertext Transfer Protocol

> HTTP is based on TCP
### Status Codes:
1xx informational  
2xx success  
3xx redirection  
4xx client error  
5xx server error  
### Request Methods:
GET DELETE UPDATE PUT POST
### Request Headers:
Header Name: Header Value
Host: domain name of the server
User-Agent: browser and OS name of the client
Referer: webpage which led the client to this page
Cookie: key value pairs sent back and forth
Range: specifies a subset of bytes to fetch
Cache-Control: specify if we want a cached response
If-Modified-Since: only send back resource if it changes after this date
Connection: control TCP socket `keep-alive` `close`
Accept: which type of content we want `text/html`
Accept-Encoding: encoding algorithms we understand `gzip`
Accept-Language: what language we want `es`
### Response Headers:
Date: when response was sent
Last-Modified: when content was last modified
Cache-Control: whether cache the response on the client machine or not
Expires: discard reponse from cache after this date
Vary: list of headers that can affect the reponse; for cache
Set-Cookie: set cookie
Location: URL to redirect to (3xx responses)
Connection: control TCP socket `keep-alive` `close`
Content-Type: which type of content in the response `text/html`
Content-Encoding: encoding of the response
Content-Language: language of the response
Content-Length: length of the response in bytes