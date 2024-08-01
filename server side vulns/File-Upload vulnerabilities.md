
File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficiently validating things like their name, type, contents, or size.

developers implement what they believe to be robust validation that is either inherently flawed or can be easily bypassed.

For example, they may attempt to blacklist dangerous file types. As with any blacklist, it's also easy to accidentally exclude more obscure file types that may still be dangerous.

In other cases, the website may attempt to check the file type by verifying properties that can be easily manipulated by an attacker using tools like Burp Proxy or Repeater.

Ultimately, even robust validation measures may be applied inconsistently across the network of hosts and directories that form the website, resulting in discrepancies that can be exploited.



`Exploiting unrestricted file uploads to deploy a web shell`

From a security perspective, the worst possible scenario is when a website allows you to upload server-side scripts, such as PHP, Java, or Python files, and is also configured to execute them as code. This makes it trivial to create your own web shell on the server.

A web shell is a malicious script that enables an attacker to execute arbitrary commands on a remote web server simply by sending HTTP requests to the right endpoint


If you're able to successfully upload a web shell, you effectively have full control over the server.

This means you can read and write arbitrary files, exfiltrate sensitive data, even use the server to carry on attacks against both internal infrastructure and other servers outside the network. 


For example, the following PHP one-liner could be used to read arbitrary files from the server's filesystem:
```php
<?php echo file_get_contents('/path/to/target/file'); ?>
```
Once uploaded, sending a request for this malicious file will return the target file's contents in the response.



A more versatile web shell may look something like this:
```php
<?php echo system($_GET['command']); ?>

```



This script enables you to pass an arbitrary system command via a query parameter as follows:
```php
GET /example/exploit.php?command=id HTTP/1.1

```


When submitting HTML forms, the browser typically sends the provided data in a `POST` request with the content type `application/x-www-form-url-encoded`. 

This is fine for sending simple text like your name or address. However, it isn't suitable for sending large amounts of binary data, such as an entire image file or a PDF document.

In this case, the content type `multipart/form-data` is preferred



One way that websites may attempt to validate file uploads is to check that this input-specific `Content-Type` header matches an expected MIME type.

If the server is only expecting image files, for example, it may only allow types like `image/jpeg` and `image/png`. Problems can arise when the value of this header is implicitly trusted by the server.

If no further validation is performed to check whether the contents of the file actually match the supposed MIME type, this defence can be easily bypassed using tools like Burp Repeater.