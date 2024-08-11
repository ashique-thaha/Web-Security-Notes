
DOM XSS happens when javascript takes data from attacker controllable source, such as the url and passes it to a sink that support code execution.

Vulnerable functions like `eval()` or `innerHTML`  enables attacker to execute malicious javascript.

`window.location` is a javascript object where the attackers can inject malicious javascript  using the url. There are a lot of javascript objects like this one.

###### Testing HTML sinks

To test for DOM XSS on an html sink place an alphanumeric string that might not exist on the code and inspect it using developer tools, some times you can see some sanitations or some extra elements might be present on this one. 

Try to seek out of those filters using basic logic.


### Testing JavaScript execution sinks

To test this is a bit harder, because our input doesn't really reflect in the DOM. So you can't search for it. instead use javascript debugger to determine how our input is send to the sink.

Manually search for vulnerable sources like location to see how the input is passed to sink and add breakpoint and try to understand how is the source's value is used. Sometime we might find the source getting assigned to other variables and also look if they are passed to a sink.

Also refine the inputs like html sinks to get a successful XSS.



## DOM XSS combined with reflected and stored data

if the script reads data from url and writes it to a sink, then the vulnerability is entirely client side.


## Sinks that can lead to DOM-XSS vulnerabilities

```
document.write() 
document.writeln() 
document.domain 
element.innerHTML 
element.outerHTML 
element.insertAdjacentHTML 
element.onevent
```




 # jQuery functions  which are sinks that can lead to DOM-XSS vulnerabilities

```js
add() 
after() 
append() 
animate() 
insertAfter() 
insertBefore() 
before() 
html() 
prepend() 
replaceAll() 
replaceWith() 
wrap() 
wrapInner() 
wrapAll() 
has() 
constructor() 
init() 
index() 
jQuery.parseHTML() 
$.parseHTML()
```