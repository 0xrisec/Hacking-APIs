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

# Lab

Discover the IP address and verify open ports.

```
nmap -p- 192.168.50.35
```

Follow up the general Nmap scan with an all-port scan to see if anything is hiding on an uncommon port.

```
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
1025/tcp open  NFS-or-IIS
2377/tcp open  swarm
7946/tcp open  unknown
8025/tcp open  ca-audit-da
```

The results of your initial Nmap scans reveal a web application running on port 8080, which should lead to the next logical step: a hands-on analysis of the web app. Visit all ports that sent HTTP responses to Nmap (namely, ports 8000, 8025, 8080, 8087, and 8888)

Now use DevTools to investigate the JavaScript source files on this page. Visit the Network tab and refresh the page so the source files populate. Select a source file that interests you, right-click it, and send it to the Sources panel. Find API endpoints, sensitive information, and bugs.

You can interact with this API’s functions and identify its weaknesses.
