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

# Endpoint analysis

## Finding Request Information

API hacking relies on the backend operations of those items that are found in the GUI—namely, GET requests with query param- eters and most POST/PUT/UPDATE/DELETE requests.

Before you craft requests to an API, you’ll need an understanding of its endpoints, request parameters, necessary headers, authentication require- ments, and administrative functionality.

### Finding Information in Documentation

First, try using your Google hacking skills to find it on search engines and in other recon tools. Second

Second, use the [Wayback Machine](https://web.archive.org/). If your target once posted their API documentation publicly and later retracted it, there may be an archive of their docs available.

Third, when permitted, try social engineering techniques to trick an organization into sharing its documentation.

When the documentation is not publicly available, try creating an account and searching for the documentation while authenticated.

If you still cannot find the docs, A couple of API wordlists on GitHub that can help you discover API documentation through the use of a fuzzing technique called directory brute force: [https://github.com/hAPI-hacker/Hacking-APIs](https://github.com/hAPI-hacker/Hacking-APIs)

#### Documentation Analysis:

Normally found at the beginning of the doc, the overview will provide a high-level introduction to how to connect and use the API. In addition, it could contain information about authentication and rate limiting.

Review the documentation for functionality, or the actions that you can take using the given API. These will be represented by a combination of an HTTP method (GET, PUT, POST, DELETE) and an endpoint. Every orga- nization’s APIs will be different, but you can expect to find functionality related to user account management, options to upload and download data, different ways to request information, and so on.

When making a request to an endpoint, make sure you note the request requirements. Requirements could include some form of authentication, parameters, path variables, headers, and information included in the body of the request.

### Importing API Specifications

If your target has a specification, in a format like OpenAPI (Swagger), RAML, or API Blueprint or in a Postman collection, finding this will be even more useful than finding the documentation.

To import the specification, begin by launching Postman.

/swagger.json

/swagger/ui/index

### Reverse Engineering APIs

#### Building a Postman Collection by Proxy

#### Adding API Authentication Requirements to Postman



## Analyzing Functionality

You’ll begin by using the API as it was intended. In the process, you’ll pay attention to the responses and their status codes and error messages. In particular, you’ll seek out functional- ity that interests you as an attacker, especially if there are indications of information disclosure, excessive data exposure, and other low-hanging vulnerabilities.

Look for endpoints that could provide you with sensitive information, requests that allow you to interact with resources, areas of the API that allow you to inject a payload, and administrative actions. Beyond that, look for any endpoint that allows you to upload your own payload and interact with resources.

### Testing Intended Use

As you proceed, ask yourself these questions:&#x20;

• What sorts of actions can I take? \
• Can I interact with other user accounts? \
• What kinds of resources are available? \
• When I create a new resource, how is that resource identified? \
• Can I upload a file? Can I edit a file?

### Performing Privileged Actions

Privileged actions will often lead to additional functionality, information, and control. For example, admin requests could give you the ability to create and delete users, search for sensitive user information, enable and disable accounts, add users to groups, manage tokens, access logs, and more.

My recommendation is to test these actions in several phases: first as an unau- thenticated user, then as a low-privileged user, and finally as an administra- tive user. When you make the administrative requests as documented but without any authorization requirements, you should receive some sort of unauthorized response if any security controls are in place.

### Analyzing API Responses

One of the most basic skills you’ll need as an API hacker is the ability to analyze the responses you receive. This is initially done by issuing a request and review- ing the response status code, headers, and content included in the body.

First check that you are receiving the responses you expect. API docu- mentation can sometimes provide examples of what you could receive as a response. However, once you begin using the API in unintended ways, you will no longer know what you’ll get as a response, which is why it helps to first use the API as it was intended before moving into attack mode. Developing a sense of regular and irregular behavior will make vulnerabilities obvious.





