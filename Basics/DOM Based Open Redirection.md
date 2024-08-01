Dom based open redirection happens when a script writes attacker controllable data into a sink that helps the attacker to redirect the victim to attacker intending website.

```
let url = /https?:\/\/.+/.exec(location.hash); 
if (url) {   
  location = url[0]; 
}
```

This  code is vulnerable due to the unsafe way it handles the  `location.hash` property.
The attacker will be able to construct the vulnerability in a way that the attacker can redirect the victim to any domain to that they want. This can be escalate to executing a phishing attack.


Further it can be escalated to javascript injection attack, we can use payloads starting with `javascript:` to execute javascript code.


###### sinks that can lead to DOM-based open-redirection vulnerabilities:

```
location 
location.host 
location.hostname 
location.href 
location.pathname 
location.search 
location.protocol 
location.assign() 
location.replace() 
open() 
element.srcdoc 
XMLHttpRequest.open() 
XMLHttpRequest.send() 
jQuery.ajax() 
$.ajax()
```
