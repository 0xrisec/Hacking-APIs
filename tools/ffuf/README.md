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

# ffuf

## Lab

[http://ffuf.me/](http://ffuf.me/)

## Payloads

[https://github.com/danielmiessler/SecLists#install](https://github.com/danielmiessler/SecLists#install)\
[https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/big.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/big.txt)\
[https://gist.github.com/0xrisec/a82521dabf52e1cc4d446ba5435aae19](https://gist.github.com/0xrisec/a82521dabf52e1cc4d446ba5435aae19)

## Simple Command

```
ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/big.txt
```

## Finding pages and directories

Start enumerating with a generic list of files such as [raft-medium-files-lowercase.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-medium-files-lowercase.txt).

```
ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt
```

However, using a large generic wordlist containing irrelevant file extensions is not very efficient.\
Instead, we can usually assume `index.<extension>` is the default page on most websites so we can try common extensions for just the index page. With this method, we can usually determine what programming language or languages the site uses.

```
ffuf -u http://MACHINE_IP/indexFUZZ -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt
```

[https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt)\
\
Now that we know the extensions supported we can try a list of generic words (without of extension) and apply the extensions we know works (found from Q2) + some common ones like `.txt`.

We'll exclude the 4 letter extensions from this wordlist as it will result in many false positives.

```
ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e .php,.txt
```



## Remove False positive

[https://codingo.io/tools/ffuf/bounty/2020/09/17/everything-you-need-to-know-about-ffuf.html#automatically-calibrate-filtering](https://codingo.io/tools/ffuf/bounty/2020/09/17/everything-you-need-to-know-about-ffuf.html#automatically-calibrate-filtering)

```
ffuf -w vhostnames.txt -u https://target -H "Host: FUZZ. target" -acc "www"
```

## Filter status codes

```
ffuf -u http://example.com/FUZZ -w wordlist.txt -fc 200,301,403
```

## Filter Payloads

```
ffuf -u https://0a4e00c4044a052280181c32006700c8.web-security-academy.net/FUZZ -w <(grep "jwks" dirsearch-final.txt)
```
