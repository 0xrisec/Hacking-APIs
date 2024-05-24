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

# The None Attack

If you ever come across a JWT using "none" as its algorithm, you’ve found an easy win. After decoding the token, you should be able to see the header, payload, and signature. From here, you can alter the information contained in the payload to whatever you’d like.&#x20;

For example, you could change the username to something likely used by the provider’s admin account (like root, admin, administrator, test, or adm).

Once you’ve edited the payload, use Burp Suite’s Decoder to encode the payload with base64; then insert it into the JWT. Importantly, since the algorithm is set to "none", any signature that was present can be removed. In other words, you can remove everything following the third period in the JWT. Send the JWT to the provider in a request and check whether you’ve gained unauthorized access to the API.

Set the algorithm used as `"None"` and remove the signature part.

None algorithm variants:

* none
* None
* NONE
* nOnE

Alternatively, you can modify an existing JWT (be careful with the expiration time)

**Using jwt\_tool**

```
python3 jwt_tool.py [JWT_HERE] -X a
```

Try using different HTTP methods such as GET, POST, and PUT for each API request. \[I found this in [juice-shop](https://juice-shop.herokuapp.com/) lab]. I found that a "none" algo token works with the GET method but not with the PUT method in the same request.

Remove the header part of the JWT token and send the request. Check if there is any error information in the response that can help with further investigation.
