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

## Command

```
ffuf -u http://ffuf.me/cd/FUZZ -w wordlist.txt -sa -of csv -mc 200 -o output.csv -fl 0 -c -recursion -recursion-depth 3 -v -p '1.0-2.0' -t 100 -rate 2
```

