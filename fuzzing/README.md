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

# Fuzzing

## Detecting Anomalies

You’ll typically be analyzing hundreds or thousands of responses, not just two or three. Therefore, you need to filter your responses to detect anomalies.

For example, if you send input like `~'!@#$%^&*()-_+` to an endpoint that improperly handles it, you could receive an error.

if you issue 100 API requests and 98 of those result in an HTTP 200 response code with a similar response size, you can consider those requests to be your baseline. Also, examine a few of the baseline responses to get a sense of their content. Once you know that the baseline responses have been properly handled, review the two anomalous responses.

the differences between baseline and anomalous requests will be minuscule. For example, the HTTP response codes might all be identical, but a few requests might result in a response size that is a few bytes larger than the baseline responses. When small differences like this come up, use Burp Suite’s Comparer to get a side-by-side comparison of the differences within the responses. Right-click the result you’re interested in and choose Send to Comparer (Response). You can send as many responses as you’d like to Comparer, but you’ll at least need to send two.

## Fuzzing Wide and Deep

Fuzzing wide is the act of sending an input across all of an API’s unique requests in an attempt to discover a vulnerability

Fuzzing deep is the act of thoroughly testing an individual request with a variety of inputs, replacing headers, parameters, query strings, endpoint paths, and the body of the request with your payloads.

### Fuzzing Wide with Postman

I recommend using Postman to fuzz wide for vulnerabilities across an API, as the tool’s Collection Runner makes it easy to run tests against all API requests. If an API includes 150 unique requests across all the endpoints, you can set a variable to a fuzzing payload entry and test it across all 150 requests. This is particularly easy to do when you’ve built a collection or imported API requests into Postman.

For example, you might use this strategy to test whether any of the requests fail to handle various “bad” characters. Send a single payload across the API and check for anomalies

Create a Postman environment in which to save a set of fuzzing variables. This lets you seamlessly use the environmental variables from one collection to the next. Once the fuzzing variables are set, you can save or update the environment.

### Fuzzing Deep with Burp Suite

### Fuzzing Deep with Wfuzz

```
wfuzz -z file,/home/hapihacker/big-list-of-naughty-strings.txt -H "Content-Type: application/
json" -H "x-access-token: [...]" -p 127.0.0.1:8080 --hc 400 -X PUT -d "{
\"user\": \"FUZZ\",
\"pass\": \"FUZZ\",
\"id\": \"FUZZ\",
\"name\": \"FUZZ\",
\"is_admin\": \"FUZZ\",
\"account_balance\": \"FUZZ\"
}" -u http://192.168.195.132:8090/api/user/edit_info
```

### Fuzzing Wide for Improper Assets Management

Improper assets management vulnerabilities arise when an organization exposes APIs that are either retired, in a test environment, or still in development. In any of these cases, there is a good chance the API has fewer protections than its supported production counterparts. Improper assets management might affect only a single endpoint or request, so it’s often useful to fuzz wide to test if improper assets management exists for any request across an API.

Also, check any sort of changelog or GitHub repository. A changelog that says something along the lines of “resolved broken object level authorization vulnerability in v3” will make finding an endpoint still using v1 or v2 all the sweeter.

The production version of the sample API is v2, so it would be a good idea to test a few keywords, like v1, v3, test, mobile, uat, dev, and old, as well as any interesting paths discovered during analysis or reconnaissance test- ing.

Additionally, some API providers will allow access to administrative functionality by adding /internal/ to the path before or after the versioning, which would look like this: /api/v2/internal/users /api/internal/v2/users

### Testing Request Methods with Wfuzz

```
wfuzz -z list,GET-HEAD-POST-PUT-PATCH-TRACE-OPTIONS-CONNECT- -X FUZZ http://testsite.com/api/
v2/account
```

### Fuzzing “Deeper” to Bypass Input Sanitization

Tricking the server’s input sanitization code into processing a payload it should normally restrict. There are a few tricks you could use against restricted fields.

First, try sending something that takes the form of the restricted field (if it’s an email field, include a valid-looking email), add a null byte, and then add another payload position for fuzzing payloads to be inserted. Here’s an example:&#x20;

`"user": "a@b.com%00§test§"`&#x20;

Instead of a null byte, try sending a pipe (|), quotes, spaces, and other escape symbols. Better yet, there are enough possible symbols to send that you could add a second payload position for typical escape characters, like this:

`"user": "a@b.com§escape§§test§"`

Use a set of potential escape symbols for the `§escape§` payload and the payload you want to execute as the `§test§`. To perform this test, use Burp Suite’s cluster bomb attack, which will cycle through multiple payload lists and attempt every other payload against it

## Fuzzing for Directory Traversal

Another weakness you can fuzz for is directory traversal. Also known as path traversal, directory traversal is a vulnerability that allows an attacker to direct the web application to move to a parent directory using some form of the expression ../ and then read arbitrary files.

If you’re able to exit the API path, you may be able to access sensitive information such as application logic, usernames, passwords, and additional personally identifiable information (like names, phone numbers, emails, and addresses).

Directory traversal can be conducted using both wide and deep fuzzing techniques. Ideally, you would fuzz deeply across all of an API’s requests, but since this can be an enormous task, try fuzzing wide and then focus- ing in on specific request values. Make sure to enrich your payloads with information collected from reconnaissance, endpoint analysis, and API responses containing errors or other information disclosures.
