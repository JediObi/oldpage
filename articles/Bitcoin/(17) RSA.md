### **RSA**



RSA安全性依赖于大数分解质因子的难度。单向的公钥加密系统，一方产生秘钥对，再传播公钥，之后双方加密传输对称秘钥。 

+ 产生密钥对
```
1，N=p∗q，p，q为两个质数，则φ(N)=(p-1) * (q-1)
2，选取公钥，取任意整数e，e<φ(N)，且e与φ(N)互质
3，产生私钥，取d，使d满足（d * e）mod φ(N) = 1

```

+ 加解密过程
```
公钥(e,N)，秘钥(d,N)
用户对明文A使用公钥加密
B=A^e mod N
对密文B使用秘钥解密
A=B^d mod N
```

+ 证明如下
```
A=B^d mod N
 =(A^e mod N)^d mod N
 =(A^e)^d mod N
 =A^(d * e) mod N
 =A^(k * φ(N) +1) mod N
 =[A * (A^φ(N))^k] mod N
 =[ A mod N * ((A^φ(N))^k) mod N] mod N
 =[ A mod N * (((A^φ(N)) mod N)^k) mod N)] mod N  //由同余式法则
 =[ A mod N * (1^k) mod N] mod N      //由欧拉定理可得(A^φ(N)) mod N = 1
 =[A mod N] mod N
 =A 
注：为什么(A^φ(N)) mod N = 1满足欧拉定理，因为A远小于大质数p，q，而N只能分解为p * q，所以A必然与N互质，所以满足以上。
```

