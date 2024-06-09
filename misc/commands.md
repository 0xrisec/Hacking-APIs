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

# Commands

```
 cat download_csv.csv | tr ',' ' ' | awk '$4 == "true" {print $1}' > valid-domains.txt
```
