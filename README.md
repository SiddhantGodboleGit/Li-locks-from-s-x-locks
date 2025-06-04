# L<sub>i</sub> locks from shared and exclusive locks
Implementing Locks For A Commutativity Table In Layered Locking using exclusive(write) and shared(read) locks.
## Overview :
- Goal is to implement a Lock system for a specific Commutavity table **without a manager or busy-waiting**.
- The locks will block and operate like the shared and exclusive locks in cpp mutex.
- Each Lock is an **array of N (No. of operations)(columns in table) basic r/w locks**.
- Idea is to use the try_lock function on the array.
- [used lock reference](https://en.cppreference.com/w/cpp/thread/shared_mutex.html)

### When Locking an item(i) for an operation A

   - For every column/operation B in i's array :
     - When A != B
       - For **-** on B get **Shared** lock on i's B.
       - For **+** on B get **NO** lock on i's B.
     - When A == B get a **Exclusive** lock on i's A.         

   - When acquiring the locks follow strict rules of all or nothing.
     1 try_lock_shared() / try_lock() on the array in order.
     2 on failure on a lock, release others and acquire that one.
     3 then release and repeat **1** till all are acquired at once.
     
   - When unlocking the item, just release the locks in the array.

> [!NOTE]
> - Have self-commutative operations after all the others.
> - Array acquisition in ascending while release in decending order of index. 
> - For non-symmetric table with A-A commutavity an additional fix is required.
