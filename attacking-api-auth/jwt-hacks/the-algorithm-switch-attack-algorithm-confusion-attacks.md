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

# The Algorithm Switch Attack / Algorithm confusion attacks

## The Algorithm Switch Attack

Algorithm confusion attacks (also known as key confusion attacks) occur when an attacker is able to force the server to verify the signature of a JSON web token ([JWT](https://portswigger.net/web-security/jwt)) using a different algorithm than is intended by the website's developers. If this case isn't handled properly, this may enable attackers to forge valid JWTs containing arbitrary values without needing to know the server's secret signing key.

### How do algorithm confusion vulnerabilities arise? <a href="#how-do-algorithm-confusion-vulnerabilities-arise" id="how-do-algorithm-confusion-vulnerabilities-arise"></a>

Algorithm confusion vulnerabilities typically arise due to flawed implementation of JWT libraries. Although the actual verification process differs depending on the algorithm used, many libraries provide a single, algorithm-agnostic method for verifying signatures. These methods rely on the `alg` parameter in the token's header to determine the type of verification they should perform.

The following pseudo-code shows a simplified example of what the declaration for this generic `verify()` method might look like in a JWT library:

```javascript
function verify(token, secretOrPublicKey) {
    algorithm = token.getAlgHeader();
    if (algorithm == "RS256") {
        // Use the provided key as an RSA public key 
    } else if (algorithm == "HS256") {
        // Use the provided key as an HMAC secret key 
    }
}
```

Problems arise when website developers who subsequently use this method assume that it will exclusively handle JWTs signed using an asymmetric algorithm like RS256. Due to this flawed assumption, they may always pass a fixed public key to the method as follows:

```
publicKey = <public-key-of-server>;
token = request.getCookie("session");
verify(token, publicKey);
```

In this case, if the server receives a token signed using a symmetric algorithm like HS256, the library's generic `verify()` method will treat the public key as an HMAC secret. This means that an attacker could sign the token using HS256 and the public key, and the server will use the same public key to verify the signature.

{% hint style="info" %}
**Note**

The public key you use to sign the token must be absolutely identical to the public key stored on the server. This includes using the same format (such as X.509 PEM) and preserving any non-printing characters like newlines. In practice, you may need to experiment with different formatting in order for this attack to work.
{% endhint %}

An algorithm confusion attack generally involves the following high-level steps:

1. [Obtain the server's public key](https://portswigger.net/web-security/jwt/algorithm-confusion#step-1-obtain-the-server-s-public-key)
2. [Convert the public key to a suitable format](https://portswigger.net/web-security/jwt/algorithm-confusion#step-2-convert-the-public-key-to-a-suitable-format)
3. [Create a malicious JWT](https://portswigger.net/web-security/jwt/algorithm-confusion#step-3-modify-your-jwt) with a modified payload and the `alg` header set to `HS256`.
4. [Sign the token with HS256](https://portswigger.net/web-security/jwt/algorithm-confusion#step-4-sign-the-jwt-using-the-public-key), using the public key as the secret.

#### Step 1 - Obtain the server's public key <a href="#step-1-obtain-the-server-s-public-key" id="step-1-obtain-the-server-s-public-key"></a>

Servers sometimes expose their public keys as JSON Web Key (JWK) objects via a standard endpoint mapped to `/jwks.json` or `/.well-known/jwks.json`, for example. These may be stored in an array of JWKs called `keys`. This is known as a JWK Set.

```
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "75d0ef47-af89-47a9-9061-7c02a610d5ab",
            "n": "o-yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9mk6GPM9gNN4Y_qTVX67WhsN3JvaFYw-fhvsWQ"
        },
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "d8fDFo-fS9-faS14a9-ASf99sa-7c1Ad5abA",
            "n": "fc3f-yy1wpYmffgXBxhAUJzHql79gNNQ_cb33HocCuJolwDqmk6GPM4Y_qTVX67WhsN3JvaFYw-dfg6DH-asAScw"
        }
    ]
}
```

Even if the key isn't exposed publicly, you may be able to extract it from a pair of existing JWTs.

#### Step 2 - Convert the public key to a suitable format <a href="#step-2-convert-the-public-key-to-a-suitable-format" id="step-2-convert-the-public-key-to-a-suitable-format"></a>

Although the server may expose their public key in JWK format, when verifying the signature of a token, it will use its own copy of the key from its local filesystem or database. This may be stored in a different format.

In order for the attack to work, the version of the key that you use to sign the JWT must be identical to the server's local copy. In addition to being in the same format, every single byte must match, including any non-printing characters.

For the purpose of this example, let's assume that we need the key in X.509 PEM format. You can convert a JWK to a PEM using the [JWT Editor](https://portswigger.net/bappstore/26aaa5ded2f74beea19e2ed8345a93dd) extension in Burp as follows:

1. With the extension loaded, in Burp's main tab bar, go to the **JWT Editor Keys** tab.
2. Click **New RSA** Key. In the dialog, paste the JWK that you obtained earlier.
3. Select the **PEM** radio button and copy the resulting PEM key.
4. Go to the **Decoder** tab and Base64-encode the PEM.
5. Go back to the **JWT Editor Keys** tab and click **New Symmetric Key**.
6. In the dialog, click **Generate** to generate a new key in JWK format.
7. Replace the generated value for the `k` parameter with a Base64-encoded PEM key that you just copied.
8. Save the key.

### **Deriving public keys from existing tokens** <a href="#deriving-public-keys-from-existing-tokens" id="deriving-public-keys-from-existing-tokens"></a>

In cases where the public key isn't readily available, you may still be able to test for algorithm confusion by deriving the key from a pair of existing JWTs. This process is relatively simple using tools such as `jwt_forgery.py`. You can find this, along with several other useful scripts, on the [`rsa_sign2n` GitHub repository](https://github.com/silentsignal/rsa\_sign2n).

We have also created a simplified version of this tool, which you can run as a single command:

```
docker run --rm -it portswigger/sig2n <token1> <token2>
```

> **Note**
>
> You need the Docker CLI to run either version of the tool. The first time you run this command, it will automatically pull the image from Docker Hub, which may take a few minutes.

This uses the JWTs that you provide to calculate one or more potential values of `n`. Don't worry too much about what this means - all you need to know is that only one of these matches the value of `n` used by the server's key. For each potential value, our script outputs:

* A Base64-encoded PEM key in both X.509 and PKCS1 format.
* A forged JWT signed using each of these keys.

To identify the correct key, use Burp Repeater to send a request containing each of the forged JWTs. Only one of these will be accepted by the server. You can then use the matching key to construct an algorithm confusion attack.

[https://www.youtube.com/watch?v=4roTwhGSWZY](https://www.youtube.com/watch?v=4roTwhGSWZY)\
[https://portswigger.net/web-security/jwt/algorithm-confusion#deriving-public-keys-from-existing-tokens](https://portswigger.net/web-security/jwt/algorithm-confusion#deriving-public-keys-from-existing-tokens)
