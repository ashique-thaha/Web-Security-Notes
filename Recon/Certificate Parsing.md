  Another way of finding hosts is to take advantage of the Secure Sockets Layer (SSL) certificates used to encrypt web traffic

An SSL certificate’s `Subject Alternative Name field lets certificate owners specify additional hostnames` that use the same certificate, so you can find those hostnames by parsing this field

`Use online databases like crt.sh, Censys, and Cert Spotter to find certificates for a domain`

running a certificate search using `crt.sh` for `facebook.com`

`output:`

```
X509v3 Subject Alternative Name:
DNS:*.facebook.com
DNS:*facebook.net
DNS:*.fbcdn.net
DNS:*.fbsbx.com
DNS:*.messenger.com
DNS:facebook.com
DNS:messenger.com
DNS:*.m.facebook.com
DNS:*.xx.fbcdn.net
DNS:*.xy.fbcdn.net
DNS:*.xz.fbcdn.net
```


The `crt.sh` website also has a useful utility that lets you retrieve the information in JSON format, rather than HTML, for easier parsing. Just add the URL parameter `output=json` to the request URL:
`https://crt.sh/?q=facebook.com&output=json`
