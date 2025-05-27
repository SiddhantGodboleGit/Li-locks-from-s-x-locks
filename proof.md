Each Lock is an **array of N (No. of operations)(columns in table) basic r/w locks**.

`write` - `exclusive lock`

`read`  - `shared lock`

`A-B`   - `operation A before B with - in commutavity`

### When Locking an item(i) for an operation A

   - For every column/operation B in i's array :
     - When A != B
       - For **-** on B get **read** on i's B.
       - For **+** on B get **NO** lock on i's B.
     - When A == B get a **write** lock on i's A.
       - For **+** on A release A **after** acquiring all required locks.
       - For **-** on A keep **write**.
         
   - When unlocking the item, just release the locks in the array.

> [!NOTE]
> - Have self-commutative operations after all the others.
> - Array acquisition in ascending while release in decending order of index. 
> - For non-symmetric table with A-A commutavity an additional fix is required.

### Proof by Induction
Looking at symmetric tables.

- For single operation
  | A | 
  | :--: |
  | -/+ |
  + **A-** : works like basic write with similar unlock
  + **A+** : obtaining and release of write without issue
  
- For two operations
  | | W | R |
  | :--: | :--: | :--: |
  | **W** | - | - | 
  | **R** | - | + |
  
  + **R+R** : obtaining and release of write without issue
  + **R-W** : R has read on W . W has to wait for write
  + **W-R** : W has read on R . R has to wait for write
  + **W-W** : W has write on itself

- For any new operation
  | | N | A | B | | Y | Z | M |
  | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
  | N | | | | | | | |
  | A | | | | | | | |
  | B | | | | | | | |
  | | | | | | | | |
  | Y | | | | | | | |
  | Z | | | | | | | |
  | M | | | | | | | |
  
  + self-commutive go last in array, other on start of array
  + A-A diagonal is always - followed by + giving concurrency.
  + 
