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

# Recursion

This is similar to the first scan but this time we're using the -recursion switch. This switch tells ffuf that if it encounters a directory it should start another scan within that directory and so on until no more results are found

```
ffuf -w ~/wordlists/common.txt -recursion -u http://ffuf.me/cd/recursion/FUZZ
```

This scan should uncover the directory **/admin** and then **/admin/users** and finally the file **/admin/users/96**
