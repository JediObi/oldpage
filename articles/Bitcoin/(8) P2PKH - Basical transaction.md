### **P2PKH - Basical transaction**

The creation of a basical transaction. <br>
==> To create a transaction, you need addresses for output position. <br>
==> The basical transaction is used to send some btc from one address to another address. <br>
==> Don't forget the address for change.
The recommendation of the prevoius output. <br>
==> you have to use a previous utxo and sign it with your private key.

<hr>

## (0) Init Test Environment

+ #### generate 200 blocks at node 1

```~:make generate BLOCKS=200``` <br>
==================== State =======================

Wallet at Node1 -> W1 <br>
Wallet at Node2 -> W2 <br>
W1_balance: 5000 <br>
W2_balance: 0 <br>

================================================

+ #### generate 2 new addresses at node2 and get their private keys

```
~:make address2
~:make address2
~:bitcoin-cli -datadir=2 dumpprivkey <address>
```

==================== Result =======================

Address1 -> A1 <br>
Address2 -> A2 <br>
A1_hash: moX3qTsaLt9NjvQBGHy78jvmLZX9ZWk7aP  <br>
A1_privKey: cN3ggP6FEmQF9zWt2FUNP8rxCLxNLBCgRcmWfD7jtqAvx1Geud7w <br>
A2_hash: n3MXNfADQRYwwJ4iwoNLMWxA3bwba7hsiU  <br>
A2_privKey: cQkEdyskEDiRJdadFgq9DD9yDbc6Kib5YPkNdH6fZX3U4TZxoj7n <br>

=================================================

=================== State =========================

W1_balance: 5000 <br>
W2_balance: 0 <br>
A1_balance:  0 <br>
A2_balance:  0 <br>

=================================================

+ #### send 200 btc from node1 to node2

send 200 btc to ```A1```. <br>
```
~:make sendfrom1 ADDRESS=moX3qTsaLt9NjvQBGHy78jvmLZX9ZWk7aP AMOUNT=200
~:make generate BLOCKS=10
```

==================== State ========================

W1_balance: 5299.99987060 <br>
W2_balance: 200 <br>
A1_balance: 200 <br>
A2_balance: 0 <br>

=================================================

<hr>

## (2) Create transaction

Create transaction ```X``` that sends 10 coin to A1 from A2. <br>

+ #### command
```
# show utxo
~:bitcoin-cli -datadir=2 listunspent
# create raw transaction
~:bitcoin-cli -datadir=2 createrawtransaction '[{"txid":"", "vout":, <"sequence":>}]' '{"A2":189.9999,"A1":10}'
```
Here A2 was used to receive change in output, you can also use a new address. <br>

+ #### create transaction X

```
~:bitcoin-cli -datadir=2 listunspent
~:bitcoin-cli -datadir=2 createrawtransaction '[{"txid":"9fceca794c2c156ee3a2c56dcec96be6dd6a95339057262d972e96931f687b2e", "vout":1}]' '{"moX3qTsaLt9NjvQBGHy78jvmLZX9ZWk7aP":189.9999,"n3MXNfADQRYwwJ4iwoNLMWxA3bwba7hsiU":10}'
```
==================== Result X ======================

unsigned_hex <br>

=================================================

<hr>

## (3) Use private key 1 to sign X
Sign X and get signed raw transaction X1. <br>
+ #### command
```
~:bitcoin-cli -datadir=2 signrawtransaction x '[]' '["A1_privKey"]'
```
+ #### sign X

generate X1 <br>

```
~:bitcoin-cli -datadir=2 signrawtransaction <unsigned_hex> '[]' '["cN3ggP6FEmQF9zWt2FUNP8rxCLxNLBCgRcmWfD7jtqAvx1Geud7w"]'
```
==================== Result X1 =====================

signed_hex

=================================================

<hr>

## (4) Boardcast X1 and make record

+ #### command
```
~:bitcoin-cli -datadir=2 sendrawtransaction X1 
~:make generate BLOCKS=10
```

+ #### boardcast and make record
```
~:bitcoin-cli -datadir=2 sendrawtransaction <signed_hex>
~:make generate BLOCKS=10
```

===================== Result ======================

X1 txid: X1_hash <br>

=================================================

==================== State ========================

W1_balance: 5799.99987060 <br>
W2_balance: 189.9999 <br>
A1_balance: 189.9999 <br>
A2_balance: 10 <br>

=================================================

<hr>