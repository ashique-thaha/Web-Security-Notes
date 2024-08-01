Cookies are a key part of the HTTP protocol. As the http protocol is stateless the server needs to know who is asking for resources. cookies helps with that,


A server issues a cookie using the Set-Cookie response header, as you have seen:
```http
Set-Cookie: tracking=tI8rk7joMx44S2Uu85nSWc
```

the users browser will add this header with each request that the client make. so that the server can know who is accessing the resources.

multiple cookies can be issued by the server to the user.  As before, in cookie header each cookies will be separated using semicolon.


In addition to the cookie’s actual value,
the `Set-Cookie` header can include any of the following optional attributes, which can be used to control how the browser handles the cookie:

- `expires` sets a date until which the cookie is valid. This causes the browser to save the cookie to persistent storage, and it is reused in subsequent browser sessions until the expiration date is reached. If this attribute is not set, the cookie is used only in the current browser session.

- `domain` specifies the domain for which the cookie is valid. This must be the same or a parent of the domain from which the cookie is received.

- `path` specifies the URL path for which the cookie is valid.  

- `secure` — If this attribute is set, the cookie will be submitted only in HTTPS requests.

- `HttpOnly `— If this attribute is set, the cookie cannot be directly accessed via client-side JavaScript.