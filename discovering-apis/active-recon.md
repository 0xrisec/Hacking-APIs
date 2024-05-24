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

# Active Recon

One shortcoming of performing passive reconnaissance is that you’re collecting information from secondhand sources. As an API hacker, the best way to validate this information is to obtain information directly from a target by port or vulnerability scanning, pinging, sending HTTP requests, making API calls, and other forms of interaction with a target’s environment. This section will focus on discovering an organization’s APIs using detection scanning, hands-on analysis, and targeted scanning.

## Phase One:

&#x20;Detection Scanning The goal of detection scanning is to reveal potential starting points for your investigation. Begin with general scans meant to detect hosts, open ports, services running, and operating systems currently in use, as described in the “Baseline Scanning with Nmap” section of this chapter. APIs use HTTP or HTTPS, so as soon as your scan detects these services, let the scan continue to run and move into phase two.

## Phase Two: Hands-on Analysis

You should usually consider the application from three perspectives: guests, authenticated users, and site administrators.

Your first step is to visit the website in a browser, explore the site, and consider it from these perspectives. Here are some considerations for each user group:&#x20;

#### Guest&#x20;

How would a new user use this site? \
Can new users interact with the API? \
Is API documentation public? \
What actions can this group perform?&#x20;

#### Authenticated User&#x20;

What can you do when authenticated that you couldn’t do as a guest? \
Can you upload files? \
Can you explore new sections of the web application? \
Can you use the API? \
How does the web application recognize that a user is authenticated?&#x20;

#### Administrator&#x20;

Where would site administrators log in to manage the web app? \
What is in the page source? \
What comments have been left around various pages? \
What programming languages are in use? \
What sections of the website are under development or experimental?

## Phase Three: Targeted Scanning

In the targeted scanning phase, refine your scans and use tools that are specific to your target. Whereas detection scanning casts a wide net, targeted scanning should focus on the specific type of API, its version, the web appli- cation type, any service versions discovered, whether the app is on HTTP or HTTPS, any active TCP ports, and other information gleaned from understanding the business logic. For example, if you discover that an API is running over a nonstandard TCP port, you can set your scanners to take a closer look at that port. If you find out that the web application was made with WordPress, check whether the WordPress API is accessible by visiting /wp-json/wp/v2. At this point, you should know the URLs of the web application and can begin brute-forcing uniform resource identifiers to find hiddenapplication directories and files

## Nmap

```
nmap -sC -sV <target address or network range> -oA nameofoutput
nmap -p- <target address> -oA allportscan

```

### Brute-Forcing URIs with FFUF

You can take word list from  [assetnote wordlist](https://wordlists.assetnote.io/)

### Discovering API Content with Kiterunner

#### Generic Scan

```
kr scan http://192.168.195.132:8090 -w ~/routes-large.kite
```

#### Formats, json

```
kr scan http://192.168.195.132:8090 -w ~/routes-large.kite -o json,txt
```

#### Doing a generic bruteforce

```
kr brute http://192.168.195.132:8090 -A=airroutes-210228
```

\-A : load [assetnote wordlist](https://wordlists.assetnote.io/)

You can simultaneously utilize both -A and -w to include additional words.

```
kr wordlist list
```

#### Replay a request

See orginal request, application in debug mode

```
kr kb replay "GET 414 [ 183, 7, 8] http://192.168.50.35:8888/api/privatisations/
count 0cf6841b1e7ac8badc6e237ab300a90ca873d571" -w ~/api/wordlists/data/kiterunner/routes-
large.kite
```

Result can be false positive so use burpsuite for further investigation.

#### Filter

```
kr brute http://192.168.195.132:8090 -A=airroutes-210228 
--fail-status-codes 400,401,303,501,502,426,411
```

#### More than one target:

```
kr scan source.txt -w data-large.kite
```

#### Add Headers

```
kr scan http://192.168.50.35:8090 -w ~/api/wordlists/data/kiterunner/routes-large.kite -H
'x-access-token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7Il9pZCI6NDUsImVtYWlsIjoiaGF
waUBoYWNrZXIuY29tIiwicGFzc3dvcmQiOiJQYXNzd29yZDEhIiwibmFtZSI6Im15c2VsZmNyeSIsInBpYyI6Imh0dHBzO
i8vczMuYW1hem9uYXdzLmNvbS91aWZhY2VzL2ZhY2VzL3R3aXR0ZXIvZ2FicmllbHJvc3Nlci8xMjguanBnIiwiaXNfYWRt
aW4iOmZhbHNlLCJhY2NvdW50X2JhbGFuY2UiOjUwLCJhbGxfcGljdHVyZXMiOltdfSwiaWF0IjoxNjMxNDE2OTYwfQ._qoC
_kgv6qlbPLFuH07-DXRUm9wHgBn_GD7QWYwvzFk'
```
