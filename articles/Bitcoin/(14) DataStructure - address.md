### **DataStructure - address**

<hr>

public key ==> pubKey 32 bytes <br>

HASH160(SHA256(pubKey)) ==> H(m) 20 bytes <br>

```version``` 1 byte <br>
version + H(m) 21 bytes <br>

Base58(SHA256(SHA256(version+H(m)))) ==> get the first 4 bytes ==> checksum <br>

```address``` = Base58(version + H(m) + checksum) <br>

<hr>