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

# Proxifying ffuf traffic

Whether it's for [network pivoting](https://blog.raw.pm/en/state-of-the-art-of-network-pivoting-in-2019/) or for using BurpSuite plugins you can send all the ffuf traffic through a web proxy (HTTP or SOCKS5).

```
$ ffuf -u http://MACHINE_IP/FUZZ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -x http://127.0.0.1:8080
```

It's also possible to send only matches to your proxy for replaying:

```
$ ffuf -u http://MACHINE_IP/FUZZ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -replay-proxy http://127.0.0.1:8080
```

This may be useful if you don't need all the traffic to traverse an upstream proxy and want to minimize resource usage or to avoid polluting your proxy history.
