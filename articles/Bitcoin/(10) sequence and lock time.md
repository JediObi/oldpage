### **sequence and lock time**




sequence and lock_time determine the transaction will be final or non-final. 
Note: Bitcoin doesn't support non-final transaction in the newes verison. 

The sequence is a number between 0x00000000 and 0xffffffff. 

+ sequnce number

```
If it is 0xffffffff, the transaction is final state.0xffffffff will ignore lock_time.
If not, the tranxaction is non-final state.
```

+ lock time

```
If lock_time=0, the transaction is final state.0 will ignore the sequence number.
If not, the transaction is non-final state.
```

New transaction in the future can not use the utxo from non-final transactions. 
A non-final transaction could be change if you want to.
For example, you create a transaciton who's lock time is 6 months later because you want this transaciton to become effective at 6 months later. During the 6 month, you could change the transaction content if you want. 
And the new transaciton with new sequence number replace the old version. The old version will be expired.


