Each Lock is an **array of N (No. of operations)(columns in table) basic r/w locks**.
### When Locking an item(i) for an operation A

   - For every column/operation B in i's array :
     - When A != B
       - For **-** on B get **Shared** lock on i's B.
       - For **+** on B get **NO** lock on i's B.
     - When A == B get a **Exclusive** lock on i's A.
       - For **+** on A release A **after** acquiring all required locks.
       - For **-** on A keep **Exclusive** lock.
         
   - When unlocking the item, just release the locks in the array.

> [!NOTE]
> - Array acquisition in ascending while release in decending order of index. 
> - For non-symmetric table with A-A commutavity an additional fix is required.

### Proof by Induction
- For single operation
  | A | correctness |
  | -- | -- |
  | -  | works like basic exclusive lock |
  
