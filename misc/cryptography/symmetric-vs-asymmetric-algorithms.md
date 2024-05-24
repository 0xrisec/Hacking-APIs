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

# Symmetric vs asymmetric algorithms

JWTs can be signed using a range of different algorithms. Some of these, such as HS256 (HMAC + SHA-256) use a "symmetric" key. This means that the server uses a single key to both sign and verify the token. Clearly, this needs to be kept secret, just like a password.

Other algorithms, such as RS256 (RSA + SHA-256) use an "asymmetric" key pair. This consists of a private key, which the server uses to sign the token, and a mathematically related public key that can be used to verify the signature.

As the names suggest, the private key must be kept secret, but the public key is often shared so that anybody can verify the signature of tokens issued by the server.

