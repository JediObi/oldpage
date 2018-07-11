### **Json RPC**



If a server runs with json-rpc.
Anyone may send request to this server in json format. 
```json
{
    "jsonrpc" : 2.0,
    "method" : "sayHello", 
    "params" : ["Hello JSON-RPC"], 
    "id" : 1
}
```
Just like an url ```http://localhost:8080/test_project/rpc``` with body```{"method":"sayHello","params":[],"id":1234}```. 
A json-rpc calling is just like a traditional http request.
Because we use json or key-value format to transform our message in http server.
