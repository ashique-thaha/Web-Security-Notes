In this step we enumerate the services hosted on the Target.

Since services often run on default ports, a good way to find them is by port-scanning the machine with either active or passive scanning

`in active scanning, you directly engage with the server`. Active scanning tools send requests to connect to the target machine’s ports to look for the open ones. You can use tools like `Nmap` or `Masscan` for active scanning.

For example, this simple Nmap command reveals the open ports 

`command:`

```
nmap scanme.nmap.org
```

`output:`

```
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.086s latency)
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 993 closed ports
PORT STATE SERVICE
22/tcp open ssh
25/tcp filtered smtp
80/tcp open http
135/tcp filtered msrpc
445/tcp filtered microsoft-ds
9929/tcp open nping-echo
31337/tcp open Elite
Nmap done: 1 IP address (1 host up) scanned in 230.83 seconds
```


 `In passive scanning, you use third-party resources to learn about a machine’s ports without interacting with the server`

To find services on a machine without actively scanning it, you can use `Shodan`, a search engine that lets the user find machines connected to the internet

With Shodan, you can discover the presence of webcams, web servers, or even power plants based on criteria such as hostnames or IP addresses

You can see that the search yields different data than our port scan, and provides additional information about the server

Alternatives to Shodan include Censys and Project Sonar. Combine the information you gather from different databases for the best results