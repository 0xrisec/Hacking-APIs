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

# Finding Business Logic Flaws

Business logic flaws could be discovered as early as when you review the API documentation and find directions for how not to use the application.

When you find these, your next step should be obvious: do the opposite of what the documentation recommends! Consider the following examples:

1. If the documentation tells you not to perform action X, perform action X.
2. If the documentation tells you that data sent in a certain format isn’t validated, upload a reverse shell payload and try to find ways to execute it. Test the size of file that can be uploaded. If rate limiting is lacking and file size is not validated, you’ve discovered a serious business logic flaw that will lead to a denial of service.&#x20;
3. If the documentation tells you that all file formats are accepted, upload files and test all file extensions. If you can upload these sorts of files, the next step would be to see if you can execute them.\
   [https://github.com/hAPI-hacker/Hacking-APIs](https://github.com/hAPI-hacker/Hacking-APIs)

The challenging part about business logic flaws is that they are unique to each business. Identifying features as vulnerabilities will require putting on your evil genius cap and using your imagination.
