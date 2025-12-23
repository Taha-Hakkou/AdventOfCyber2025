# day 20

* A **race condition** happens when two or more actions occur at the same time, and the system’s outcome depends on thebunny character showing car racing. order in which they finish. In web applications, this often happens when multiple users or automated requests simultaneously access or modify shared resources, such as inventory or account balances. If proper synchronisation isn’t in place, this can lead to unexpected results, such as duplicate transactions, oversold items, or unauthorised data changes.
Types of Race Conditions

* Generally, race condition attacks can be divided into three categories:
    - __Time-of-Check to Time-of-Use (TOCTOU)__: A TOCTOU race condition happens when a program checks something first and uses it later, but the data changes in between. This means what was true at the time of the check might no longer be true when the action happens.
    - __Shared resource__: This occurs when multiple users or systems try to change the same data simultaneously without proper control. Since both updates happen together, the final result depends on which one finishes last, creating confusion.
    - __Atomicity violation__: An atomic operation should happen all at once, either fully done or not at all. When parts of a process run separately, another request can sneak in between and cause inconsistent results.

* Mitigation measures to avoid the vulnerability:
    - Use __atomic database transactions__ so stock deduction and order creation execute as a single, consistent operation.
    - Perform a __final stock validation__ right before committing the transaction to prevent overselling.
    - Implement __idempotency keys__ for checkout requests to ensure duplicates aren’t processed multiple times.
    - Apply __rate limiting__ or concurrency controls to block rapid, repeated checkout attempts from the same user or session.

* Practical (Burpsuite):
    - when using burpsuite as proxy, all requests are recorded in 'http history' either intercepted or not.
    - to use a request in repeater: right-click + 'Send to repeater'.
    - in repeater, right-click the tab > add tab to group > create tab group.
    - duplicate tab (15 copies ofr example).
    - Send > send group in parallel (launches all copies at once and waits for the final byte from each response, maximising the timing overlap to trigger race conditions).
    - Send group.