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

# waybackurls

GitHub: [https://github.com/tomnomnom/waybackurls](https://github.com/tomnomnom/waybackurls)\
KXSS: [https://github.com/Emoe/kxss](https://github.com/Emoe/kxss)\
nuclei: [https://github.com/projectdiscovery/nuclei](https://github.com/projectdiscovery/nuclei)\
httpx

```
cat subdomains.txt | waybackurls > endpoints.txt
cat endpoints.txt | grep -E 'user|admin|password|api' // grep important infromation


// find xss
echo http://testphp.vulnweb.com/ | waybackurls | kxss
cat  endpoints.txt | kxss

// automate your work
nuclei
tempaltes: https://github.com/projectdiscovery/nuclei-templates

```

