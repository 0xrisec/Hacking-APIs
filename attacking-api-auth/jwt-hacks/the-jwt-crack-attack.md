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

# The JWT Crack Attack

The JWT Crack attack attempts to crack the secret used for the JWT signature hash, giving us full control over the process of creating our own valid JWTs. Hash-cracking attacks like this take place offline and do not interact with the provider. Therefore, we do not need to worry about causing havoc by sending millions of requests to an API provider.

You can use `JWT_Tool` or a tool like `Hashcat` to crack JWT secrets. You’ll feed your hash cracker a list of words. The hash cracker will then hash those words and compare the values to the original hashed signature to determine if one of those words was used as the hash secret. If you’re performing a long-term brute-force attack of every character possibility, you may want to use the dedicated GPUs that power Hashcat instead of JWT\_Tool. That being said, JWT\_Tool can still test 12 million passwords in under a minute.

To perform a JWT Crack attack using JWT\_Tool, use the following command:

```
jwt_tool <JWT Token> -C -d /wordlist.txt
```

The -C option indicates that you’ll be conducting a hash crack attack and the -d option specifies the dictionary or wordlist you’ll be using against the hash.

Useful list of 3502 public-available JWT: [wallarm/jwt-secrets/jwt.secrets.list](https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list), including `your_jwt_secret`, `change_this_super_secret_random_string`, etc.
