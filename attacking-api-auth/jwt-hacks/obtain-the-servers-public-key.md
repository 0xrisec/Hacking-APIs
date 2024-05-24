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

# Obtain the server's public key

## ssh-keyscan

Download the public key from GitHub and save it into the github.pub file:

```
ssh-keyscan -t rsa github.com | sed "s/^[^ ]* //" > github.pub
```

Convert SSH public key format into X.509 public key format:

```
ssh-keygen -f github.pub -e -m pem > github.pem
```

Parse ASN.1 encoding of key to obtain public key modulus and exponent:

```
sed "/--/d" github.pem | openssl asn1parse | grep "INTEGER" | sed "s/.*://"
```

Result:

```
AB603B8511A67679BDB540DB3BD2034B004AE936D06BE3D760F08FCBAADB4EB4EDC3B3C791C70AAE9A74C95869E4774421C2ABEA92E554305F38B5FD414B3208E574C337E320936518462C7652C98B31E16E7DA6523BD200742A6444D83FCD5E1732D03673C7B7811555487B55F0C4494F3829ECE60F94255A95CB9AF537D7FC8C7FE49EF318474EF2920992052265B0A06EA66D4A167FD9F3A48A1A4A307EC1EAAA5149A969A6AC5D56A5EF627E517D81FB644F5B745C4F478ECD082A9492F744AAD326F76C8C4DC9100BC6AB79461D2657CB6F06DEC92E6B64A6562FF0E32084EA06CE0EA9D35A583BFB00BAD38C9D19703C549892E5AA78DC95E250514069
23
```

## SSH

Use `ssh`'s verbose options: `ssh -v user@hostname.example.com`, and look at stderr.
