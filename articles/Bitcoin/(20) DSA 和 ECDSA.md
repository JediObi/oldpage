### **DSA 和 ECDSA**



### DSA

非对称加密签名算法 

+ 算法参数
(p,q,g)
```
1. 素数p
φ(p) = p -1

2. 选取一个φ(p)的素因子q

3. 选取 h， 位于[1,p-1]
4. g
g = h^[(p-1)/q] mod p

```

+ 产生密钥对
```
1. 私钥 x
从 [1，q-1] 中选取一个x
2. 公钥 y
y = g^x mod p
```

+ 加解密 验证 

明文 m
```
(1) 私钥签名
  1. 从 [1,q-1] 随机取 k
  2. r = g^k mod p mod q
  3. s = [(H(m) + x * r) * (k^-1)] mod q   //使用私钥
(r,s)即为签名

(2) 公钥验签
  1. w = s^-1 mod q
  2. u1 = [ H(m) * w ] mod q
  3. u2 = (r * w) mod q
  4. v = ( [(g^u1)*(y^u2)] mod p) mod q
 v = r 即成功
```

+ 证明
```
p,q,h,g,x,y
p素数，欧拉函数  φ(p) = p-1
q素数，是φ(p)的素因子
h属于[1,p-1]，h与p互质
g = h^[(p-1)/q] mod p

(1) g^(nq) mod p = 1
证明：
g^(nq) mod p 
= [h^[(p-1)/q] mod p]^(nq) mod p
= h^(p-1) mod p       // 欧拉定理
= 1

(2) g^t mod p = g^(t mod q) mod p
证明：
任意整数 t, n，可构造 t = nq +z
z = t mod q
g^t mod p
= g^(nq+z) mod p
= [(g^nq mod p) * (g^z mod p)] mod p    //利用(1)可得
= [1 * (g^z mod p)] mod p
= g^z mod p
= g^(t mod q) mod p

(3) 证明 v=r
已知：
u1 = [ H(m) * w ] mod q
u2 = (r * w) mod q
v = ( [(g^u1)*(y^u2)] mod p) mod q
将u1,u2带入v
v = ( [(g^u1)*(y^u2)] mod p) mod q
= (( [(g^u1) mod p] * [(y^u2)] mod p]) mod p) mod q
= (( [(g^u1) mod p] * [(y^u2)] mod p]) mod p) mod q
= ([g^((H(m)*w) mod q) mod p] * [(g^x mod p)^[(r * w) mod q] mod p]) mod p mod q
// 应用结论（2）
= ([g^(H(m)*w) mod p] * [(g^[(r * w) mod q] mod p)^x mod p]) mod p mod q
= ([g^(H(m)*w) mod p] * [(g^(r * w) mod p)^x mod p]) mod p mod q
= ([g^(H(m)*w) mod p] * [(g^(r * w)*x mod p]) mod p mod q
= g^[(H(m)*w)+(r * w)*x)] mod p mod q
最终：
v = g^[(H(m)*w)+(r * w)*x)] mod p mod q
= g^[(H(m)+ x*r) * w] mod p mod q

又已知： 
w = s^-1 mod q
所以：
(s * w) mod q
= (s * s^-1 mod q) mod q
= (s * s^-1) mod q
= 1
已知：
 s = [(H(m) + x * r) * (k^-1)] mod q 
所以：
(s * w) mod q
= (([(H(m) + x * r) * (k^-1)] mod q ) * w ) mod q
= ([(H(m) + x * r) * (k^-1)]*w) mod q
=([(H(m) + x * r) * w] * (k^-1)) mod q
=1
既：
([(H(m) + x * r) * w] * (k^-1)) mod q = 1
又因为：
(k * k^-1) mod q = 1
所以：
[(H(m) + x * r) * w] mod q = k mod q

所以：
v = g^[(H(m)+ x*r) * w] mod p mod q
= g^([(H(m)+ x*r) * w] mod q) mod p mod q
= g^(k mod q) mod p mod q
= g^k mod p mod q
= r
```



### ECDSA

仍然是有限域上的椭圆曲线 

+ 椭圆曲线产生密钥对
```
曲线 E
基点 G，阶n
私钥 d，取值范围 [1,n-1]
公钥 Q = dG
{E，G，n，d，Q}
```

+ 产生dsa签名
```
取随机数 k，取值范围 [1,n-1]
kG = (x1, y1)
==>
r = x1 mod n
s = ([H(m)+d*r] * k^-1) mod n
==>
签名
(r,s)
```

+ 验签
```
w = s^-1 mod n
u1 = (H(m)*w) mod n
u2 = (r * w) mod n
u1*G + u2 * Q = (x0, y0)
==>
v = x0 mod n
==>
r = v
```

+ 证明
```
已知：
w = s^-1 mod n
所以：
(s * w) mod n
= (s * [s^-1 mod n]) mod n
= (s * s^-1) mod n
= 1
把 s = ([H(m)+d*r] * k^-1) mod n 带入 (s * w) mod n 得
(s * w) mod n
= [( ( [H(m)+d*r] * k^-1 ) mod n ) * w] mod n
= [ ( [H(m)+d*r] * k^-1 ) * w] mod n
= ([(H(m)*w)+d*r*w] * k^-1) mod n
= [ ( (H(m)*w+d*r*w) mod n) * (k^-1 mod n) ] mod n

∵  ( (H(m)*w+d*r*w) mod n) 
= [(H(m)*w) mod n + (d*r*w) mod n] mod n
= [(H(m)*w) mod n + (d*r*w) mod n] mod n
= [ u1 + ( (d mod n) * ((r * w) mod n) ) mod n] mod n
= [ u1 + ( d* ((r * w) mod n) ) mod n] mod n
= [ u1 + (d * u2) mod n] mod n
= [ u1 mod n +   ((d * u2) mod n) mod n ] mod n
= [ u1 + d * u2 ] mod n

∴
(s * w) mod n
= [ ( (H(m)*w+d*r*w) mod n) * (k^-1 mod n) ] mod n
= [ ( [ u1 + d * u2 ] mod n)  * (k^-1 mod n) ] mod n
= [( u1 + d * u2) * k^-1] mod n
既：
(s * w) mod n 
= [( u1 + d * u2) * k^-1] mod n 
= ([( u1 + d * u2) mod n] * [(k^-1) mod n]) mod n 
= 1
又因为：
(k * k^-1) mod n = [(k mod n)*(k^-1 mod n)] mod n = 1
所以：
( u1 + d * u2) mod n = k mod n
既 ：
u1 + d * u2 = i*n + z
k = j*n +z


已知：
u1*G+u2*Q = (x0, y0)
= u1*G + u2 * d*G
= (u1+d*u2)*G
= (i*n+z)*G             // 基点G的阶为n
= 0 + z * G
所以：
u1*G+u2*Q =(u1+d*u2)*G = 0 + z * G = j * n *G + z * G = kG
所以：
x0 mod n = x1 mod n
既：
 v = r
证明完毕！
```