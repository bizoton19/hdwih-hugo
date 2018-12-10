---
title: The Go Memory model
date: 2018-12-10T03:16:58.543Z
thumbnail: /static/img/Hdwih_Logo.png
---
Within a single goroutine, reads and writes must behave as if they executed in the order specified by the program. That is, compilers and processors may reorder the reads and writes executed within a single goroutine only when the reordering does not change the behavior within that goroutine as defined by the language specification. Because of this reordering, the execution order observed by one goroutine may differ from the order perceived by another. For example, if one goroutine executes a = 1; b = 2;, another might observe the updated value of b before the updated value of a.



To specify the requirements of reads and writes, we define happens before, a partial order on the execution of memory operations in a Go program. If event e1 happens before event e2, then we say that e2 happens after e1. Also, if e1 does not happen before e2 and does not happen after e2, then we say that e1 and e2 happen concurrently.



Within a single goroutine, the happens-before order is the order expressed by the program.



A read r of a variable v is allowed to observe a write w to v if both of the following hold:



r does not happen before w.

There is no other write w' to v that happens after w but before r.

To guarantee that a read r of a variable v observes a particular write w to v, ensure that w is the only write r is allowed to observe. That is, r is guaranteed to observe w if both of the following hold:



w happens before r.

Any other write to the shared variable v either happens before w or after r.

This pair of conditions is stronger than the first pair; it requires that there are no other writes happening concurrently with w or r.



Within a single goroutine, there is no concurrency, so the two definitions are equivalent: a read r observes the value written by the most recent write w to v. When multiple goroutines access a shared variable v, they must use synchronization events to establish happens-before conditions that ensure reads observe the desired writes.



The initialization of variable v with the zero value for v's type behaves as a write in the memory model.



Reads and writes of values larger than a single machine word behave as multiple machine-word-sized operations in an unspecified order.
