### **Usage - rpc**



### (1) start service

A node could start with by ```bitcoind```
There are some configs of this command.  
You could set this commad through ```bitcoind -server=1```.
Or ```bitcoind -datadir=<directory_path>``` or ```bitcoind -conf=<file_path>``` to specify config file. 

+ bitcoin.conf example

```
# testnet-box functionality
regtest=1
dnsseed=0
upnp=0

# don't listen on a port, just connect to node 1
listen=0
connect=127.0.0.1:19000

# listen on different ports than default testnet
port=19010
rpcport=19011

# always run a server, even with bitcoin-qt
server=1

# enable SSL for RPC server
#rpcssl=1

rpcallowip=0.0.0.0/0

rpcuser=admin2
rpcpassword=123
```
```listen``` default 1;0 don't accept p2p connections from outside. 1 accept. 
```connect``` only connect specified IP:port 
```port``` the port to listen p2p connections from outside. 
```rpcport``` the port to listen rpc connection 
```server``` accept command and rpc command or not 
```rpcuser, rpcpassword``` rpc user info 
```testnet``` use test chain; 0 or 1 
```regtest``` Enter regression test mode, which uses a special chain in which blocks can be solved instantly. This is intended for regression testing tools and app development. 
```dnsseed``` Query for peer addresses via DNS lookup, if low on addresses (default: 1 unless -connect used) 
```rpcallowip=0.0.0.0/0``` Allow JSON-RPC connections from specified source. 



### (2) command on local and rpc

create new transaction 

+ bitcoin-cli

```
~:bitcoin-cli -conf=/home/user/bitcoin.conf createrawtransaciton '[{"txid":"xxx", "vout":1}]' '{"address1":10.00, "address2":4.99999}'
```

+ bitcoin-cli

```
~:bitcoin-cli -datadir=/home/user/bitcoin1 createrawtransaciton '[{"txid":"xxx", "vout":1}]' '{"address1":10.00, "address2":4.99999}'
```

+ curl rpc

```
 ~:curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "createrawtransaction", "params": ["[{\"txid\":\"myid\",\"vout\":0}]", "{\"data\":\"00010203\"}"] }' -H 'content-type: text/plain;' http://127.0.0.1:19011/
```