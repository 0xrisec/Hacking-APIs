# Table of contents

* [How web application works?](README.md)
* [The Anatomy of Web API](the-anatomy-of-web-api.md)
* [API Insecurities](api-insecurities.md)
* [Setting up an API hacking system](setting-up-an-api-hacking-system.md)
* [API Targets](api-targets.md)
* [Discovering APIs](discovering-apis/README.md)
  * [Passive Recon](discovering-apis/passive-recon.md)
  * [Active Recon](discovering-apis/active-recon.md)
  * [Craft rogue API docs](discovering-apis/craft-rogue-api-docs.md)
  * [Lab](discovering-apis/lab.md)
* [Endpoint analysis](endpoint-analysis.md)
* [Vulnerabilities](vulnerabilities/README.md)
  * [Finding Information Disclosures](vulnerabilities/finding-information-disclosures.md)
  * [Finding Security Misconfigurations](vulnerabilities/finding-security-misconfigurations.md)
  * [Finding Excessive Data Exposures](vulnerabilities/finding-excessive-data-exposures.md)
  * [Finding Business Logic Flaws](vulnerabilities/finding-business-logic-flaws.md)
* [Attacking API auth](attacking-api-auth/README.md)
  * [Types of auth](attacking-api-auth/types-of-auth.md)
  * [JWT Hacks](attacking-api-auth/jwt-hacks/README.md)
    * [The JWT Crack Attack](attacking-api-auth/jwt-hacks/the-jwt-crack-attack.md)
    * [Brute-force HMAC secret](attacking-api-auth/jwt-hacks/brute-force-hmac-secret.md)
    * [The None Attack](attacking-api-auth/jwt-hacks/the-none-attack.md)
    * [Null Signature Attack (CVE-2020-28042)](attacking-api-auth/jwt-hacks/null-signature-attack-cve-2020-28042.md)
    * [The Algorithm Switch Attack / Algorithm confusion attacks](attacking-api-auth/jwt-hacks/the-algorithm-switch-attack-algorithm-confusion-attacks.md)
    * [Obtain the server's public key](attacking-api-auth/jwt-hacks/obtain-the-servers-public-key.md)
    * [JWT Signature - Disclosure of a correct signature (CVE-2019-7644)](attacking-api-auth/jwt-hacks/jwt-signature-disclosure-of-a-correct-signature-cve-2019-7644.md)
    * [CVE-2018-0114](attacking-api-auth/jwt-hacks/cve-2018-0114.md)
  * [ASP.NET Core Cookie Authentication](attacking-api-auth/asp.net-core-cookie-authentication.md)
  * [Labs](attacking-api-auth/labs.md)
* [Fuzzing](fuzzing.md)
* [AWS](aws.md)
* [Misc](misc/README.md)
  * [Auth type](misc/auth-type.md)
  * [Brute Force - CheatSheet](misc/brute-force-cheatsheet.md)
  * [Cryptography](misc/cryptography/README.md)
    * [JWT Handbook](misc/cryptography/jwt-handbook.md)
    * [Symmetric vs asymmetric algorithms](misc/cryptography/symmetric-vs-asymmetric-algorithms.md)
    * [HS2562](misc/cryptography/hs2562.md)
    * [RS256](misc/cryptography/rs256.md)
* [Tools](tools/README.md)
  * [ffuf](tools/ffuf.md)
  * [wfuzz](tools/wfuzz.md)
  * [jwt\_tool](tools/jwt\_tool.md)
  * [Hashcat](tools/hashcat.md)
  * [Common User Passwords Profiler](tools/common-user-passwords-profiler.md)
* [Resources](resources.md)
* [Wordpress API](wordpress-api/README.md)
  * [Page 1](wordpress-api/page-1.md)