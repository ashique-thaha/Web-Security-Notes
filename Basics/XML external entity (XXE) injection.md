
## Exploiting XXE to retrieve files

There are two ways to do this:

1. Introduce or edit a `DOCTYPE` element that defines an external entity containing the path to the file.

2. Edit a data value in the XML that is returned in the application's response, to make use of the defined external entity.

## 1. Introduce or edit a `DOCTYPE` element:

In this method, an attacker crafts or modifies an XML document's `DOCTYPE` declaration to include a reference to an external entity (usually a file on the server). For example:


```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
```

Here, the `ENTITY` declaration named `xxe` references a file (`/etc/passwd` in this case) using the `SYSTEM` keyword. When the XML parser processes this document, it resolves the entity reference and includes the contents of the specified file in the parsed output. The attacker can then observe or manipulate this output to retrieve sensitive information.

## 2. Edit a data value in the XML response:

In this method, an attacker doesn't directly control the XML document's `DOCTYPE` declaration. Instead, they manipulate the data values within the XML elements returned by the application.

Consider an XML response like this:


```xml
<user>    
   <name>John</name>   
   <age>30</age> 
</user>
```

The attacker can modify the XML data sent in the request to include an external entity reference. For example:



```xml
<user>   
   <name>John</name>   
   <age>&xxe;</age> 
</user>
```

If the application is vulnerable to XXE and doesn't properly sanitise or validate input, it may blindly parse the XML including the external entity reference. In this case, when the parser encounters `&xxe;`, it will replace it with the contents of the entity declaration, which could be a file path on the server. The attacker can then see the contents of that file in the application's response.

In both methods, the attacker's goal is to craft XML input in such a way that the XML parser resolves external entity references to retrieve sensitive information from the server. It's important for developers to properly validate and sanitise XML input to mitigate XXE vulnerabilities.

## Leverage XXE to SSRF

We can leverage XXE to SSRF by using the same way we used to retrieve files

```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://{ip}/endpoint"> ]>
```

don't forget to give `&xxe;`


## XML XInclude attacks

Here we include our XML payload on client submitted data, for example parameters, 

==Example:==
```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>
```


## How to prevent XXE vulnerabilities

Generally, it is sufficient to disable resolution of external entities and disable support for `XInclude`. This can usually be done via configuration options or by programmatically overriding default behaviour

