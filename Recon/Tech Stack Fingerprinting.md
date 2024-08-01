Fingerprinting techniques can help you understand the target application even better.

Fingerprinting is identifying the software brands and versions
that a machine or an application uses. 

This information allows you to perform targeted attacks on the application, because you can search for any known misconfigurations and publicly disclosed vulnerabilities related to a particular version. 

For example, if you know the server is using an old version of Apache that could be impacted by a disclosed vulnerability, you can immediately attempt to attack the server using it.

The security community classifies known vulnerabilities as Common Vulnerabilities and Exposures (CVEs) and gives each CVE a number for reference. 


The simplest way of fingerprinting an application is to engage with the application directly. 

First, run `Nmap` on a machine with the `-sV` flag on to
enable version detection on the port scan.

Here, you can see that `Nmap` attempted to fingerprint some software running on the target host for us

```shell
nmap scanme.nmap.org -sV
```


output:

```shell
Starting Nmap 7.60 ( https://nmap.org )
Nmap scan report for scanme.nmap.org (45.33.32.156)Web Hacking Reconnaissance 79
Host is up (0.065s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 992 closed ports
PORT STATE SERVICE VERSION
22/tcp open ssh OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
25/tcp filtered smtp
80/tcp open http Apache httpd 2.4.7 ((Ubuntu)
135/tcp filtered msrpc
139/tcp filtered netbios-ssn
445/tcp filtered microsoft-ds
9929/tcp open nping-echo Nping echo
31337/tcp open tcpwrapped
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
Service detection performed. Please report any incorrect results at https://nmap.org/submit/.
Nmap done: 1 IP address (1 host up) scanned in 9.19 seconds
```


Next, in Burp, send an HTTP request to the server to check the HTTP headers used to gain insight into the tech stack.

eg:
```http
Server: Apache/2.0.6 (Ubuntu)
X-Powered-By: PHP/5.0.1
X-Generator: Drupal 8
X-Drupal-Dynamic-Cache: UNCACHEABLE
Set-Cookie: PHPSESSID=abcde;
```

HTTP headers like `Server and X-Powered-By` are good indicators of technologies. 

The `Server header` often reveals the software versions running
on the server. 

`X-Powered-By` reveals the server or scripting language used.


only `Drupal` uses `X-Generator` and ``X-Drupal-Dynamic-Cache``. 

Technology-specific cookies such as `PHPSESSID` are also clues; if a server sends back a cookie named`PHPSESSID`, it’s probably developed using `PHP`.


The `HTML source code` of web pages can also provide clues. 

Many `web frameworks` or other technologies will embed a `signature in source code`.

Right-click a page, select View Source Code, and press CTRL-F to search for phrases like powered by, built with, and running.

For instance, you might find `Powered by: WordPress 3.3.2` written in the source.

Check technology-specific file extensions, filenames, folders, and directories. 

For example, a file named `phpmyadmin` at the root directory, like
`https://example.com/phpmyadmin`, means the application runs `PHP`.

A directory named `jinja2` that contains templates means the site probably uses `Django`and `Jinja2` 


Several applications can automate this process. `Wappalyzer (https://www.wappalyzer.com/)` is a browser extension that identifies content management systems, frameworks, and programming languages used on a site.

`BuiltWith (https://builtwith.com/)` is a website that shows you which web technologies a site is built with. 

`StackShare (https://stackshare.io/)` is an online platform
that allows developers to share the tech they use. 

You can use it to find out if the organisation’s developers have posted their tech stack. Finally, `Retire.js` is a tool that `detects outdated JavaScript libraries and Node.js packages.`