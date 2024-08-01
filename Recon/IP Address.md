Another way of discovering your target’s top-level domains is to locate IP addresses. here we take facebook.com as example:

`command:`
```
nslookup facebook.com
```

we can use ns-lookup to find ip:

`output:`
```
Server: 192.168.0.1
Address: 192.168.0.1#53
Non-authoritative answer:
Name: facebook.com
Address: 157.240.2.35
```

here we have found ip address of facebook.com we can run a reverse whois using ip to find details.This will look for domains on the same server given an ip:

`command:`
```
whois 157.240.2.35
```

`output:`
```
NetRange: 157.240.0.0 - 157.240.255.255
CIDR: 157.240.0.0/16
NetName: THEFA-3
NetHandle: NET-157-240-0-0-1
Parent: NET157 (NET-157-0-0-0-0)
NetType: Direct Assignment
OriginAS:
Organization: Facebook, Inc. (THEFA-3)
RegDate: 2015-05-14
Updated: 2015-05-14
Ref: https://rdap.arin.net/registry/ip/157.240.0.0
OrgName: Facebook, Inc.
OrgId: THEFA-3
Address: 1601 Willow Rd.
City: Menlo Park
StateProv: CA
PostalCode: 94025
Country: US
RegDate: 2004-08-11
Updated: 2012-04-17
Ref: https://rdap.arin.net/registry/entity/THEFA-3
OrgAbuseHandle: OPERA82-ARIN
OrgAbuseName: Operations
OrgAbusePhone: +1-650-543-4800
OrgAbuseEmail: noc@fb.com
OrgAbuseRef: https://rdap.arin.net/registry/entity/OPERA82-ARIN
OrgTechHandle: OPERA82-ARIN
OrgTechName: Operations
OrgTechPhone: +1-650-543-4800
OrgTechEmail: noc@fb.com
OrgTechRef: https://rdap.arin.net/registry/entity/OPERA82-ARIN
```

Another way of finding IP addresses in scope is by looking at autonomous systems, which are routable networks within the public internet 

 Autonomous system numbers (ASNs) identify the owners of these networks.

By checking if two IP addresses share an ASN, you can determine whether the IPs belong to the same owner.

To figure out if a company owns a dedicated IP range, run several IP-to-ASN translations to see if the IP addresses map to a single ASN

If many addresses within a range belong to the same ASN, the organisation might have a dedicated IP range

From the following output, we can deduce that any IP within the 157.240.2.21 to 157.240.2.34 range probably belongs to Facebook

`command:`
```
whois -h whois.cymru.com 157.240.2.20
```


`output:`
```
AS | IP | AS Name
32934 | 157.240.2.20 | FACEBOOK, US
```


`command:`
```
whois -h whois.cymru.com 157.240.2.2
```


`output:`
```
AS | IP | AS Name
32934 | 157.240.2.27 | FACEBOOK, US
```


`command:`
```
whois -h whois.cymru.com 157.240.2.35
```


`output:`
```
AS | IP | AS Name
32934 | 157.240.2.35 | FACEBOOK, US
```


The `-h` flag in the whois command sets the WHOIS server to retrieve information from, and `whois.cymru.com` is a database that translates IPs to ASNs.

If the company has a dedicated IP range and doesn’t mark those addresses as out of scope, you could plan to attack every IP in that range


To find all ip address of a domain using amass:
```shell
amass enum -d example.com
```
