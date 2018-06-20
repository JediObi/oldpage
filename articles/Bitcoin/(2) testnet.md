### **Testnet on Docker**

(1) pull bitcoin testnet image
```
~:docker pull freewil/bitcoin-testnet-box
```
(2) start a testnet container
```
~:docker run -t -i -p 19001:19001 -p 19011:19011 freeewil/bitcoin-testnet-box
```
(3) start testnet
```
~:make start
```
(4) getinfo
```
~:make getinfo
```
(5) generate 1 block
```
~:make generate
```
(6) generate n blocks
```
~:make generate BLOCKS=200
~:make getinfo
```
(7) transfer some money
generate a transaction.
you need to generate blocks to make transactions effective.
```
~:make sendfrom1 ADDRESS=xxxxxxxx AMOUNT =10
~:make generate BLOCKS=10
~:make getinfo
```