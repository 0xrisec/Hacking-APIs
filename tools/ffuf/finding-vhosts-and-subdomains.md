---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Finding vhosts and subdomains

ffuf may not be as efficient as specialized tools when it comes to subdomain enumeration but it's possible to do.

```
$ ffuf -u http://FUZZ.mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

Some subdomains might not be resolvable by the DNS server you're using and are only resolvable from within the target's local network by their private DNS servers. So some virtual hosts (vhosts) may exist with private subdomains so the previous command doesn't find them. To try finding private subdomains we'll have to use the Host HTTP header as these requests might be accepted by the web server.\
Note: [virtual hosts](https://httpd.apache.org/docs/2.4/en/vhosts/examples.html) (vhosts) is the name used by Apache httpd but for Nginx the right term is [Server Blocks](https://www.nginx.com/resources/wiki/start/topics/examples/server\_blocks/).\


You could compare the results obtained with direct subdomain enumeration and with vhost enumeration:

```
$ ffuf -u http://FUZZ.mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -fs 0
$ ffuf -u http://mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H 'Host: FUZZ.mydomain.com' -fs 0
```

For example, it is possible that you can't find a sub-domain with direct subdomain enumeration (1st command) but that you can find it with vhost enumeration (2nd command).

Vhost enumeration technique shouldn't be discounted as it may lead to discovering content that wasn't meant to be accessed externally.
