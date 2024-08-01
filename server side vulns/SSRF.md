Server-side request forgery is a web security vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location

so, imagine here is a scenario that a web application is requesting for whether forecast details on an external API.

like this,
`wheatherApi=http://bla-bla-bla

It does this by passing the URL to the relevant back-end API endpoint via a front-end HTTP request.

so simply the application makes an http request to backend api and returns results via wheatherApi parameter.

for example, we can modify the request that is going to the server via the wheatherApi parameter by changing the contents,

example:
`wheatherApi=http://localhost/admin`

if the /admin url is coming from the local machine itself the application thinks it is accessed from trusted location.

so why does this happen? why the application simply trust the request coming from local machine?

The access checking functionality might be implemented on different component that sits on the front end of the application.So when the request is made from the application itself back to the server, it bypass these access control functionalities.


for disaster recovery purposes, the application might allow admin access to users who access from the local machine,This provides a way for an administrator to recover the system if they lose their credentials. 

This assumes that only a fully trusted user would come directly from the server.

The administrative interface might listen on a different port number to the main application, and might not be reachable directly by users so the application might trust the request legit.


in some cases the application might be able to interact with the backend systems which are not accessible by every users,

These systems often have non-routable private IP addresses. The back-end systems are normally protected by the network topology, so they often have a weaker security posture

As this is non-routable private ip addresses the one who can interact with the machine via the application will be authenticated without any checks.


## whitelist bypass techniques

- You can embed credentials in a URL before the hostname, using the `@` character. For example:
    
    `https://expected-host:fakepassword@evil-host`
    
- You can use the `#` character to indicate a URL fragment. For example:
    
    `https://evil-host#expected-host`
    
- You can leverage the DNS naming hierarchy to place required input into a fully-qualified DNS name that you control. For example:
    
    `https://expected-host.evil-host`
    
- You can URL-encode characters to confuse the URL-parsing code. This is particularly useful if the code that implements the filter handles URL-encoded characters differently than the code that performs the back-end HTTP request. You can also try [double-encoding](https://portswigger.net/web-security/essential-skills/obfuscating-attacks-using-encodings#obfuscation-via-double-url-encoding) characters; some servers recursively URL-decode the input they receive, which can lead to further discrepancies.

#### localhost alternatives:
```
127.0.0.1
2130706433
017700000001
127.1
```



## Blind SSRF
Blind SSRF happens when an application make a  back-end HTTP request to the supplied URL, but the response from the back-end request is not returned in the front-end response.

