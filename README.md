# L<sub>i</sub> locks from shared and exclusive locks
Implementing Locks For A Commutativity Table In Layered Locking using exclusive(write) and shared(read) locks.
## Overview :
- Goal is to implement a Lock system for a specific Commutavity table **without a manager or busy-waiting**.
- Each Lock is an **array of N (No. of operations)(columns in table) basic r/w locks**.
### When Locking an item(i) for an operation A

   - For every column/operation B in i's array :
     - When A != B
       - For **-** on B get **Shared** lock on i's B.
       - For **+** on B get **NO** lock on i's B.
     - When A == B get a **Exclusive** lock on i's A.
       - For **+** on A release A **after** acquiring all required locks.
       - For **-** on A keep **Exclusive** lock.
         

   - In other words, for array do :
   
      - On others 
         - non-commutvity is a **read** lock, nothing otherwise.
      - On self get **write** lock
         - on commutavity release it after acquiring all, keep otherwise.

   - When unlocking the item, just release the locks in the array.

> [!NOTE]
> - Have self-commutative operations after all the others.
> - Array acquisition in ascending while release in decending order of index. 
> - For non-symmetric table with A-A commutavity an additional fix is required.
