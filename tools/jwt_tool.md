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

# jwt\_tool

## **Running a playbook scan**

While you’re in the JSON Web Token Toolkit, you may also want to run a playbook scan, that will look for known JWT-related vulnerabilities in the app you’re targeting.

To do this, run the following command:\
`python3 jwt_tool.py -t <URL of the target app> -rc "jwt=<your token>" -M pb`

For this scan to work, you will need to have Burp Suite running in the background, as this command requires a proxy to send its request to the target app. Also make sure Intercept is turned off in Burp Suite, as you don’t want to manually forward requests one by one…

An example of this command would be:

An example of this command would be:\
`python3 jwt_tool.py -t http://192.168.1.26:8888 -rc "jwt=eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0aW0xQGdtYWlsLmNvbSIsImlhdCI6MTY2ODM2MDI4OCwiZXhwIjoxNjY4NDQ2Njg4fQ.2dtyrHI68nsMx6q0RCB3UEIkVb6DcT7-We0dJxvMpJIujE6yshHvnU4ZfB9w-9iNyVL2yjQvE_dN706t0jbQ3g" -M pb`

This will automate a number of checks.

If this doesn’t turn up anything interesting, you can try another JSON Web Token Toolkit scan with an extended set, using the following command:

```
python3 jwt_tool.py -M at -t "http://crapi.apisec.ai/community/api/v2/community/posts/recent" -rh "Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJlbWFpbEBnbWFpbC5jb20iLCJpYXQiOjE3MTU4Mjg3ODIsImV4cCI6MTcxNTkxNTE4Mn0.ogRsnPsSDO0t8a3M8KjgMTum5aeWdClfuEr9Etnw1xXX9S9zpAJ9uQF0LDvjL6pUGCf_sMZmhsJQOD2kdL_eVA"
```



```
jwt_tool eyghbocibiJIUZZINIISIRSCCI6IkpXUCJ9.eyIzdW1101IxMjMENTY3ODkwIiwibmFtZSI6ImhBuEkg
SGFja2VyIiwiaWFQIjoxNTE2MjM5MDIyfQ.IX-Iz_e1CrPrkel FjArExaZpp3Y2tfawJUFQaNdftFw
```

Additionally, jwt\_tool has a “Playbook Scan” that can be used to target a web application and scan for common JWT vulnerabilities. You can run this scan by using the following:

```
jwt_tool -t http://target-site.com/ -rc "Header: JWT_Token" -M pb
```

To use this command, you’ll need to know what you should expect as the JWT header. When you have this information, replace "Header" with the name of the header and "JWT\_Token" with the actual token value.

To perform a JWT Crack attack using JWT\_Tool, use the following command:

```
jwt_tool <JWT Token> -C -d /wordlist.txt
```

{% embed url="https://jwt.io/" %}
