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

# File Extensions

We've come across a directory called /logs but we cannot view its contents. We can make a safe assumption that the files stored in here are using the .log extension

Use the below scan with the -e switch to specify the extension type to add onto the end of every word in the wordlist to find the correct log

```
ffuf -w ~/wordlists/common.txt -e .log -u http://ffuf.me/cd/ext/logs/FUZZ
```
