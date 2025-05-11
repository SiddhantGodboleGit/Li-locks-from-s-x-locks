# L<sub>i</sub> locks from shared and exclusive locks
Implementing Layered Locks For A Commutativity Table In Layered Locking using exclusive and shared locks.
## Overview :
- Each Lock is an array of N (columns in table) basic r/w locks.
- When Locking an item(i) for an operation(A) :
- * For + on another operation(B) get **NO** lock on i's B.
