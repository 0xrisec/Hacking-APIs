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

# Finding Mass Assignment Targets

One of the most common places to discover and exploit mass assignment vulnerabilities is in API requests that accept and process client input. Account registration, profile editing, user management, and client management are all common functions that allow clients to submit input using the API.

## Account Registration

A common version of this attack is to upgrade an account to an administrator role by adding a variable that the API provider likely uses to identify admins:

```
POST /api/v1/register
--snip--
{
"username":"hAPI_hacker",
"email":"hapi@hacker.com",
"admin": true,
"password":"Password1!"
}
```

If the API provider uses this variable to update account privileges on the backend and accepts additional input from the client, this request will turn the account being registered into an admin-level account.



## Unauthorized Access to Organizations

Mass assignment attacks go beyond making attempts to become an admin- istrator. You could also use mass assignment to gain unauthorized access to other organizations, for instance. If your user objects include an organiza- tional group that allows access to company secrets or other sensitive infor- mation, you can attempt to gain access to that group.

```
POST /api/v1/register
--snip--
{
"username":"hAPI_hacker",
"email":"hapi@hacker.com",
"org": "§CompanyA§",
"password":"Password1!"
}
```

Do not limit your search for mass assignment vulnerabilities to the account registration process. Other API functions are capable of being vulnerable. Test other endpoints used for resetting passwords; updating account, group, or company profiles; and any other plays where you may be able to assign yourself additional access.

## Finding Mass Assignment Variables

### Finding Variables in Documentation

Begin by looking for sensitive variables in the API the documentation, especially in sections focused on privileged actions. In particular, the docu- mentation can give you a good indication of what parameters are included within JSON objects.

### Fuzzing Unknown Variables

Build up your wordlist with the parameters you collect from an endpoint and then flex your fuzzing skills by submitting requests with those parameters included. Account creation is a great place to do this, but don’t limit yourself to it.

### Blind Mass Assignment Attacks

If you cannot find variable names in the locations discussed, you could perform a blind mass assignment attack. In such an attack, you’ll attempt to brute-force possible variable names through fuzzing. Send a single request with many possible variables, like the following, and see what sticks:

```
POST /api/v1/register
--snip--
{
"username":"hAPI_hacker",
"email":"hapi@hacker.com",
"admin": true,
"admin":1,
"isadmin": true,
"role":"admin",
"role":"administrator",
"user_priv": "admin",
"password":"Password1!"
}
```

### Automating Mass Assignment Attacks with Arjun and Burp Suite Intruder

As with many other API attacks,, you can discover mass assignment by man- ually altering an API request or by using a tool such as Arjun for parameter fuzzing.

As a result, Arjun will send a series of requests with various param- eters from a wordlist to the target host. Arjun will then narrow down likely parameters based on deviations of response lengths and response codes and provide you with a list of valid parameters. Remember that if you run into issues with rate limiting, you can use the Arjun —stable option to slow down the scans. This sample scan completed and discovered three valid parameters: user, pass, and admin.

Many APIs prevent you from sending too many parameters in a single request. As a result, you might receive one of several HTTP status codes in the 400 range, such as 400 Bad Request, 401 Unauthorized, or 413 Payload Too Large. In that case, instead of sending a single large request, you could cycle through possible mass assignment variables over many requests. This can be done by setting up the request in Burp Suite’s Intruder with the pos- sible mass assignment values as the payload, like so:

```
POST /api/v1/register
--snip--
{
"username":"hAPI_hacker",
"email":"hapi@hacker.com",
§"admin": true§,
"password":"Password1!"
}
```

## Combining BFLA and Mass Assignment

If you’ve discovered a BFLA vulnerability that allows you to update other users’ accounts, try combining this ability with a mass assignment attack.

For example, let’s say a user named Ash has discovered a BFLA vulnerability, but the vulnerability only allows him to edit basic profile information such as usernames, addresses, cities, and regions:

```
PUT /api/v1/account/update
Token:UserA-Token
--snip--
{
"username": "Ash",
"address": "123 C St",
"city": "Pallet Town"
"region": "Kanto",
}
```

At this point, Ash could deface other user accounts, but not much more. However, performing a mass assignment attack with this request could make the BFLA finding much more significant. Let’s say that Ash analyzes other GET requests in the API and notices that other requests include parameters for email and multifactor authentication (MFA) set- tings. Ash knows that there is another user, named Brock, whose account he would like to access.

Ash could disable Brock’s MFA settings, making it easier to gain access to Brock’s account. Moreover, Ash could replace Brock’s email with his own. If Ash were to send the following request and get a successful response, he could gain access to Brock’s account:

```
PUT /api/v1/account/update
Token:UserA-Token
--snip--
{
"username": "Brock",
"address": "456 Onyx Dr",
"city": "Pewter Town",
"region": "Kanto",
"email": "ash@email.com",
"mfa": false
}
```

Since Ash does not know Brock’s current password, Ash should leverage the API’s process for performing a password reset, which would likely be a PUT or POST request sent to /api/v1/account/reset. The password reset pro- cess would then send a temporary password to Ash’s email. With MFA dis- abled, Ash would be able to use the temporary password to gain full access to Brock’s account. Always remember to think as an adversary would and take advantage of every opportunity.

If you encounter a request that accepts client input for sensitive variables and allows you to update those variables, you have a serious finding on your hands. As with other API attacks, sometimes a vulnerability may seem minor until you’ve combined it with other interesting findings. Finding a mass assignment vulnerability is often just the tip of the iceberg. If this vul- nerability is present, chances are that other vulnerabilities are present.
