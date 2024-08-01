## Intro
CSP(Content security Policy) is a browser mechanism that will help in mitigating XSS.
It works by regulating what all resources, like scripts and images that the website can load. also defining that if the page can be `Iframed` by other pages. 


To enable this CSP the response need to include the header called `Content-Security-Policy` 
##### Example

1. `Content-Security-Policy: script-src 'self'` => this will only allow scripts loaded from the same origin as the page itself.
2. `Content-Security-Policy: script-src https://scripts.normal-website.com` => This will load script only from the specified domain


==nonces and hashes are another way to load scripts securely==



##### Nonces
These are values given in CSP directives. the same value should be present in the tag that loads the script. Then only the script will execute

##### Hashes
These are of-course the hash of  contents of a trusted script. The hash will be given in CSP directive and if the hash of he actual script does not match the hash of script given in the directive then it will not be executed.

## Dangling markup injection 



