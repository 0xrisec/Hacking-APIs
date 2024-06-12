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

# Passive Recon

Passive reconnaissance is the act of obtaining information about a target without directly interacting with the target’s devices. Typically, passive reconnaissance leverages open-source intelligence (OSINT), which is data collected from publicly available sources.

### Phase One: Cast a Wide Net

Google, DNS Dumpster, OWASP Amass

Search the internet for very general terms to learn some fundamental information about your target. Search engines such as Google such as its usage, design and architecture, documentation, and business purpose, as well as industry-related information and many other potentially significant items.

Additionally, you need to investigate your target’s attack surface. This can be done with tools such as DNS Dumpster and OWASP Amass. DNS Dumpster performs DNS mapping by showing all the hosts related to the target’s domain name and how they connect to each other.&#x20;

### Phase Two: Adapt and Focus

Next, take your findings from phase one and adapt your OSINT efforts to the information gathered. This might mean increasing the specificity of your search queries or combining the information gathered from separate tools to gain new insights. In addition to using search engines, you might search **GitHub** for repositories related to your target and use a tool such as `passive`to find exposed sensitive information.

### Phase Three: Document the Attack Surface&#x20;

Taking notes is crucial to performing an effective attack. Document and take screen captures of all interesting findings. Create a task list of the passive reconnaissance findings that could prove useful throughout the rest of the attack. Later, while you’re actively attempting to exploit the API’s vulnerabilities, return to the task list to see if you’ve missed anything. The following sections go deeper into the tools you’ll use throughout this process. Once you begin experimenting with these tools, you’ll notice some crossover between the information they return. However, I encourage you to use multiple tools to confirm your results.&#x20;

You wouldn’t want to fail to find privileged API keys posted on GitHub, for example, especially if a criminal later stumbled upon that low-hanging fruit and breached your client.

### Google Hacking

&#x20;Google hacking (also known as Google dorking) involves the clever use of advanced search parameters and can reveal all sorts of public API-related information about your target, including vulnerabilities, API keys, and usernames, that you can leverage during an engagement.

{% embed url="https://www.exploit-db.com/google-hacking-database" %}

{% embed url="https://pentest-tools.com/information-gathering/google-hacking" %}

{% embed url="https://www.researchgate.net/profile/Bill-Gardner/publication/319621156_Google_Hacking_for_Penetration_Testers_Third_Edition_3rd_Edition/links/59b5b05f0f7e9b3743551f2e/Google-Hacking-for-Penetration-Testers-Third-Edition-3rd-Edition.pdf" %}

{% embed url="https://web.archive.org/web/20071011023432/http://johnny.ihackstuff.com/ghdb.php" %}

{% embed url="https://www.blackhat.com/presentations/bh-europe-05/BH_EU_05-Long.pdf" %}

{% embed url="https://web.archive.org/web/20071011020052/http://johnny.ihackstuff.com/ghdb.php?function=summary&cat=19" %}

### OWASP Amass

```
amass enum -passive -d twitter.com |grep api
```

Amass has several useful command line options. Use the intel command to collect SSL certificates, search reverse Whois records, and find ASN IDs associated with your target. Start by providing the command with target IP addresses:

```
amass intel -addr <target IP addr>
```

If this scan is successful, it will provide you with domain names. These domains can then be passed to intel with the whois option to perform a reverse Whois lookup:

```
amass intel -d <target domain> –whois
```

This could give you a ton of results. Focus on the interesting results that relate to your target organization. Once you have a list of interesting domains, upgrade to the enum subcommand to begin enumerating subdo- mains. If you specify the -passive option, Amass will refrain from directly interacting with your target:

```
amass enum -passive -d <target domain>
```

The active enum scan will perform much of the same scan as the passive one, but it will add domain name resolution, attempt DNS zone transfers, and grab SSL certificate information:&#x20;

```
amass enum -active -d <target domain>
```

To up your game, add the -brute option to brute-force subdomains, -w to specify the API\_superlist wordlist, and then the -dir option to send the output to the directory of your choice:&#x20;

```
$ amass enum -active -brute -w /usr/share/wordlists/API_superlist -d -dir [directory name] 
```

If you’d like to visualize relationships between the data Amass returns, use the viz subcommand, as shown next, to make a cool-looking web page (see Figure 6-6). This page allows you to zoom in and check out the various related domains and hopefully some API endpoints.&#x20;

```
amass viz -enum -d3 -dir <directory name>
```

### Exposed Information on GitHub

Searching GitHub for OSINT could reveal your target’s API capabilities, documentation, and secrets, such as admin-level API keys, passwords, and tokens, which could be useful during an attack. Begin by searching GitHub for your target organization’s name paired with potentially sensitive types of information, such as “api-key,” “password,” or “token.”

Additionally, view historical commits to the code by using the History button found. If you came across an issue or comment that led you to believe there were once vulner- abilities associated with the code, you can look for historical commits to see if the vulnerabilities are still viewable.

### Issues

The Issues tab is a space where developers can track bugs, tasks, and feature requests. If an issue is open, there is a good chance that the vulnerability is still live within the code

#### Pull Requests

The Pull Requests tab is a place that allows developers to collaborate on changes to the code. If you review these proposed changes, you might sometimes get lucky and find an API exposure that is in the process of being resolved.

### AssetFinder

&#x20;[https://github.com/tomnomnom/assetfinder](https://github.com/tomnomnom/assetfinder)\
[https://github.com/projectdiscovery/katana](https://github.com/projectdiscovery/katana)\
waybackurls\
\
Subdomain finder, give addition info: [https://www.vedbex.com/subdomain-finder/](https://www.vedbex.com/subdomain-finder/)

## Github dorks

[https://github.com/techgaun/github-dorks](https://github.com/techgaun/github-dorks)\


[https://www.zoomeye.hk/](https://www.zoomeye.hk/)\
[https://viz.greynoise.io](https://viz.greynoise.io)\
[https://search.censys.io/](https://search.censys.io/)\
[https://hunter.ho](https://hunter.how)
