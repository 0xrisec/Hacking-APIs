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

# Using filters

By adding `-fc 403` (filter code) we'll hide from the output all 403 [HTTP status codes](https://en.wikipedia.org/wiki/List\_of\_HTTP\_status\_codes).

```
ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fc 403
```

Sometimes you might want to filter out multiple status codes such as 500, 302, 301, 401, etc. For instance, if you know you want to see 200 status code responses, you could use `-mc 200` (match code) instead of having a long list of filtered codes.

```
ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -mc 200
```

Sometimes it might be beneficial to see what requests the server doesn't handle by matching for HTTP 500 Internal Server Error response codes (`-mc 500`). Finding irregularities in behavior could help better understand how the web app works.

There are other filters and matchers. For example, you could encounter entries with a 200 status code with a response size of zero. eg. `functions.php` or `inc/myfile.php`.

`-fs 0`

## Filter regexp

```
ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fr '/\..*'
```

## No 404 Status

In a perfect world all websites would respond correctly with the correct HTTP status codes

Let's try running the below ffuf example and see what happens

```
ffuf -w ~/wordlists/common.txt -u http://ffuf.me/cd/no404/FUZZ
```

From the ffuf response you'll notice that every file you've requested has come back as been found! It's not that you've got lucky and come across a load of content it's that the webpage displaying the "Page Cannot Be Found" message is not returning a 404 header

You'll notice that the "Page Cannot Be Found" page consistently has a file size of 669 bytes. Let's re-run the ffuf command but with the -fs switch which filters out any results that are 669 bytes in length.

```
ffuf -w ~/wordlists/common.txt -u http://ffuf.me/cd/no404/FUZZ -fs 669
```

## Word limit

\-fw 126

## Calibration

\-ac
