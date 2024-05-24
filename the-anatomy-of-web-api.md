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

# The Anatomy of Web API

## Understanding API Endpoints

An API consumer can request resources from an API endpoint, which is a URL for interacting with a part of the API. Each of the following examples is a different API endpoint:&#x20;

```
https://example.com/api/v3/users/ 
https://example.com/api/v3/customers/ 
https://example.com/api/updated_on/ 
https://example.com/api/state/1/ 
```

#### Types of Resources

In the context of APIs, a "resource" refers to any type of data or service that can be accessed through an API endpoint. Resources can be categorized as follows:

* **Singleton Resource**: This represents a single, specific entity. For example, `/api/user/{user_id}` would access the data for a user with a specific `user_id`. It's a direct way to query or manipulate the state of a single object in the database.
* **Collection**: This is a group of resources. An endpoint like `/api/profiles/users` might return a list (or collection) of user profiles. Collections are useful for retrieving summary or list data, where the client can then select specific items for more detailed interaction.
* **Subcollection**: A subcollection is essentially a collection that exists within a singleton resource. For instance, `/api/user/{user_id}/settings` would access the settings (a collection) specific to a user identified by `{user_id}`. This allows for more granular control and organization of related data within a larger resource.

Endpoints are designed to handle various HTTP methods such as GET (retrieve data), POST (submit data), PUT (update data), DELETE (remove data), etc., depending on what actions the API allows for a given resource.

## What is API gateway?

An API Gateway is a server that acts as an intermediary between clients and backend services, providing a unified interface for clients to access multiple services through a single endpoint. The API gateway filters bad requests, monitors incoming traffic, and routes each request to the proper service or microservice. The API gateway can also handle security controls such as authentication, authorization, encryption in transit using SSL, rate limiting, and load balancing.

## Microservice&#x20;

A microservice is a modular piece of a web app that handles a specific function. Microservices use APIs to transfer data and trigger actions.&#x20;

For example, a web application with a payment gateway may have several different features on a single web page: a billing feature, a feature that logs customer account information, and one that emails receipts upon purchase. The application’s backend design could be monolithic, meaning all the services exist within a single application, or it could have a microservice architecture, where each service functions as its standalone application.

{% hint style="info" %}
From an API hacker’s perspective, the documentation can reveal which endpoints to call for customer data, which API keys you need to become an administrator, and even business logic flaws.
{% endhint %}

## Standard Web API Types

### RESTful APIs

Representational State Transfer (REST) is a set of architectural constraints for applications that communicate using HTTP methods. APIs that use REST constraints are called RESTful (or just REST) APIs.

REST was designed to improve upon many of the inefficiencies of other older APIs, such as Simple Object Access Protocol (SOAP).\
For example, it relies entirely on the use of HTTP, which makes it much more approachable to end users. REST APIs primarily use the HTTP methods GET, POST, PUT, and DELETE to accomplish CRUD

REST is a style rather than a protocol, so each RESTful API may be . It may have methods enabled beyond CRUD, its own sets of authen- tication requirements, subdomains instead of paths for endpoints, different rate-limit requirements, and so on. Furthermore, developers or an orga- nization may call their API “RESTful” without adhering to the standard, which means you can’t expect every API you come across to meet all the REST constraints.

### GraphQL

Short for Graph Query Language, GraphQL is a specification for APIs that allow clients to define the structure of the data they want to request from the server. GraphQL is RESTful, as it follows the six constraints of REST APIs. However, GraphQL also takes the approach of being query-centric, because it is structured to function similarly to a database query language like Structured Query Language (SQL).

GraphQL improves on typical REST APIs in several ways. Since REST APIs are resource based, there will likely be instances when a consumer needs to make several requests in order to get all the data they need. On the other hand, if a consumer only needs a specific value from the API pro- vider, the consumer will need to filter out the excess data. With GraphQL, a consumer can use a single request to get the exact data they want. That’s because, unlike REST APIs, where clients receive whatever data the server is programmed to return from an endpoint, including the data they don’t need, GraphQL APIs let clients request specific fields from a resource.

## API Data Interchange Formats

JSON\
XML\
YAML\


## API Authentication

### Basic Authentication

The simplest form of API authentication is HTTP basic authentication, in which the consumer includes their username and password in a header or the body of a request. The API could either pass the username and pass- word to the provider in plaintext, like username:password, or it could encode the credentials using something like base64 to save space (for example, as dXNlcm5hbWU6cGFzc3dvcmQK). Encoding is not encryption, and if base64-encoded data is captured, it can easily be decoded. For example, you can use the Linux command line to base64-encode username:password and then decode the encoded result:

### API Keys

API keys are unique strings that API providers generate and grant to authorize access for approved consumers. Once an API consumer has a key, they can include it in requests whenever specified by the provider. The provider will typically require that the consumer pass the key in query string parameters, request headers, body data, or as a cookie when they make a request.&#x20;

API keys typically look like semi-random or random strings of numbers and letters. For example, take a look at the API key included in the query string of the following URL:&#x20;

```
/api/v1/users?apikey=ju574n3x4mpl34p1k3y 
```

The following is an API key included as a header:&#x20;

```
"API-Secret": "17813fg8-46a7-5006-e235-45be7e9f2345" 
```

Finally, here is an API key passed in as a cookie:&#x20;

```
Cookie: API-Key= 4n07h3r4p1k3y
```

Since each API provider may have their own system for generating API keys, you’ll find instances in which the API key is generated based on user data.&#x20;

{% hint style="info" %}
API hackers may guess or forge API keys by learning about the API consumers. API keys may also be exposed to the internet in online repositories, left in code comments, intercepted when transferred over unencrypted connections, or stolen through phishing.
{% endhint %}

### JSON Web Tokens

#### HMAC

A hash-based message authentication code (HMAC) is the primary API authentication method used by Amazon Web Services (AWS). When using HMAC, the provider creates a secret key and shares it with consumer. When a consumer interacts with the API, an HMAC hash function is applied to the consumer’s API request data and secret key. The resulting hash (also called a message digest) is added to the request and sent to the provider.

#### OAuth 2.0

OAuth 2.0, or just OAuth, is an authorization standard that allows different services to access each other’s data, often using APIs to facilitate the service-to-service communications.

Let’s say you want to automatically share your Twitter tweets on LinkedIn. In OAuth’s model, we would consider Twitter to be the service provider and LinkedIn to be the application or client. In order to post your tweets, LinkedIn will need authorization to access your Twitter information. Since both Twitter and LinkedIn have implemented OAuth, instead of providing your credentials to the service provider and consumer every time you want to share this information across platforms, you can simply go into your LinkedIn settings and authorize Twitter. Doing so will send you to api.twitter.com to authorize LinkedIn to access your Twitter account.

When you authorize LinkedIn to access your Twitter posts, Twitter generates a limited, time-based access token for LinkedIn. LinkedIn then provides that token to Twitter to post on your behalf, and you don’t have to give LinkedIn your Twitter credentials.
