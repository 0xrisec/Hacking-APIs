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
