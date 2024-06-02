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

# Fuzzing parameters

What would you do when you find a page or API endpoint but don't know which parameters are accepted?&#x20;

<mark style="color:purple;">You fuzz!</mark>

Viewing the page [/cd/param/data](http://ffuf.me/cd/param/data) you are greeted with the message "Required Parameter Missing" and the page is set to a HTTP Status Code of 400 which means Bad Request.

Using the below request we can try and find the missing parameter.

```
root@ffuf:~# ffuf -w ~/wordlists/parameters.txt -u http://ffuf.me/cd/param/data?FUZZ=1
```

Discovering a vulnerable parameter could lead to file inclusion, path disclosure, XSS, SQL injection, or even command injection. Since ffuf allows you to put the keyword anywhere we can use it to fuzz for parameters.

```
$ ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -fw 39
$ ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -fw 39
```

At this point, we could generate a wordlist and save a file containing integers. To cut out a step we can use `-w -` which tells ffuf to read a wordlist from [stdout](https://www.gnu.org/software/libc/manual/html\_node/Standard-Streams.html). This will allow us to generate a list of integers with a command of our choice then pipe the output to ffuf. Below is a list of 5 different ways to generate numbers 0 - 255.

```
$ ruby -e '(0..255).each{|i| puts i}' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
$ ruby -e 'puts (0..255).to_a' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
$ for i in {0..255}; do echo $i; done | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
$ seq 0 255 | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
$ cook '[0-255]' | ffuf -u 'http://MACHINE_IP/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 
```

We can also use ffuf for wordlist-based brute-force attacks, for example, trying passwords on an authentication page.

```
$ ffuf -u http://MACHINE_IP/sqli-labs/Less-11/ -c -w /usr/share/seclists/Passwords/Leaked-Databases/hak5.txt -X POST -d 'uname=Dummy&passwd=FUZZ&submit=Submit' -fs 1435 -H 'Content-Type: application/x-www-form-urlencoded'
```

Here we have to use the POST method (specified with `-X`) and to give the POST data (with `-d`) where we include the `FUZZ` keyword in place of the password.

We also have to specify a custom header `-H 'Content-Type: application/x-www-form-urlencoded'` because ffuf doesn't set this content-type header automatically as curl does.

