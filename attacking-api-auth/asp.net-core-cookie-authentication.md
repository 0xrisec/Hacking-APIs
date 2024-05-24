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

# ASP.NET Core Cookie Authentication

ASPXAUTH : beehive

The cookies you're seeing, `idsrv.session` and `idsrv`, suggest that the authentication mechanism in use is likely related to IdentityServer. IdentityServer is an open-source framework for implementing authentication and authorization services in .NET applications. It supports various authentication protocols like OpenID Connect and OAuth 2.0.

The presence of these cookies indicates that the application is using IdentityServer for handling user authentication. When a user logs in, IdentityServer issues these cookies as part of the session management. Here's a brief overview of what these cookies might represent:

* `idsrv.session`: This cookie is typically used for session management. It can help in maintaining the session state between the browser and the server. It doesn't contain sensitive information but rather an identifier to maintain session consistency.
* `idsrv`: This cookie might contain information related to the user's authentication session. The exact contents can vary based on the IdentityServer configuration and what the application needs. It could include tokens or other data necessary for maintaining the user's authenticated state.

[https://identityserver4.readthedocs.io/en/latest/topics/signin.html?highlight=idsrv](https://identityserver4.readthedocs.io/en/latest/topics/signin.html?highlight=idsrv)\
\
[https://hbothra22.medium.com/got-cookies-cookie-based-authentication-vulnerabilities-in-wild-55fa7c374be0](https://hbothra22.medium.com/got-cookies-cookie-based-authentication-vulnerabilities-in-wild-55fa7c374be0)\
