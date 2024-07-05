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

# Cross-API Scripting (XAS)

XAS is cross-site scripting performed across APIs.

For example, imagine that the hAPI Hacking blog has a sidebar powered by a LinkedIn newsfeed. The blog has an API connection to LinkedIn such that when a new post is added to the LinkedIn newsfeed, it appears in the blog sidebar as well. If the data received from LinkedIn isn’t sanitized, there is a chance that an XAS payload added to a LinkedIn newsfeed could be injected into the blog. To test this, you could post a LinkedIn newsfeed update containing an XAS script and check whether it successfully executes on the blog

The web app must poorly sanitize the data submitted through its own API or a third- party one.

The API input must also be injected into the web application in a way that would launch the script.

In addition to testing third-party APIs for XAS, you might look for the vulnerability in cases when a provider’s API adds content or makes changes to its web application.

```
POST /api/profile/update HTTP/1.1
Host: hapihackingblog.com
Authorization: hAPI.hacker.token
Content-Type: application/json
{
"fname": "hAPI",
"lname": "Hacker",
"city": "<script>alert("xas")</script>"
}
```

Another useful XAS tip is to try altering the Content-Type header to induce the API into accepting an HTML payload to spawn the script:

```
Content-Type: text/html
```
