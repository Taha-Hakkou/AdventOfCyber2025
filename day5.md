### Day 5

* IDOR: Insecure Direct Object References (access control vulnerability)

* privilege escalation:
- vertical: gaining access to more features
- horizontal: using features you are authorized to use, but gaining access to data you are not.

* IDOR is usually a form of horizontal privilege escalation.

* techniques to hide potential IDOR: (not efficient)
- encoding: Base64...
- hashing: md5...
- uuid (can be decoded: online decoder)

* instead use random or hard to guess IDs

* the issue with UUID1 is if we know the exact date when the code was generated, we can recover the uuid.