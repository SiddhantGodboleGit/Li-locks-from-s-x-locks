# L<sub>i</sub> locks from shared and exclusive locks
Implementing Locks For A Commutativity Table In Layered Locking using exclusive(write) and shared(read) locks.

This is helpful for commutativity-aware concurrency control.
## Overview :
- Goal is to implement a Lock system for a specific Commutavity table **without a manager or busy-waiting**.
- The locks will block and operate like the shared and exclusive locks in cpp mutex.
- Each Lock is an **array of N (No. of operations)(columns in table) basic r/w locks**.
- Idea is to use the try_lock function on the array.
- [used lock reference](https://en.cppreference.com/w/cpp/thread/shared_mutex.html)

### For an operation A on item(i)

   - For every column/operation B in i's array :
     - When A != B
       - For **-** on B get **Shared** lock on i's B.
       - For **+** on B get **NO** lock on i's B.
     - When A == B get an **Exclusive** lock on i's A.
       - For **+** on A release A **after** acquiring all required locks.
       - For **-** on A keep **write**.        

   - When acquiring the locks follow strict rules of all or nothing.
     - try_lock_shared() / try_lock() on the array in order.
     - on failure on a lock, release others and acquire that one.
     - then release and repeat from top till all are acquired at once.
     
   - When unlocking the item, just release the locks in the array in reverse order.

> [!NOTE]
> - Have self-commutative operations after all the others.
> - Array acquisition in ascending while release in decending order of index. 
> - For non-symmetric table with A-A commutavity an additional fix is required.

## Extra :
- Without the try lock system the later operations will get lower precedence and would be blocked by the prior operations. This can be useful in some systems with high traffic or with operations having precedences.
