# Introduction to ECC & ECDSA

## ECC
T(p, a, b, G, n)
y^2 = x^3 + ax + b mod p

G:基點
n:order(G)=n
EX: when 3^n mod 7 = 3^1
循環體

離散數學
field
針對field 定義兩個 operations

加法
G · Q = G + Q

乘法
G · G = 2G

G^d = dG = Q
Q 是隨機的
知道d => 容易得到Q
知道Q => 很難得到d
d: private key 應用： d = sha3(str)
Q: public key

## ECDSA
加密：機密性
簽章：不可否認性

### 用ECC簽章
sign(h(m)) = (s, r)
n = Ord(G)
1. pick random k where (n-1 >= k >= 1)
2. compute kG = (x1, y1), r = x1 mod n, go back to 1 if r = 0
3. s = (k^-1)(h(m) + dr) mod n, go back to 1 if s = 0

KG即乘法運算(G·G·G·G...K次)

### 驗簽章
verify(m, s, r) = (ture, if v = r) or (false, otherwise)

1. w = (s^-1) mod n
2. u1 = h(m)w mod n
   u2 = rw mod n
3. u1·G + u2·Q = (x0, y0)
   v = x0 mod n
