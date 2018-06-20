### **Transaction data structure**

tx_a hash code is tx_a_hash
tx_b hash code is tx_b_hash
+ tx_a 
tx_a_hash
```
Input:
Previous tx: <previous_tx_hash>
Index: 0
scriptSig: <previous_tx_hash_ecdsa> <user_a_wallet_pub_key>

Output:
Value: 5000000000
scriptPubKey: OP_DUP OP_HASH160 <user_b_wallet_pub_key_hash> OP_EQUALVERIFY OP_CHECKSIG
```

+ tx_b 
tx_b_hash
```
Input:
Previous tx: <tx_a_hash>
Index: 0
scriptSig: <tx_a_hash_ecdsa> <user_b_wallet_pub_key>

Output:
Value: 5000000000
scriptPubKey: OP_DUP OP_HASH160 <user_c_wallet_pub_key_hash> OP_EQUALVERIFY OP_CHECKSIG
```
now run tx_b input script
```
:put <tx_a_hash_ecdsa>
:put <user_b_wallet_pub_key>
:skip to tx_a and run tx_a output script
:OP_DUP -> cp top ->put <user_b_wallet_pub_key>
:OP_HASH160 -> pop top <user_b_wallet_pub_key> and hash->put <user_b_wallet_pub_key_hash'>
:put <user_b_wallet_pub_key_hash>
:OP_EQUALVERIFY -> pop top 1 and top 2 and compare -> compare(<user_b_wallet_pub_key_hash'>,<user_b_wallet_pub_key_hash>) if true then proceed
:OP_CHECKSIG
```