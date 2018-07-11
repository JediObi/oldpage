### **P2SH - Multiple Signature, Contract Embry**



+ The creation of P2SH. 
==> Create UTXO : To create a P2SH transaction output, you need a multiple signed address for output position. 

+ The recommendation of P2SH's output. 
==> Use P2SH OUTPUT : To use a P2SH utxo, you and your companys need to sign the transaction multiple times with your and your companys' private keys to generate scriptSig. 

+ M-of-N 
==> P2SH is the derivatives of M-of-N. 
==> Create UTXO : M-of-N is the manual process to create a scriptPubKey with m, n and pubKey of each address. P2SH use a multi-signed address's scriptPubKey to instead of scriptPubKey by manual process. Actually the manual process in M-of-N will produce a multi-signed address just like P2SH (You can see this by ```decodescript <scriptPubKey>```).  
==> Use OUTPUT : The transaction need to be signed multiple times with all private keys of scriptPubKey to generate scriptSig. This processes is same with P2SH. 



## (0) Init Test Environment

+ #### generate 200 blocks at node 1

```~:make generate BLOCKS=200```

==================== State =======================

Wallet at Node1 -> W1 
Wallet at Node2 -> W2 
W1_balance: 5000 
W2_balance: 0 

================================================

+ #### generate 2 new addresses at node2 and get their private keys

```
~:make address2
~:make address2
~:bitcoin-cli -datadir=2 dumpprivkey <address>
```

==================== Result =======================

Address1 -> A1 
Address2 -> A2 

A1_hash: moX3qTsaLt9NjvQBGHy78jvmLZX9ZWk7aP   
A1_privKey: cN3ggP6FEmQF9zWt2FUNP8rxCLxNLBCgRcmWfD7jtqAvx1Geud7w 

A2_hash: n3MXNfADQRYwwJ4iwoNLMWxA3bwba7hsiU   
A2_privKey: cQkEdyskEDiRJdadFgq9DD9yDbc6Kib5YPkNdH6fZX3U4TZxoj7n 

=================================================

=================== State =========================

W1_balance: 5000 
W2_balance: 0 
A1_balance:  0 
A2_balance:  0 

=================================================

+ #### send 200 btc from node1 to node2

send 200 btc to ```A1```. 
```
~:make sendfrom1 ADDRESS=moX3qTsaLt9NjvQBGHy78jvmLZX9ZWk7aP AMOUNT=200
~:make generate BLOCKS=10
```

==================== State ========================

W1_balance: 5299.99987060 
W2_balance: 200 
A1_balance: 200 
A2_balance: 0 

=================================================



## (1) Create multiple signatured address

Create a multiple signatured address ```A3``` to receive output. 

+ #### command

```
~:bitcoin-cli -datadir=2 createmultisig 2 '["A1_pubKey","A2_pubKey"]'
```

If you keep the private keys of all the address, you can just use the addresses instead of the pubkeys of them to generate multiple signatrued address.  

+ #### generate multiple signatured address

```
~:bitcoin-cli -datadir=2 createmultisig 2 '["moX3qTsaLt9NjvQBGHy78jvmLZX9ZWk7aP","n3MXNfADQRYwwJ4iwoNLMWxA3bwba7hsiU"]'
```
==================== Result =======================

Address 3 -> A3 
{ 
  "address": "2N5q4afW3cCVVUR8365imV5nJq6r7oe76yS", 
  "redeemScript": "52210387f89fca6896b35752e37cc3e61c614abfedfb80269a8794283ed40657d9742a210392edcffb03a4b48ac5083ed2bbc05b15b4bd2168bf00d69f446ca19cae3ca6f752ae" 
} 

=================================================

+ #### get scriptPubKey of A3

```
~:bitcoin-cli -datadir=2 validateaddress 2N5q4afW3cCVVUR8365imV5nJq6r7oe76yS
```

==================== Result =======================
{ 
  "isvalid": true, 
  "address": "2N5q4afW3cCVVUR8365imV5nJq6r7oe76yS", 
  "scriptPubKey": "a9148a07cca1f387bad85410be81e9c0747be94234ea87", 
  "ismine": false, 
  "iswatchonly": false, 
  "isscript": true 
} 

=================================================

==================== State ========================

W1_balance: 5299.99987060 
W2_balance: 200 
A1_balance: 200 
A2_balance: 0 
A3_balance: 0 

=================================================



## (2) Create P2SH transaction

Create transaction ```X``` that outputs to the multiple signatured address ```A3```. 

+ #### command

```
# show utxo
~:bitcoin-cli -datadir=2 listunspent
# create raw transaction
~:bitcoin-cli -datadir=2 createrawtransaction '[{"txid":"", "vout":, <"sequence":>}]' '{"A1":189.9999,"A3":10}'
```

+ #### create transaction X

```
~:bitcoin-cli -datadir=2 listunspent
~:bitcoin-cli -datadir=2 createrawtransaction '[{"txid":"9fceca794c2c156ee3a2c56dcec96be6dd6a95339057262d972e96931f687b2e", "vout":1}]' '{"moX3qTsaLt9NjvQBGHy78jvmLZX9ZWk7aP":189.9999,"2N5q4afW3cCVVUR8365imV5nJq6r7oe76yS":10}'
```

==================== Result X ======================

01000000012e7b681f93962e972d26579033956adde66bc9ce6dc5a2e36e152c4c79cace9f0100000000ffffffff02f0d67c6c040000001976a91457c5a1d30532a4bef13cdc3f7eab5c0b50001eba88ac00ca9a3b0000000017a9148a07cca1f387bad85410be81e9c0747be94234ea8700000000 

=================================================

## (3) Use private key 1 to sign X

Sign X and get signed raw transaction X1.

+ #### command

```
~:bitcoin-cli -datadir=2 signrawtransaction x '[]' '["A1_privKey"]'
```

+ #### sign X

generate X1 
```
~:bitcoin-cli -datadir=2 signrawtransaction 01000000012e7b681f93962e972d26579033956adde66bc9ce6dc5a2e36e152c4c79cace9f0100000000ffffffff02f0d67c6c040000001976a91457c5a1d30532a4bef13cdc3f7eab5c0b50001eba88ac00ca9a3b0000000017a9148a07cca1f387bad85410be81e9c0747be94234ea8700000000 '[]' '["cN3ggP6FEmQF9zWt2FUNP8rxCLxNLBCgRcmWfD7jtqAvx1Geud7w"]'
```

==================== Result X1 =====================

01000000012e7b681f93962e972d26579033956adde66bc9ce6dc5a2e36e152c4c79cace9f010000006b483045022100f565e7e70622da2afbd14d2fcf6ea64658ce4f83858f56a0addef6618171d8bd0220792e2e7a30d4097b21ad87c5026ddd67bda2aea0995aeec7c3df1553dc4df15701210387f89fca6896b35752e37cc3e61c614abfedfb80269a8794283ed40657d9742affffffff02f0d67c6c040000001976a91457c5a1d30532a4bef13cdc3f7eab5c0b50001eba88ac00ca9a3b0000000017a9148a07cca1f387bad85410be81e9c0747be94234ea8700000000 

=================================================



## (4) Boardcast X1 and make record

+ #### command

```
~:bitcoin-cli -datadir=2 sendrawtransaction X1 
~:make generate BLOCKS=10
```

+ #### boardcast and make record

```
~:bitcoin-cli -datadir=2 sendrawtransaction 01000000012e7b681f93962e972d26579033956adde66bc9ce6dc5a2e36e152c4c79cace9f010000006b483045022100f565e7e70622da2afbd14d2fcf6ea64658ce4f83858f56a0addef6618171d8bd0220792e2e7a30d4097b21ad87c5026ddd67bda2aea0995aeec7c3df1553dc4df15701210387f89fca6896b35752e37cc3e61c614abfedfb80269a8794283ed40657d9742affffffff02f0d67c6c040000001976a91457c5a1d30532a4bef13cdc3f7eab5c0b50001eba88ac00ca9a3b0000000017a9148a07cca1f387bad85410be81e9c0747be94234ea8700000000 
~:make generate BLOCKS=10
```

===================== Result ======================

X1 txid:  2ac8a4e25f6e68f667d75249176abac6f07ee75bcdc508d90856888f2731c1dc 

=================================================

==================== State ========================

W1_balance: 5799.99987060 
W2_balance: 189.9999 
A1_balance: 189.9999 
A2_balance: 0 
A3_balance: 10 

=================================================



## (5) Create new transaction that recommend the P2SH's output

Create a new transcation Y that recommend X1's output 1 as input. 

+ #### command

```
# show utxo
~:bitcoin-cli -datadir=2 listunspent
# get tx hex
~:bitcoin-cli -datadir=2 getrawtransaction <txid>
# decode hex
~:bitcoin-cli -datadir=2 decoderawtransaction <hex>

# create transaction
~:bitcoin-cli -datadir=2 createrawtransaction '[{"txid":"X1_txid", "vout": 1}]' '{"A2":9.9999}'
```

+ #### create transaction Y

```
~:bitcoin-cli -datadir=2 createrawtransaction '[{"txid":"2ac8a4e25f6e68f667d75249176abac6f07ee75bcdc508d90856888f2731c1dc", "vout":1}]' '{"n3MXNfADQRYwwJ4iwoNLMWxA3bwba7hsiU":9.9999}'
```

===================== Result Y =====================

0100000001dcc131278f885608d908c5cd5be77ef0c6ba6a174952d767f6686e5fe2a4c82a0100000000ffffffff01f0a29a3b000000001976a914ef8a388d9996d9b316b6e335183f483493770e6588ac00000000 

=================================================



## (6) Use private key 1 to sign the new transaction.

Use A1_priveKey to sign Y. It will generate Y1. 
Note: After signing, you will get a full stack of output info. {hex:,complete:false} means signing successfully finished. The error stack can be ignored if it is success. 

+ #### command

```
~:bitcoin-cli -datadir=2 signrawtransaction Y '[{"txid":"", "vout":, "scriptPubKey":"A3_scriptPubKey", "redeemScript": "A3_redeemscript"}]' '["A1_privKey"]'
```

+ #### sign transaction Y to Y1

```
~:bitcoin-cli -datadir=2 signrawtransaction 0100000001dcc131278f885608d908c5cd5be77ef0c6ba6a174952d767f6686e5fe2a4c82a0100000000ffffffff01f0a29a3b000000001976a914ef8a388d9996d9b316b6e335183f483493770e6588ac00000000 '[{"txid":"2ac8a4e25f6e68f667d75249176abac6f07ee75bcdc508d90856888f2731c1dc", "vout":1, "scriptPubKey":"a9148a07cca1f387bad85410be81e9c0747be94234ea87", "redeemScript":"52210387f89fca6896b35752e37cc3e61c614abfedfb80269a8794283ed40657d9742a210392edcffb03a4b48ac5083ed2bbc05b15b4bd2168bf00d69f446ca19cae3ca6f752ae"}]' '["cN3ggP6FEmQF9zWt2FUNP8rxCLxNLBCgRcmWfD7jtqAvx1Geud7w"]'
```

===================== Result Y1 ====================

0100000001dcc131278f885608d908c5cd5be77ef0c6ba6a174952d767f6686e5fe2a4c82a01000000910047304402204263c6b3970694a5c3fea4a781bdb2d97e9489b2b70972d1749419a251e2afb602207f5f4aac6d097adfc39ffddafe3321219f5fa6a3fa03ce2258b47bd7fff8e81e014752210387f89fca6896b35752e37cc3e61c614abfedfb80269a8794283ed40657d9742a210392edcffb03a4b48ac5083ed2bbc05b15b4bd2168bf00d69f446ca19cae3ca6f752aeffffffff01f0a29a3b000000001976a914ef8a388d9996d9b316b6e335183f483493770e6588ac00000000 

=================================================



## (7) Use private key 2 to sign the transaction Y1

Use A2_privKey to sign Y1 and generate Y2.

+ #### command

```
~:bitcoin-cli -datadir=2 signrawtransaction Y1 '[{"txid":"", "vout":, "scriptPubKey":"A3_scriptPubKey", "redeemScript": "A3_redeemscript"}]' '["A2_privKey"]'
```

+ #### sign transaction Y1

```
~:bitcoin-cli -datadir=2 signrawtransaction 0100000001dcc131278f885608d908c5cd5be77ef0c6ba6a174952d767f6686e5fe2a4c82a01000000910047304402204263c6b3970694a5c3fea4a781bdb2d97e9489b2b70972d1749419a251e2afb602207f5f4aac6d097adfc39ffddafe3321219f5fa6a3fa03ce2258b47bd7fff8e81e014752210387f89fca6896b35752e37cc3e61c614abfedfb80269a8794283ed40657d9742a210392edcffb03a4b48ac5083ed2bbc05b15b4bd2168bf00d69f446ca19cae3ca6f752aeffffffff01f0a29a3b000000001976a914ef8a388d9996d9b316b6e335183f483493770e6588ac00000000 '[{"txid":"2ac8a4e25f6e68f667d75249176abac6f07ee75bcdc508d90856888f2731c1dc", "vout":1, "scriptPubKey":"a9148a07cca1f387bad85410be81e9c0747be94234ea87","redeemScript":"52210387f89fca6896b35752e37cc3e61c614abfedfb80269a8794283ed40657d9742a210392edcffb03a4b48ac5083ed2bbc05b15b4bd2168bf00d69f446ca19cae3ca6f752ae"}]' '["cQkEdyskEDiRJdadFgq9DD9yDbc6Kib5YPkNdH6fZX3U4TZxoj7n"]'
```

===================== Result Y2 ====================

0100000001dcc131278f885608d908c5cd5be77ef0c6ba6a174952d767f6686e5fe2a4c82a01000000d90047304402204263c6b3970694a5c3fea4a781bdb2d97e9489b2b70972d1749419a251e2afb602207f5f4aac6d097adfc39ffddafe3321219f5fa6a3fa03ce2258b47bd7fff8e81e01473044022061599deb6773f0fe57ffdad2cd382c16135e106add6ed6b48320969a8b314ef9022048ec961daf7e6dcffc2c6d954b6648a36c1b3f1ba9d9deef6a4874e56beb72db014752210387f89fca6896b35752e37cc3e61c614abfedfb80269a8794283ed40657d9742a210392edcffb03a4b48ac5083ed2bbc05b15b4bd2168bf00d69f446ca19cae3ca6f752aeffffffff01f0a29a3b000000001976a914ef8a388d9996d9b316b6e335183f483493770e6588ac00000000 

=================================================



## (8) Boardcast transaction Y2 signed after (7) and make record.

+ #### command

```
~:bitcoin-cli -datadir=2 sendrawtransaction y3
~:make generate BLOCKS=10
```

+ #### boardcast
+ #### make record

```
~:bitcoin-cli -datadir=2 sendrawtransaction 0100000001dcc131278f885608d908c5cd5be77ef0c6ba6a174952d767f6686e5fe2a4c82a01000000d90047304402204263c6b3970694a5c3fea4a781bdb2d97e9489b2b70972d1749419a251e2afb602207f5f4aac6d097adfc39ffddafe3321219f5fa6a3fa03ce2258b47bd7fff8e81e01473044022061599deb6773f0fe57ffdad2cd382c16135e106add6ed6b48320969a8b314ef9022048ec961daf7e6dcffc2c6d954b6648a36c1b3f1ba9d9deef6a4874e56beb72db014752210387f89fca6896b35752e37cc3e61c614abfedfb80269a8794283ed40657d9742a210392edcffb03a4b48ac5083ed2bbc05b15b4bd2168bf00d69f446ca19cae3ca6f752aeffffffff01f0a29a3b000000001976a914ef8a388d9996d9b316b6e335183f483493770e6588ac00000000
~:make generate BLOCKS=10
```

==================== State ========================

W1_balance: 6299.99987060 
W2_balance: 199.9998 
A1_balance: 189.9999 
A2_balance: 9.9999 
A3_balance: 0 

=================================================

