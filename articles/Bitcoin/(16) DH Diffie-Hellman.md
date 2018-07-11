### **DH Diffie-Hellman**



DH算法的安全性依赖于计算离散对数的难度。双向公钥交换，再各自本地产生相同的对称秘钥，免去了秘钥传输过程。 

1，有两个全局公开的参数，一个素数q和一个整数a,a是q的一个原根. 

2，假设用户A和B希望交换一个密钥，用户A选择一个作为私有密钥的随机数XA(XA<q)，并计算公开密钥YA=a^XA mod q。A对XA的值保密存放而使YA能被B公开获得。类似地，用户B选择一个私有的随机数XB<q，并计算公开密钥YB=a^XB mod q。B对XB的值保密存放而使YB能被A公开获得. 

3，用户A产生共享秘密密钥的计算方式是K = (YB)^XA mod q.同样，用户B产生共享秘密密钥的计算是K = (YA)^XB mod q.这两个计算产生相同的结果： K = (YB)^XA mod q = (a^XB mod q)^XA mod q = (a^XB)^XA mod q （根据取模运算规则得到） = a^(XBXA) mod q = (a^XA)^XB mod q = (a^XA mod q)^XB mod q = (YA)^XB mod q 因此相当于双方已经交换了一个相同的秘密密钥K. 

4，因为XA和XB是保密的，一个敌对方可以利用的参数只有q,a,YA和YB.因而敌对方被迫取离散对数来确定密钥.例如，要获取用户B的秘密密钥，敌对方必须先计算 XB = inda,q(YB) 然后再使用用户B采用的同样方法计算其秘密密钥K.  Diffie-Hellman密钥交换算法的安全性依赖于这样一个事实：虽然计算以一个素数为模的指数相对容易，但计算离散对数却很困难.对于大的素数，计算出离散对数几乎是不可能的. 下面给出例子.密钥交换基于素数q = 97和97的一个原根a = 5.A和B分别选择私有密钥XA = 36和XB = 58.每人计算其公开密钥 YA = 5^36 = 50 mod 97 YB = 5^58 = 44 mod 97 在他们相互获取了公开[密钥]之后，各自通过计算得到双方共享的秘密密钥如下： K = (YB)^XA mod 97 = 44^36 = 75 mod 97 K = (YA)^XB mod 97 = 50^58 = 75 mod 97 从|50,44|出发，攻击者要计算出75很不容易. 

5，攻击者知道a,q,YA,YB，试图获取对称秘钥 K，
由 K = (YB)^XA mod q 可知攻击者需要计算XA，
由 YA=a^XA mod q 可知 XA = log<sub>a</sub> (n * q + YA)，而计算过程只能是暴力破解尝试。 


```
1. 素数p，原根a, (a mod p)! = φ(p)
2. 用户A，选择私钥 XA(XA<p), 公钥 YA = a^XA mod p；
   用户B，选择私钥 XB(XB<p), 公钥 YB = a^XB mod p；
3. 产生对称秘钥K 
   用户A计算 K = (YB)^XA mod p
   用户B计算 K = (YA)^XB mod p
4.证明 
(YB)^XA mod p        //用户A使用私钥XA计算
=(a^XB mod p)^XA mod p
=(a^XB)^XA mod p
=(a^XA)^XB mod p
=(a^XA mod p)^XB mod p
=(YA)^XB mod p    //用户B使用私钥XB计算
```

