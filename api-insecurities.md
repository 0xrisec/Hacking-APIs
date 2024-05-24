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

# API Insecurities

## Information Disclosure

When an API and its supporting software share sensitive information with unprivileged users, the API has an information disclosure vulnerability. Generally, any information that can help us find more severe vulnerabili- ties or assist in exploitation can be considered an information disclosure vulnerability.



Another common information disclosure issue involves verbose messag- ing. Error messaging helps API consumers troubleshoot their interactions with an API and allows API providers to understand issues with their appli- cation.

a site that is using the WordPress API may unknowingly be sharing user information with anyone who navigates to the API path /wp-json/wp/v2/users, which returns all the WordPress usernames, or “slugs.”

The following information can also be leveraged in an attack: software packages, operating system information, system logs, and software bugs

## Broken Object Level Authorization

One of the most prevalent vulnerabilities in APIs is broken object level authori- zation (BOLA). BOLA vulnerabilities occur when an API provider allows an API consumer access to resources they are not authorized to access. If an API endpoint does not have object-level access controls, it won’t perform checks to make sure users can only access their own resources. When these controls are missing, User A will be able to successfully request User B’s resources.

In general, you can test for BOLAs by understanding how an API’s resources are structured and attempting to access resources you shouldn’t be able to access. By detecting patterns within API paths and parameters, you should be able to predict other potential resources.

The bolded ele- ments in the following API requests should catch your attention: GET /api/resource/1 GET /user/account/find?user\_id=15 POST /company/account/Apple/balance POST /admin/pwreset/account/90 In these instances, you can probably guess other potential resources, like the following, by altering the bolded values: GET /api/resource/3 GET /user/account/find?user\_id=23 POST /company/account/Google/balance POST /admin/pwreset/account/111

## Broken User Authentication

## Excessive Data Exposure

## Lack of Resources and Rate Limiting

## Broken Function Level Authorization

Broken function level authorization (BFLA) is a vulnerability where a user of one role or group is able to access the API functionality of another role or group.

A BFLA is present if you are able to use the functionality of another privilege level or group. In other words, BFLA can be a lateral move, where you use the functions of a similarly privileged group, or it could be a privi- lege escalation, where you are able to use the functions of a more privileged group.

BFLA is similar to BOLA, except instead of an authorization problem involving accessing resources, it is an authorization problem for performing actions. For example, consider a vulnerable banking API. When a BOLA vul- nerability is present in the API, you might be able to access the information of other accounts, such as payment histories, usernames, email addresses, and account numbers. If a BFLA vulnerability is present, you might be able to transfer money and actually update the account information. BOLA is about unauthorized access, whereas BFLA is about unauthorized actions.

60 Chapter 3 If an API has different privilege levels or roles, it may use different endpoints to perform privileged actions. For example, a bank may use the /{user}/account/balance endpoint for a user wishing to access their account information and the /admin/account/{user} endpoint for an administrator wishing to access user account information. If the application does not have access controls implemented correctly, we’ll be able to perform administra- tive actions, such as seeing a user’s full account details, by simply making administrative requests.

The easiest way to discover BFLA is to find administrative API docu- mentation and send requests as an unprivileged user that test admin func- tions and capabilities..



## Mass Assignment

Mass assignment occurs when an API consumer includes more parameters in their requests than the application intended and the application adds these parameters to code variables or internal objects. In this situation, a con- sumer may be able to edit object properties or escalate privileges.

Imagine an API is called to create an account with parameters for "User" and "Password": { "User": "scuttleph1sh", "Password": "GreatPassword123" } While reading the API documentation regarding the account creation process, suppose you discover that there is an additional key, "isAdmin", that consumers can use to become administrators. You could use a tool like Postman or Burp Suite to add the attribute to a request and set the value to true: { "User": "scuttleph1sh", "Password": "GreatPassword123", "isAdmin": true } If the API does not sanitize the request input, it is vulnerable to mass assignment, and you could use the updated request to create an admin account.

## Security Misconfigurations

Security misconfigurations include all the mistakes developers could make within the supporting security configurations of an API. If a security mis- configuration is severe enough, it can lead to sensitive information expo- sure or a complete system takeover. For example, if the API’s supporting security configuration reveals an unpatched vulnerability, there is a chance that an attacker could leverage a published exploit to easily “pwn” the API and its system.

Security misconfigurations are really a set of weaknesses that includes misconfigured headers, misconfigured transit encryption, the use of default accounts, the acceptance of unnecessary HTTP methods, a lack of input sanitization, and verbose error messaging.

A lack of input sanitization can allow attackers to upload malicious pay- loads to the server. APIs often play a key role in automating processes, so imagine being able to upload payloads that the server automatically pro- cesses into a format that could be remotely executed or executed by an unsuspecting end user. For example, if an upload endpoint was used to pass uploaded files to a web directory, it could allow the upload of a script. Navigating to the URL where the file is located could launch the script, resulting in direct shell access to the web server. Additionally, lack of input sanitization can lead to unexpected behavior on the part of the application.

API providers use headers to provide the consumer with instructions for handling the response and security requirements. Misconfigured head- ers can result in sensitive information disclosure, downgrade attacks, and cross-site scripting attacks.

For example, take the following response:&#x20;

HTTP/ 200 OK&#x20;

\--snip-- X-\
Powered-By: VulnService 1.11 \
X-XSS-Protection: 0 \
X-Response-Time: 566.43\


The X-Powered-By header reveals backend technology. Headers like this one will often advertise the exact supporting service and its version. You could use information like this to search for exploits published for that version of software.

X-XSS-Protection is exactly what it looks like: a header meant to prevent cross-site scripting (XSS) attacks. XSS is a common type of injection vul- nerability where an attacker can insert scripts into a web page and trick end users into clicking malicious links.

An X-XSS-Protection value of 0 indicates no protections are in place, and a value of 1 indicates that protection is turned on. This header, and others like it, clearly reveals whether a security control is in place.

The X-Response-Time header is middleware that provides usage metrics. In the previous example, its value represents 566.43 milliseconds. However, if the API isn’t configured properly, this header can function as a side channel used to reveal existing resources. If the X-Response-Time header has a consistent response time for nonexistent records, for example, but increases its response time for certain other records, this could be an indication that those records exist.

Here’s an example:

HTTP/UserA 404 Not Found \
\--snip-- \
X-Response-Time: 25.5 \
\
HTTP/UserB 404 Not Found \
\--snip-- \
X-Response-Time: 25.5 \
\
HTTP/UserC 404 Not Found \
\--snip-- \
X-Response-Time: 510.00

UserC has a response time value that is 20 times the response time of the other resources. With this small sample size, it is hard to definitively conclude that UserC exists. However, imagine you have a sam- ple of hundreds or thousands of requests and know the average X-Response -Time values for certain existing and nonexistent resources. Say, for instance, you know that a bogus account like /user/account/thisdefinitelydoesnotexist876 has an average X-Response-Time of 25.5 ms. You also know that your existing account /user/account/1021 receives an X-Response-Time of 510.00. If you then sent requests brute-forcing all account numbers from 1000 to 2000, you could review the results and see which account numbers resulted in drasti- cally increased response times.



You can detect several of these security misconfigurations with web application vulnerability scanners such as Nessus, Qualys, OWASP ZAP, and Nikto.



## Injections

Injection flaws exist when a request is passed to the API’s supporting infra- structure and the API provider doesn’t filter the input to remove unwanted characters (a process known as input sanitization). As a result, the infrastruc- ture might treat data from the request as code and run it. When this sort of flaw is present, you’ll be able to conduct injection attacks such as SQL injec- tion, NoSQL injection, and system command injection.

## Improper Assets Management

Improper assets management takes place when an organization exposes APIs that are either retired or still in development. As with any software, old API versions are more likely to contain vulnerabilities because they are no lon- ger being patched and upgraded. Likewise, APIs that are still being devel- oped are typically not as secure as their production API counterparts.\
\
You can discover improper assets management by paying close attention to outdated API documentation, changelogs, and version history on reposi- tories. For example, if an organization’s API documentation has not been updated along with the API’s endpoints, it could contain references to por- tions of the API that are no longer supported. Organizations often include versioning information in their endpoint names to distinguish between older and newer versions, such as /v1/, /v2/, /v3/, and so on. APIs still in develop- ment often use paths such as /alpha/, /beta/, /test/, /uat/, and /demo/. If you know that an API is now using apiv3.org/admin but part of the API documen- tation refers to apiv1.org/admin, you could try testing different endpoints to see if apiv1 or apiv2 is still active. Additionally, the organization’s changelog may disclose the reasons why v1 was updated or retired. If you have access to v1, you can test for those weaknesses.



## Business Logic Vulnerabilities

Business logic vulnerabilities (also known as business logic flaws, or BLFs) are intended features of an application that attackers can use maliciously. For example, if an API has an upload feature that doesn’t validate encoded payloads, a user could upload any file as long as it was encoded. This would allow end users to upload and execute arbitrary code, including malicious payloads. Vulnerabilities of this sort normally come about from an assumption that API consumers will follow directions, be trustworthy, or only use the API in a certain way. In those cases, the organization essentially depends on trust as a security control by expecting the consumer to act benevolently.

You can search API documentation for telltale signs of business logic vulnerabilities. Statements like the following should illuminate the light- bulb above your head: “Only use feature X to perform function Y.” “Do not do X with endpoint Y.” “Only admins should perform request X.” These statements may indicate that the API provider is trusting that you won’t do any of the discouraged actions, as instructed. When you attack their API, make sure to disobey such requests to test for the presence of security controls. Another business logic vulnerability comes about when developers assume that consumers will exclusively use a browser to interact with the web application and won’t capture API requests that take place behind the scenes. All it takes to exploit this sort of weakness is to intercept requests with a tool like Burp Suite Proxy or Postman and then alter the API request before it is sent to the provider. This could allow you to capture shared API keys or use parameters that could negatively impact the security of the application.

As an example, consider a web application authentication portal that a user would normally employ to authenticate to their account. Say the web application issued the following API request: POST /api/v1/login HTTP 1.1 Host: example.com --snip-- UserId=hapihacker\&password=arealpassword!\&MFA=true There is a chance that we could bypass multifactor authentication by simply altering the parameter MFA to false.

