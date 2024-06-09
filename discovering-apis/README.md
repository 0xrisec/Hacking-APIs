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

# Discovering APIs

Before you can attack a target’s APIs, you must locate those APIs and validate whether they are operational. In the process, you’ll also want to find credential information (such as keys, secrets, usernames, and passwords), version information, API documentation, and information about the API’s business purpose. The more information you gather about a target, the better your odds of discovering and exploiting API-related vulnerabilities. This chapter describes passive and active reconnaissance processes and the tools to get the job done.

## Techniques

1. Directory Listing: Finding open directories using Google Dork on your target organisation helps one to understand the directory structure on the webserver. It may reveal sensitive information or it may lead to information disclosure.
2. Configuration Files: Often times configuration files contains sensitive information such as hardcoded passwords, sensitive drive locations or API tokens which can help you gain privilege access to the internal resources.
3. Database Files: Database Files are data files that are used to store the contents of the database in a structured format into a file in separate tables and fields. Depending on the nature of the web application these files could provide access to sensitive information.
4. WordPress: WordPress is an open-source CMS written in PHP. WordPress has thousands of plugins to build, customise and enhance the websites. There are numerous vulnerabilities in these plugins. Finding WordPress related
5. Log Files: Log files sometimes provide detailed information of the users' activities in a particular application. These files are good to look at session cookies or other types of tokens.
6. Backup and Old Files: Backup files are original copies of the critical systems. These provide access to PII or access to sensitive records.
7. Login Pages: It is extremely important to identify login pages of your target organisation to perform bruteforce attempts or trying default credentials to gain further access to organisation resources.
8. SQL Errors: SQL errors leaks sensitive information about the backend systems. This can help one to perform enumeration on the database types and see if the application is vulnerable to input validation related attacks such as SQL Injection.
9. Apache Config Files: Apache HTTP Server is configured by placing directives in plain text configuration files. The main configuration file is usually called httpd.conf. In addition, other configuration files may be added using the Include directive, and wildcards can be used to include many configuration files. Any directive may be placed in any of these configuration files. Depending on the entries in these config files it may reveal database connection strings, username and passwords, the internal workings, used and referenced libraries and business logic of application.
10. Robots.txt File: Robots.txt file instructs web robots how to crawl pages on their website. Depending on the content of the file, an attacker might discover hidden directories and files.
11. DomainEye: DomainEye is a domain/host investigation tool that has the largest domain databases. They provide services such as reverse Whois, reverse IP lookup, as well as reverse NS and MX.
12. Publicly Exposed Documents: Such documents can be used to extract metadata information.
13. phpinfo(): Exposing phpinfo() on its own isn't necessarily a risk, but in combination with other vulnerabilities could lead to your site becoming compromised. Additionally, module versions could make attackers life easier when targeting application using newly discovered exploits.
14. Finding Backdoors: This can help one to identify website defacements or server hijacking related issues. By exploiting the open redirect vulnerability on the trusted web application, the attacker can redirect victims to a phishing page.
15. Install/Setup Files: Such files allows an attacker to perform enumeration on the target organisation. Information gathered using these files can help discover version details which can then be used to perform the targeted exploit.
16. Open Redirects: With these, we look at various known parameters vulnerable to open redirect related issues.
17. Apache Struts RCE: Successfully exploiting an RCE vulnerability could allow the attacker to run arbitrary programs. Here, we are looking for files with extensions of ".action" or ".do".
18. 3rd Party Exposure: Here we are looking for exposure of information on third party sites such as Codebeautify, Codeshare and Codepen.
19. Check Security Headers: Identify quickly if the target site is using security related headers in the server response.
20. GitLab: Quickly look for sensitive information on the GitLab.
21. Find Pastebin Entries: Shows you the results related to the target organisation on the Pastebin site. This could be passwords or any other sensitive information related to the target organisation.
22. Employees on LINKEDIN: Identifying employee names on LinkedIn can help you build a username list when it comes to password spraying attack.
23. .HTACCESS / Sensitive Files: Look for sensitive file exposure. This may indicate a server misconfiguration.
24. Find Subdomains: Subdomain helps you expand the attack surface on the target organisation. There are numerous tools available to automate the process of subdomain enumeration.
25. Find Sub-Subdomains: Identify sub-sub domains on the target organisation using Google Dork,
26. Find WordPress related exposure: WordPress related exposure helps you gain access to sensitive files and folders.
27. BitBucket & Atlassian: Source code leakage, hardcoded credentials and access to cloud infrastructure.
28. PassiveTotal: PassiveTotal is a great tool to perform threat investigation. Using BigBountyRecon we will use PassiveTotal to identify subdomains on the target information.
29. Stackoverflow: Source code exposure or any technology-specific questions mentioned on the Stackoverflow.
30. Find WordPress related exposure using Wayback Machine: Look for archieved WordPress files using WaybackMachine.
31. GitHub: Quickly look for sensitive information on the GitHub.
32. OpenBugBounty: Look for publicly exposed security issues on the OpenBugBounty website.
33. Reddit: Information about the particular organisation on the Reddit platform.
34. Crossdomain.xml: Look for misconfigured crossdomain.xml files on the target organisation.
35. ThreatCrowd: Search engine for threats, however, we are going to use this to identify additional sub-domains.
36. .git Folder: Source code exposure. it's possible to download the entire repository content if accessible.
37. YouTube: Look for any recent news on Youtube.
38. Digitalocean Spaces: Spaces is an S3-compatible object storage service that lets you store and serve large amounts of data. We will look for any data exposures.
39. .SWF File (Google): Flash is dead. We are going to use Google Dorks to look for older versions of flash .swf's which contain vulnerabilities.
40. .SWF File (Yandex): Flash is dead. We are going to use Yandex to look for older versions of flash .swf's which contain vulnerabilities.
41. .SWF File (Wayback Machine): Flash is dead. We are going to use WaybackMachine to look for older versions of flash .swf's which contain vulnerabilities.
42. Wayback Machine: Look for archived files to access old files.
43. Reverse IP Lookup: Reverse IP Lookup lets you discover all the domain names hosted on any given IP address. This will help you to explore the attack surface for a target organisation.
44. Traefik: Look for an open-source Edge Router for an unauthenticated interface which exposes internal services.
45. Cloud Storage and Buckets: Google CSE for various cloud storages - aws, digitalocean, backblaze, wasabi, rackspace, dropbox, ibm, azure, dreamhost, linode, gcp, box, mailru
46. s3 Buckets: Open s3 buckets.
47. PublicWWW: Source code search engine indexes the content of over 200 million web sites and provides a query interface that lets the caller find any alphanumeric snippet, signature or keyword in the web pages ‘HTML’, ‘JavaScript’ and ‘CSS’ style sheet code.
48. Censys (IPv4, Domains & Certs): Search engine for finding internet devices. We will use this to look for additional sub-domains using various endpoints on Censys.
49. Shodan: Search engine for Internet-connected devices
50. SharePoint RCE: Look for CVE-2020-0646 SharePoint RCE related endpoint.
51. API Endpoints: Find WSDL files.
52. Gist Searches: Quickly look for sensitive information on the Gist pastes.
53. CT Logs: Certificate Transparency (CT) is an Internet security standard and open-source framework for monitoring and auditing digital certificates. We will use to look for additional sub-domains for a targeted organisation.
54. Password Leak: Look for plaintext passwords of internal employees exposed in various leaks.
55. What CMS: Identify the version and type of CMS used by a target organisation for targeted enumeration and exploit research
