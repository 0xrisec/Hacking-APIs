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

## Effective Fuzzing

All of the following could result in interesting responses: • Sending an exceptionally large number when a small number is expected • Sending database queries, system commands, and other code • Sending a string of letters when a number is expected • Sending a large string of letters when a small string is expected • Sending various symbols (-\_!@#$%^&\*();':''|,./?>) • Sending characters from unexpected languages (漢, さ, Ж, Ѫ, Ѭ, Ѧ, Ѩ, Ѯ)

Targeted fuzzing payload types might include API object or variable names, cross-site scripting (XSS) payloads, directories, file extensions, HTTP request methods, JSON or XML data, SQL or No SQL commands, or commands for particular operat- ing systems.

[https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)\
its big-list-of-naughty-strings.txt wordlist is excellent at causing useful responses.\
[https://github.com/fuzzdb-project/fuzzdb](https://github.com/fuzzdb-project/fuzzdb)\
[https://github.com/xmendez/wfuzz](https://github.com/xmendez/wfuzz)\
including a great list that combines several targeted payloads in their injection directory, called All\_attack.txt.\
\
Additionally, you can always quickly and easily create your own generic fuzzing payload list. In a text file, combine symbols, numbers, and charac- ters to create each payload as line-separated entries, like this: AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA 9999999999999999999999999999999999999999 \~'!@#$%^&\*()-\_+ {}\[]|:''; '<>?,./ %00 0x00 $ne %24ne $gt %24gt |whoami

***

' '' ' OR 1=1-- - '' '''''' 漢, さ, Ж, Ѫ, Ѭ, Ѧ, Ѩ, Ѯ 😀 😃 😄 😁 😆 Note that instead of 40 instances of A or 9, you could write payloads consisting of hundreds them. Using a small list like this as a fuzzing payload can cause all sorts of useful and interesting responses from an API.

## Detecting Anomalies

You’ll typically be analyzing hundreds or thousands of responses, not just two or three. Therefore, you need to filter your responses in order to detect anomalies.

or example, if you sent input like \~'!@#$%^&\*()-\_+ to an endpoint that improperly handles it, you could receive an error.

if you issue 100 API requests and 98 of those result in an HTTP 200 response code with a similar response size, you can consider those requests to be your baseline. Also examine a few of the baseline responses to get a sense of their content. Once you know that the baseline responses have been prop- erly handled, review the two anomalous responses.

the differences between baseline and anomalous requests will be miniscule. For example, the HTTP response codes might all be identical, but a few requests might result in a response size that is a few bytes larger than the baseline responses. When small differences like this come up, use Burp Suite’s Comparer to get a side-by-side comparison of the differences within the responses. Right-click the result you’re inter- ested in and choose Send to Comparer (Response). You can send as many responses as you’d like to Comparer, but you’ll at least need to send two.

## Fuzzing Wide and Deep

Fuzzing wide is the act of sending an input across all of an API’s unique requests in an attempt to discover a vulnerability

Fuzzing deep is the act of thoroughly testing an individual request with a variety of inputs, replacing headers, parameters, query strings, endpoint paths, and the body of the request with your payloads.

### Fuzzing Wide with Postman

I recommend using Postman to fuzz wide for vulnerabilities across an API, as the tool’s Collection Runner makes it easy to run tests against all API requests. If an API includes 150 unique requests across all the endpoints, you can set a variable to a fuzzing payload entry and test it across all 150 requests. This is particularly easy to do when you’ve built a collection or imported API requests into Postman.

For example, you might use this strategy to test whether any of the requests fail to handle various “bad” characters. Send a single payload across the API and check for anomalies

Create a Postman environment in which to save a set of fuzzing vari- ables. This lets you seamlessly use the environmental variables from one collection to the next. Once the fuzzing variables are set, you can save or update the environment.

