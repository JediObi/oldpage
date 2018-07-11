### **DataStructure - address**



public key ==> pubKey 32 bytes 

HASH160(SHA256(pubKey)) ==> H(m) 20 bytes 

```version``` 1 byte 
version + H(m) 21 bytes 

Base58(SHA256(SHA256(version+H(m)))) ==> get the first 4 bytes ==> checksum 

```address``` = Base58(version + H(m) + checksum) 

