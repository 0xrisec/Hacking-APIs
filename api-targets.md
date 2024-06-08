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

# API Targets

The selection of OWASP crAPI, OWASP Juice Shop, OWASP DevSlop’s Pixi, and Damn Vulnerable GraphQL Application for your lab is an excellent choice for a comprehensive learning experience in API security. Each of these applications offers unique challenges and learning opportunities that cover a wide range of vulnerabilities and attack vectors. Here's a brief overview of what each application offers and how they can help develop your API hacking skills:

## OWASP crAPI (Completely Ridiculous API)

* **Focus**: Designed to be a vulnerable API first, crAPI simulates real-world API challenges in a modern, microservices architecture. It's an excellent tool for understanding how to discover and exploit API vulnerabilities in a cloud-native environment.
* **Skills Developed**: API discovery, exploiting business logic flaws, and understanding cloud-specific vulnerabilities.
* [http://crapi.apisec.ai](http://crapi.apisec.ai/login)

## OWASP Juice Shop

* **Focus**: Primarily a web application, Juice Shop also includes a rich set of API vulnerabilities that mimic a real-world e-commerce site. It's known for covering a broad spectrum of the OWASP Top 10 vulnerabilities.
* **Skills Developed**: Injection attacks, broken authentication, sensitive data exposure, and more. It's a great playground for understanding how front-end vulnerabilities can relate to underlying API weaknesses.
* [https://juice-shop.herokuapp.com/#/](https://juice-shop.herokuapp.com/#/)

## WebGoat

[https://tryhackme.com/r/room/webgoat](https://tryhackme.com/r/room/webgoat)

## OWASP DevSlop’s Pixi

* **Focus**: Pixi is part of the OWASP DevSlop project, showcasing vulnerabilities in modern web applications and APIs. It's particularly useful for learning about misconfigurations and vulnerabilities in web services and APIs.
* **Skills Developed**: Identifying misconfigurations, testing for insecure direct object references (IDOR), and understanding how to exploit XML External Entities (XXE) vulnerabilities.

## Damn Vulnerable GraphQL Application

* **Focus**: As the name suggests, this application focuses on GraphQL vulnerabilities. Given the rising popularity of GraphQL, understanding its unique security challenges is crucial.
* **Skills Developed**: Discovering hidden GraphQL queries and mutations, batch query attacks, and exploiting overly permissive queries. It's an excellent resource for learning about GraphQL-specific issues such as excessive data exposure.

## General Skills Developed Across These Applications:

* **Discovering APIs**: Learning how to find and map out the APIs exposed by these applications.
* **Fuzzing**: Using automated tools to send unexpected data to the APIs and observing how they behave, which can reveal vulnerabilities.
* **Configuring Parameters**: Understanding how to manipulate API parameters to test for vulnerabilities.
* **Testing Authentication**: Breaking or bypassing authentication mechanisms to access unauthorized functionality or data.
* **Discovering OWASP API Security Top 10 vulnerabilities**: Each application will have vulnerabilities that map to the OWASP API Security Top 10, providing a hands-on way to learn about these common issues.
* **Attacking Discovered Vulnerabilities**: Not just finding but also exploiting vulnerabilities to understand their impact and how they can be mitigated.

By working with these applications, you'll gain practical experience in identifying, exploiting, and mitigating a wide range of API vulnerabilities. This hands-on approach is invaluable for developing a deep understanding of API security and preparing for real-world challenges.

## Adding Other Vulnerable Apps

If you're looking to elevate your API hacking skills with more complex challenges, consider expanding your lab with additional vulnerable applications. GitHub hosts a wealth of repositories containing intentionally vulnerable APIs designed for educational purposes. Below is a curated list of such projects that you can easily integrate into your lab environment for a broader testing scope.

* **VAmPI** by Erev0s: Dive into the vulnerabilities of a Python-based API with [VAmPI](https://github.com/erev0s/VAmPI). This project offers a realistic environment for practicing API security testing techniques.
* **DVWS-node** by Snoopysecurity: Explore the node.js version of the Damn Vulnerable Web Services with [DVWS-node](https://github.com/snoopysecurity/dvws-node). It's a perfect target for honing your skills in identifying and exploiting common API vulnerabilities.
* **DamnVulnerable MicroServices** by ne0z: Challenge yourself with [DamnVulnerable MicroServices](https://github.com/ne0z/DamnVulnerableMicroServices), a project designed to simulate a microservices architecture with numerous security flaws waiting to be discovered.
* **Node-API-goat** by Layro01: [Node-API-goat](https://github.com/layro01/node-api-goat) offers a node.js API filled with vulnerabilities, providing a rich playground for testing and improving your security assessment skills.
* **Vulnerable GraphQL API** by AidanNoll from CarveSystems: For those interested in GraphQL, [Vulnerable GraphQL API](https://github.com/CarveSystems/vulnerable-graphql-api) presents a unique set of challenges to test your ability to exploit GraphQL-specific vulnerabilities.
* **Generic-University** by InsiderPhD: Simulating a university's API, [Generic-University](https://github.com/InsiderPhD/Generic-University) is a project that allows you to practice identifying and exploiting vulnerabilities in a more structured environment.
* **vulnapi** by tkisason: [vulnapi](https://github.com/tkisason/vulnapi) is another valuable addition to your lab, offering a variety of API vulnerabilities to test your penetration testing skills against.
* [http://testphp.vulnweb.com/](http://testphp.vulnweb.com/)

## Hacking APIs on TryHackMe and HackTheBox

Exploring API Vulnerabilities with TryHackMe and HackTheBox

[TryHackMe ](https://tryhackme.com)and [HackTheBox](https://www.hackthebox.com) offer interactive platforms for engaging with vulnerable systems, participating in capture-the-flag (CTF) events, tackling cybersecurity challenges, and advancing in hacking leaderboards. TryHackMe provides a mix of free and premium content, enabling users to launch and attack pre-configured vulnerable machines directly through their web browsers. Among its offerings are several notable machines designed to expose vulnerabilities in APIs, including:

* Bookstore (free)
* Carpe Diem 1 (free)
* ZTH: Obscure Web Vulns (paid)
* ZTH: Web2 (paid)
* GraphQL (paid)

These machines are instrumental in teaching the fundamentals of exploiting REST APIs, GraphQL APIs, and typical API authentication flaws. For beginners, TryHackMe simplifies the process of starting an attack by offering a "Start Attack Box" button, which quickly sets up a browser-based attack environment equipped with essential hacking tools discussed in this guide.

HackTheBox (HTB), while also offering both free and premium content, caters to individuals with a foundational understanding of hacking. Unlike TryHackMe, HTB does not provide attack machines, requiring users to supply their own. Access to HTB necessitates overcoming a unique challenge: hacking into the platform by cracking its invitation code. The main distinction between HTB's free and premium tiers lies in machine accessibility. The free tier grants access to the 20 latest machines, potentially including those with API vulnerabilities. To delve into HTB's comprehensive archive of vulnerable machines, including those with API weaknesses, users must opt for a VIP membership, which unlocks access to retired machines.
