Finding directories on servers is valuable, because through them, we might discover hidden admin panels, configuration files, password files, outdated functionalities, database copies, source code files etc.

sometimes this will reveal valuable info like,

a pathname that includes `phpmyadmin` usually means that the application is built with PHP

we can use `Dirsearch` or `Gobuster` for directory brute-forcing

These tools use wordlists to construct URLs, and then request these URLs from a web server. If the server responds with a status code in the 200 range, the directory or file exists. 

This means you can browse to the page and see what the application is hosting there. A status code of 404 means that the directory or file doesn’t exist, 

while 403 means it exists but is protected. 

Examine 403 pages carefully to see if you can bypass the protection to access the content.

Here’s an example of running a Dirsearch command. The `-u` flag specifies the hostname, and the `-e` flag specifies the file extension to use when constructing URLs

`command:`

```shell
./dirsearch.py -u scanme.nmap.org -e php
```

`output:`

```shell
Extensions: php | HTTP method: get | Threads: 10 | Wordlist size: 6023
Error Log: /tools/dirsearch/logs/errors.log
Target: scanme.nmap.org
[12:31:11] Starting:
[12:31:13] 403 - 290B - /.htusers
[12:31:15] 301 - 316B - /.svn -> http://scanme.nmap.org/.svn/
[12:31:15] 403 - 287B - /.svn/
[12:31:15] 403 - 298B - /.svn/all-wcprops
[12:31:15] 403 - 294B - /.svn/entries
[12:31:15] 403 - 297B - /.svn/prop-base/
[12:31:15] 403 - 296B - /.svn/pristine/
[12:31:15] 403 - 291B - /.svn/tmp/
[12:31:15] 403 - 315B - /.svn/text-base/index.php.svn-base
[12:31:15] 403 - 293B - /.svn/props/
[12:31:15] 403 - 297B - /.svn/text-base/
[12:31:40] 301 - 318B - /images -> http://scanme.nmap.org/images/
[12:31:40] 200 - 7KB - /index
[12:31:40] 200 - 7KB - /index.html
[12:31:53] 403 - 295B - /server-status
[12:31:53] 403 - 296B - /server-status/
[12:31:54] 301 - 318B - /shared -> http://scanme.nmap.org/shared/
Task Completed
```

Gobuster’s `Dir` mode is used to find additional content on a specific domain or subdomain. This includes hidden directories and files. In this mode, you can use the `-u` flag to specify the domain or subdomain you want to brute-force and `-w` to specify the wordlist you want to use


`command:`

```shell
gobuster dir -u target_url -w wordlist
```

Manually visiting all the pages you’ve found through brute forcing can be time-consuming. Instead, use a screenshot tool like `EyeWitness`  link:(https://github.com/FortyNorthSecurity/EyeWitness/) or `Snapper` (https://github.com/dxa4481/Snapper/) to automatically verify that a page is hosted on each location.

`EyeWitness` accepts a list of URLs and takes screenshots of each page. In a photo gallery app, you can quickly skim these to find the interesting-looking ones.

Keep an eye out for hidden services, such as developer or admin panels, directory listing pages, analytics pages, and pages that look outdated and ill maintained. 

These are all common places for vulnerabilities to manifest



1. **1xx - Informational**: These are provisional responses that indicate the server has received the request and is processing it. The requestor should continue to wait for the final response.
    
2. **2xx - Success**: These response codes indicate that the request was received, understood, and processed successfully by the server. The most common code in this range is 200, which means "OK."
    
3. **3xx - Redirection**: These codes inform the client that further action is required to complete the request. The client might need to follow a different URL to obtain the requested resource.
    
4. **4xx - Client Error**: These codes indicate that there was an error with the client's request. They are often caused by mistakes in the request, such as requesting a non-existent page. Common codes in this range include 404 (Not Found) and 403 (Forbidden).
    
5. **5xx - Server Error**: These codes are returned when the server encounters an error or is incapable of performing the request. This indicates a problem on the server's side. A common code in this range is 500 (Internal Server Error).


Here are a few examples of response codes from each range:

- 100 (Continue): The server has received the initial part of the request and is telling the client to continue.
    
- 200 (OK): The request was successful, and the server has returned the requested data.
    
- 301 (Moved Permanently): The requested resource has been moved to a new URL permanently.
    
- 404 (Not Found): The requested resource could not be found on the server.
    
- 500 (Internal Server Error): Something went wrong on the server's end, and the request could not be fulfilled.