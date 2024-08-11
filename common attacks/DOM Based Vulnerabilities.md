##### What is DOM?
Dom or document object model is web browser's hierarchical representation of of elements on page.

##### DOM Manipulation
Javascript can be used to manipulate DOM's objects and nodes, also their properties. 
Dom manipulation is actually not a vulnerability in fact this is the way most of the modern web app works, by making changes to the values and properties of DOM.

##### DOM Vulnerability
If javascript handles the data insecurely then DOM can be a place of vulnerability.
DOM based vulnerabilities arise when a website contains JavaScript that takes an attacker-controllable value, known as a source, and passes it into a dangerous function, known as a sink.

##### Source
Source is a javascript property that takes in  attacker controlled inputs.
ex: url

one another  example is `location.search` this one will accept inputs from a query string which is relatively easy way for an attacker to control. 
So basically any properties that attacker can control will be a potential source. This also includes `document.referrer` which is the property that refers to url.

##### Sinks
This is a dangerous JS function or property or DOM object, where it will result on vulnerabilities if attacker controlled data is passed on it. So basically the input passed from source are executed on sink.

`Example:`

`eval()` function : This one is a sink because this one consider the argument passed on it as javascript.

`document.body.innerhtml` : This one is also dangerous because it helps attacker inject arbitrary javascript and html

So fundamentally the concept of DOM vulnerabilities is that a website passes data from source to sink  and handles it in an unsafe way. 

The most common type of source is URL. Which is accessed along with the location object. Like, an attacker can construct a link to send a victim to a vulnerable page with a payload in the query string and fragment the portions of the url. 


This is vulnerable to DOM based open redirection this is because the source location.hash property is handled in an unsafe way.

```js
goto = location.hash.slice(1) 
if (goto.startsWith('https:')) { 
   location = goto; 
   }
```

This is a vulnerable code because we can use `location.hash` to cause open redirect. Attacker can do this by modifying the url like:

Before:
`https://www.innocent-website.com/example`

After:
`https://www.innocent-website.com/example#https://www.evil-user.net`

By using this method the attacker can redirect the victim to malicious site by modifying this fragment section.


##### Common Sources that can cause vulnerabilities:

```
document.URL 
document.documentURI 
document.URLUnencoded 
document.baseURI 
location 
document.cookie 
document.referrer 
window.name 
history.pushState 
history.replaceState 
localStorage 
sessionStorage 
IndexedDB (mozIndexedDB, webkitIndexedDB, msIndexedDB) 
Database
```

