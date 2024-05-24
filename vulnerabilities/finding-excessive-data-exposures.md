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

# Finding Excessive Data Exposures

Excessive data exposure is a vulnerability that takes place when the API provider sends more information than the API consumer requests.

This happens because the developers designed the API to depend on the consumer to filter results.

Check the user's comments, review their checklist, and ensure that any other user information appearing on your dashboard, webpage, or account is fetched from the respective object to obtain more data.
