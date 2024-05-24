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

# Finding Information Disclosures

Anything that helps our exploitation of an API can be considered an information disclosure, whether it’s interesting status codes, headers, or user data.

When making requests, you should review responses for software information, usernames, email addresses, phone numbers, password requirements, account numbers, partner company names, and any information that your target claims is useful.

## Request Headers

Headers can accidentally expose too much information about the application.\
\
Some, like X-powered-by, do not serve much of a purpose and often disclose information about the backend. \[it can help us know what sort of payload to craft and reveal potential application weaknesses.]

## Status Code

Status codes can inadvertently reveal important details. By attempting to access various API endpoints and observing the responses, particularly the 404 Not Found and 401 Unauthorized status codes, one can deduce the structure of an API as an unauthenticated user. This form of information leakage can become more severe if these status codes vary based on different query parameters. For instance, if you were to systematically test query parameters such as a customer's phone number, account number, and email address, you could distinguish between non-existent and existent values based on the 404 and 401 responses, respectively. The implications of this are significant. It enables tactics like password spraying, exploiting password reset features, or launching phishing, vishing, and smishing attacks. Furthermore, by combining query parameters, it might be possible to infer personally identifiable information based solely on the unique status codes returned.

## Error analysis

If the API requires you to POST a username and password in order to obtain an API token, check how the provider responds to both existing and nonexistent usernames. A common way to respond to nonexistent usernames is with the error “User does not exist, please provide a valid username.” When a user does exist but you’ve used the wrong password, you may get the error “Invalid password.” This small difference in error response is an informa- tion disclosure that you can use to brute-force usernames, which can then be leveraged in later attacks.

