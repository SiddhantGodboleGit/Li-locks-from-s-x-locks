# L<sub>i</sub> locks from shared and exclusive locks
Implementing Layered Locks For A Commutativity Table In Layered Locking using exclusive(write) and shared(read) locks.
## Overview :
- Each Lock is an array of N (No. of operations)(columns in table) basic r/w locks.
- When Locking an item(i) for an operation(A)
   - For every column/operation B in i's array :
     + When A != B
       - For **-** on B get **Shared** lock on i's B.
       - For **+** on B get **NO** lock on i's B.
     + When A == B get a **Exclusive** lock on A.
       - For **+** on A release A **after** acquiring all required locks.
       - For **-** on A keep **Exclusive** lock.
