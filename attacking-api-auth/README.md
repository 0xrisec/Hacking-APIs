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

# Attacking API auth

## Recon

List all the login pages with IP address URLs, signup pages, leaked authentication details, and leaked tokens.

## Classic Authentication Attacks

### Password Brute-Force Attacks

To perform the brute-force attack once you have a suitable wordlist, you can use tools such as Burp Suite’s brute forcer or Wfuzz.

```
wfuzz -c -z file,passwords --hh 0 -H "Content-Type: application/json" -d '{"email":"ajit@gmail.com","password":"FUZZ"}' http://crapi.apisec.ai/identity/api/auth/login
```

#### Creating targeted password lists

[https://github.com/Mebus/cupp](https://github.com/Mebus/cupp)\
[https://github.com/sc0tfree/mentalist](https://github.com/sc0tfree/mentalist)

#### Others

{% embed url="https://docs.rapid7.com/metasploit/bruteforce-attacks/" %}

{% embed url="https://github.com/foospidy/payloads" %}

### Password Reset and Multifactor Authentication Brute-Force Attacks

While you can apply brute-force techniques directly to the authentication requests, you can also use them against password reset and multifactor authentication (MFA) functionality. If a password reset process includes security questions and does not apply rate limiting to requests, we can target it in such an attack.

Like GUI web applications, APIs often use SMS recovery codes or one time passwords (OTPs) in order to verify the identity of a user who wants to reset their password. Additionally, a provider may deploy MFA to successful authentication attempts, so you’ll have to bypass that process to gain access to the account. On the backend, an API often implements this functionality using a service that sends a four-to-six-digit code to the phone number or email associated with the account. If we’re not stopped by rate limiting, we should be able to brute-force these codes to gain access to the targeted account.

### Password Spraying

Many security controls could prevent you from successfully brute-forcing an API’s authentication. A technique called password spraying can evade many of these controls by combining a long list of users with a short list of tar- geted passwords.

Let’s say you know that an API authentication process has a lockout policy in place and will only allow 10 login attempts. You could craft a list of the nine most likely passwords (one less password than the limit) and use these to attempt to log in to many user accounts.

When you’re password spraying, large and outdated wordlists like rockyou.txt won’t work. There are way too many unlikely passwords in such a file to have any success. Instead, craft a short list of likely passwords, taking into account the constraints of the API provider’s password policy, which you can discover during reconnaissance. Most password policies likely require a minimum character length, upper- and lowercase letters, and perhaps a number or special character.

Try mixing your password-spraying list with two types of path of small- resistance (POS) passwords, or passwords that are simple enough to guess but complex enough to meet basic password requirements (generally a minimum of eight characters, a symbol, upper- and lowercase letters, and a number). The first type includes obvious passwords like QWER!@#$, Password1!, and the formula Season+Year+Symbol (such as Winter2021!, Spring2021?, Fall2021!, and Autumn2021?). The second type includes more advanced passwords that relate directly to the target, often including a capitalized letter, a number, a detail about the organization, and a symbol

The key to password spraying is to maximize your user list. The more usernames you include, the higher your odds of gaining access. Build a user list during your reconnaissance efforts or by discovering excessive data exposure vulnerabilities.

### Including Base64 Authentication in Brute-Force Attacks

Some APIs will base64-encode authentication payloads sent in an API request. There are many reasons to do this, but it’s important to know that security is not one of them. You can easily bypass this minor inconvenience.

## Forging Tokens

The problem with tokens is that they can be stolen, leaked, and forged.

APIs will often use tokens as an authorization method. A consumer may have to initially authenticate using a username and password combina- tion, but then the provider will generate a token and give that token to the consumer to use with their API requests. If the token generation process is flawed, we will be able to analyze the tokens, hijack other user tokens, and then use them to access the resources and additional API functionality of the affected users. Burp Suite’s Sequencer provides two methods for token analysis: manu- ally analyzing tokens provided in a text file and performing a live capture to automatically generate tokens.

### Manual Load Analysis

To perform a manual load analysis, select the Sequencer module and choose the Manual Load tab. Click Load and provide the list of tokens you want to analyze. The more tokens you have in your sample, the better the results will be. Sequencer requires a minimum of 100 tokens to perform a basic analysis, which includes a bit-level analysis, or an automated analysis of the token converted to sets of bits. These sets of bits are then put through a series of tests involving compression, correlation, and spectral testing, as well as four tests based on the Federal Information Processing Standard (FIPS) 140-2 security requirements.

> If you would like to follow along with the examples in this section, generate your own tokens or use the bad tokens hosted on the Hacking-APIs GitHub repo (https:// github.com/hAPI-hacker/Hacking-APIs).

The token analysis report begins with a summary of the findings. The overall results include the quality of randomness within the token sample. If you can see that the quality of randomness was extremely poor, indicating that we’ll likely be able to brute-force other existing tokens.

To minimize the effort required to brute-force tokens, we’ll want to determine if there are parts of the token that do not change and other parts that often change. Use the character position analysis to determine which characters should be brute-forced. You can find this feature under Character Set within the Character-Level Analysis tab.

As you can see, the token character positions do not change all that much, with the exception of the final three characters; the string Ab4dt0k3n remains the same throughout the sampling. Now we know we should per- form a brute force of only the final three characters and leave the remain- der of the token untouched

### Live Token Capture Analysis

Burp Suite’s Sequencer can automatically ask an API provider to generate 20,000 tokens for analysis. To do this, we simply intercept the provider’s token generation process and then configure Sequencer. Burp Suite will repeat the token generation process up to 20,000 times to analyze the tokens for similarities.

Within Sequencer, make sure you have the live capture tab selected, and under Token Location Within Response, select the Configure for the Custom Location option.

Select Start Live Capture. Burp Sequencer will now begin capturing tokens for analysis. If you select the Auto analyze checkbox, Sequencer will show the effective entropy results at different milestones.

If there are token character positions with low entropy, you can attempt a brute-force attack against those character positions. Reviewing tokens with low entropy could reveal certain patterns you could take advantage of. For example, if you noticed that characters in certain positions only contained lowercase letters, or a certain range of numbers, you’ll be able to enhance your brute-force attacks by minimizing the number of request attempts.

### Brute-Forcing Predictable Tokens

Let’s return to the bad tokens discovered during manual load analysis (whose final three characters are the only ones that change) and brute- force possible letter and number combinations to find other valid tokens. Once we’ve discovered valid tokens, we can test our access to the API and find out what we’re authorized to do.

When you’re brute-forcing through combinations of numbers and let- ters, it is best to minimize the number of variables. The character-level analysis has already informed us that the first nine characters of the token Ab4dt0k3n remain static. The final three characters are the variables, and based on the sample, we can see that they follow a pattern of letter1 + letter2 + number. Moreover, a sample of the tokens tells us that that letter1 only ever consists of letters between a and d. Observations like this will help mini- mize the total amount of brute force required.

```
wfuzz -u vulnexample.com/api/v2/user/dashboard –hc 404 -H "token: Ab4dt0k3nFUZZFUZ2ZFUZ3Z1"
-z list,a-b-c-d -z list,a-b-c-d -z range,0-9
```

## JSON Web Token Abuse

the tactics described in the last section could work against JWTs as well, these tokens can be vulnerable to several additional attacks.

> For testing purposes, you might want to generate your own JWTs. Use [https://jwt.io](https://jwt.io), a site created by Auth0, to do so. Sometimes the JWTs have been configured so improp- erly that the API will accept any JWT.

More commonly, though, you’ll register with an API and the provider will respond with a JWT. Once you have been issued a JWT, you will need to include it in all subsequent requests. If you are using a browser, this process will happen automatically.

### Recognizing and Analyzing JWTs

You should be able to distinguish JWTs from other tokens because they consist of three parts separated by periods: the header, payload, and signature

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJoYWNrYXBpcy5pbyIsImV4cCI6IDE1ODM2Mzc0ODgsInVz
ZXJuYW1lIjoiU2N1dHRsZXBoMXNoIiwic3VwZXJhZG1pbiI6dHJ1ZX0.1c514f4967142c27e4e57b612a7872003fa6c
bc7257b3b74da17a8b4dc1d2ab9
```

The first step to attacking a JWT is to decode and analyze it. If you discovered exposed JWTs during reconnaissance, stick them into a decoder tool to see if the JWT payload contains any useful information, such as username and user ID. You might also get lucky and obtain a JWT that contains username and password combinations.

In Burp Suite’s Decoder, paste the JWT into the top window, select Decode As, and choose the Base64 option.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

The header is a base64-encoded value that includes information about the type of token and hashing algorithm used for signing.

For additional information, check out the Microsoft API documentation on cryptography at[ https://docs.microsoft.com/en-us/dotnet/ api/system.security.cryptography](https://learn.microsoft.com/en-us/dotnet/api/system.security.cryptography).

To help you analyze JWTs, leverage the JSON Web Token Toolkit by using the following command:

```
jwt_tool eyghbocibiJIUZZINIISIRSCCI6IkpXUCJ9.eyIzdW1101IxMjMENTY3ODkwIiwibmFtZSI6ImhBuEkg
SGFja2VyIiwiaWFQIjoxNTE2MjM5MDIyfQ.IX-Iz_e1CrPrkel FjArExaZpp3Y2tfawJUFQaNdftFw
```

Additionally, jwt\_tool has a “Playbook Scan” that can be used to target a web application and scan for common JWT vulnerabilities. You can run this scan by using the following:

```
jwt_tool -t http://target-site.com/ -rc "Header: JWT_Token" -M pb
```

To use this command, you’ll need to know what you should expect as the JWT header. When you have this information, replace "Header" with the name of the header and "JWT\_Token" with the actual token value.

#### Wordlists

[https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list](https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list)

### The None Attack

If you ever come across a JWT using "none" as its algorithm, you’ve found an easy win. After decoding the token, you should be able to clearly see the header, payload, and signature. From here, you can alter the information contained in the payload to be whatever you’d like. For example, you could change the username to something likely used by the provider’s admin account (like root, admin, administrator, test, or adm).

Once you’ve edited the payload, use Burp Suite’s Decoder to encode the payload with base64; then insert it into the JWT. Importantly, since the algorithm is set to "none", any signature that was present can be removed. In other words, you can remove everything following the third period in the JWT. Send the JWT to the provider in a request and check whether you’ve gained unauthorized access to the API.

Set the algorithm used as `"None"` and remove the signature part.

None algorithm variants:

* none
* None
* NONE
* nOnE

Alternatively, you can modify an existing JWT (be careful with the expiration time)



**Using jwt\_tool:**

```
python3 jwt_tool.py [JWT_HERE] -X a
```

### The Algorithm Switch Attack

There is a chance the API provider isn’t checking the JWTs properly. If this is the case, we may be able to trick a provider into accepting a JWT with an altered algorithm. One of the first things you should attempt is sending a JWT without including the signature. This can be done by erasing the signature altogether and leaving the last period in place, like this:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJoYWNrYXBpcy5pbyIsImV4cCI6IDE1ODM2Mzc0ODgsInVzZ
XJuYW1lIjoiU2N1dHRsZXBoMXNoIiwic3VwZXJhZG1pbiI6dHJ1ZX0.
```

A more likely scenario than the provider accepting no algorithm is that they accept multiple algorithms. For example, if the provider uses RS256 but doesn’t limit the acceptable algorithm values, we could alter the algorithm to HS256. This is useful, as RS256 is an asymmetric encryption scheme, meaning we need both the provider’s private key and a public key in order to accurately hash the JWT signature. Meanwhile, HS256 is symmetric encryption, so only one key is used for both the signature and verification of the token. If you can discover the provider’s RS256 public key and then switch the algorithm from RS256 to HS256, there is a chance you may be able to leverage the RS256 public key as the HS256 key.

The JWT\_Tool can make this attack a bit easier. It uses the format `jwt_tool <JWT_Token> -X k -pk public-key.pem`, as shown next. You will need to save the captured public key as a file on your attacking machine.

```
jwt_tool eyJBeXAiOiJKV1QiLCJhbGciOiJSUZI1Ni19.eyJpc3MiOi JodHRwOlwvxC9kZW1vLnNqb2VyZGxhbm
drzwiwZXIubmxcLyIsIm1hdCI6MTYYCJkYXRhIjp7ImhlbGxvijoid29ybGQifx0.MBZKIRF_MvG799nTKOMgdxva
_S-dqsVCPPTR9N9L6q2_10152pHq2YTRafwACdgyhR1A2Wq7wEf4210929BTWsVk19_XkfyDh_Tizeszny_
GGsVzdb103NCITUEjFRXURJ0-MEETROOC-TWB8n6wOTOjWA6SLCEYANSKWaJX5XvBt6HtnxjogunkVz2sVp3
VFPevfLUGGLADKYBphfumd7jkh80ca2lvs8TagkQyCnXq5VhdZsoxkETHwe_n7POBISAZYSMayihlweg 
-x k-pk public-key-pem
```

Once you run the command, JWT\_Tool will provide you with a new token to use against the API provider. If the provider is vulnerable, you’ll be able to hijack other tokens, since you now have the key required to sign tokens. Try repeating the process, this time creating a new token based on other API users, especially administrative ones.

The algorithm HS256 uses the secret key to sign and verify each message. The algorithm RS256 uses the private key to sign the message and uses the public key for authentication.

If you change the algorithm from RS256 to HS256, the back-end code uses the public key as the secret key and then uses the HS256 algorithm to verify the signature.

Then, using the public key and changing RS256 to HS256 we could create a valid signature. You can retrieve the certificate of the web server executing this:

```
openssl s_client -connect example.com:443 2>&1 < /dev/null | sed -n '/-----BEGIN/,/-----END/p' > certificatechain.pem 

#For this attack you can use the JOSEPH Burp extension. 
In the Repeater, select the JWS tab and select the Key confusion attack. 
Load the PEM, Update the request and send it. 
(This extension allows you to send the "non" algorithm attack also). 
It is also recommended to use the tool jwt_tool with the option 2 
as the previous Burp Extension does not always works well.

openssl x509 -pubkey -in certificatechain.pem -noout > pubkey.pem
```



### The JWT Crack Attack

The JWT Crack attack attempts to crack the secret used for the JWT signature hash, giving us full control over the process of creating our own valid JWTs. Hash-cracking attacks like this take place offline and do not interact with the provider. Therefore, we do not need to worry about causing havoc by sending millions of requests to an API provider.

You can use JWT\_Tool or a tool like Hashcat to crack JWT secrets. You’ll feed your hash cracker a list of words. The hash cracker will then hash those words and compare the values to the original hashed signature to determine if one of those words was used as the hash secret. If you’re performing a long-term brute-force attack of every character possibility, you may want to use the dedicated GPUs that power Hashcat instead of JWT\_Tool. That being said, JWT\_Tool can still test 12 million passwords in under a minute.

To perform a JWT Crack attack using JWT\_Tool, use the following command:

```
jwt_tool <JWT Token> -C -d /wordlist.txt
```

The -C option indicates that you’ll be conducting a hash crack attack and the -d option specifies the dictionary or wordlist you’ll be using against the hash.

A natural next step would be to conduct None attacks to try to bypass the hashing algorithm. I will leave that for you to explore on your own. We won’t attempt any other algorithm-switch attack, as we’re already attacking a symmetric key encryption system, so switching the algorithm type won’t benefit us here. That leaves us with performing JWT Crack attacks

## Tamper data without modifying anything <a href="#tamper-data-without-modifying-anything" id="tamper-data-without-modifying-anything"></a>

You can just tamper with the data leaving the signature as is and check if the server is checking the signature. Try to change your username to "admin" for example.



**Is the token checked?**

To check if a JWT's signature is being verified:

* An error message suggests ongoing verification; sensitive details in verbose errors should be reviewed.
* A change in the returned page also indicates verification.
* No change suggests no verification; this is when to experiment with tampering payload claims.

## Tools

Mentalist app: [https://github.com/sc0tfree/mentalist](https://github.com/sc0tfree/mentalist)\
Common User Passwords Profiler: [https://github.com/Mebus/cupp](https://github.com/Mebus/cupp\))\
Burp Suite’s brute forcer\
Wfuzz\
jwt\_tool\
hashcat
