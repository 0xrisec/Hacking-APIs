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

# Effective Fuzzing

All of the following could result in interesting responses:&#x20;

1. Sending an exceptionally large number when a small number is expected
2. Sending database queries, system commands, and other code&#x20;
3. Sending a string of letters when a number is expected&#x20;
4. Sending a large string of letters when a small string is expected&#x20;
5. Sending various symbols (-\_!@#$%^&\*();':''|,./?>)&#x20;
6. Sending characters from unexpected languages (漢, さ, Ж, Ѫ, Ѭ, Ѧ, Ѩ, Ѯ)

## Choosing Fuzzing Payloads

