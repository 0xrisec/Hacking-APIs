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

# Effective Fuzzing

All of the following could result in interesting responses:&#x20;

1. Sending an exceptionally large number when a small number is expected
2. Sending database queries, system commands, and other code&#x20;
3. Sending a string of letters when a number is expected&#x20;
4. Sending a large string of letters when a small string is expected&#x20;
5. Sending various symbols (-\_!@#$%^&\*();':''|,./?>)&#x20;
6. Sending characters from unexpected languages (漢, さ, Ж, Ѫ, Ѭ, Ѧ, Ѩ, Ѯ)

## Choosing Fuzzing Payloads

Generic payloads are those we’ve discussed so far and contain symbols, null bytes, directory traversal strings, encoded characters, large numbers, long strings, and so on.

Targeted fuzzing payloads are aimed at provoking a response from spe- cific technologies and types of vulnerabilities. Targeted fuzzing payload types might include API object or variable names, cross-site scripting (XSS) payloads, directories, file extensions, HTTP request methods, JSON or XML data, SQL or No SQL commands, or commands for particular operat- ing systems.

> Targeted fuzzing payloads are more useful once you know the technologies being used. If you’re sending SQL fuzzing payloads to an API that leverages only NoSQL databases, your testing won’t be as effective.

## Payloads:&#x20;

SecList: [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists)\
its `big-list-of-naughty-strings.txt` wordlist is excellent at causing useful responses.

FuzzDB: [https://github.com/fuzzdb-project/fuzzdb](https://github.com/fuzzdb-project/fuzzdb)

Wfuzz: [https://github.com/xmendez/wfuzz](https://github.com/xmendez/wfuzz) includes a great list that combines several targeted payloads in their injection directory, called All\_attack.txt.\
\
Additionally, you can always quickly and easily create your own generic fuzzing payload list. In a text file, combine symbols, numbers, and characters to create each payload as line-separated entries, like this:&#x20;

```
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
9999999999999999999999999999999999999999
~'!@#$%^&*()-_+
{}[]|:''; '<>?,./
%00 
0x00 
$ne 
%24ne 
$gt 
%24gt 
|whoami
'' '
OR 1=1-- -
'' ''''''
漢, さ, Ж, Ѫ, Ѭ, Ѧ, Ѩ, Ѯ
😀 😃 😄 😁 😆 
```

Note that instead of 40 instances of A or 9, you could write payloads consisting of hundreds of them. Using a small list like this as a fuzzing payload can cause all sorts of useful and interesting responses from an API.

## Detecting Anomalies

You’ll typically be analyzing hundreds or thousands of responses, not just two or three. Therefore, you need to filter your responses in order to detect anomalies. One way to do this is to understand what ordinary responses look like. You can establish this baseline by sending a set of expected requests or, as you’ll see later in the lab, by sending requests that you expect to fail. Then you can review the results to see if a majority of them are identical. For example, if you issue 100 API requests and 98 of those result in an HTTP 200 response code with a similar response size, you can consider those requests to be your baseline. Also examine a few of the baseline responses to get a sense of their content. Once you know that the baseline responses have been prop- erly handled, review the two anomalous responses. Figure out what input caused the difference, paying particular attention to the HTTP response code, response size, and the content of the response.

