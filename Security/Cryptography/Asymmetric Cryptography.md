> also known as public key cryptography
# Concept
> use one key for encryption and other for decryption  
> encryption of public/private key can be decrypted by the private/public key  

**public key:** owned by one, known by all
encryption using public key: confidential messages: verify the reader of the message
**private key:** owned by one, known by none
Use Case:
	Confidential messages: encryption with reader public key, verify the reader of the message with reader private key
	Digital signature: encryption with writer private key, verify the writer of the message with writer public key
  
**RSA Rivest-Shamir-Adleman encryption**
Choose large primes p, q, n = p×q
ϕ(n)=(p−1)×(q−1)
choose e < ϕ(n), usually 65537
calc private key d such that d×emodϕ(n)=1
加密过程：
1. 假设M是一个明文消息，首先将M转换为一个整数m（0≤m<n）
2. 加密后的密文c计算为c = m^e mod  n
解密过程：
1. 使用私钥ddd对密文ccc进行解密，计算m = c^d mod  n
2. 将整数m转换回消息M
# Establish Shared Key for Symmetric Cryptography
**key exchange**: an algorithm that allow A and B agree on a key without sending the key per se.

**Diffie-Hellman Key Exchange**
**Modular Exponentiation**: 
`f(x) = b**x mod m` (b base and m modulus are public)  
`g(fx, y) = fx**y mod m`
note that `(b**x mod m)**y mod m == b**xy mod m`