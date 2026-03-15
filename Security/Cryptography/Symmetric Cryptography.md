# Concept
> key k is used for both encryption and decryption

**one-way function**: a → b, easy to calculate b from a, but difficult to find a from b
Ek is an injection with inverse Dk
Ek(m) and Dk(c) are one way functions
### XOR Cipher
Ek(m) = k xor m
Dk(c) = k xor c
susceptible to known plaintext attacks
# 2 Types of Symmetric Cryptography
## Stream Cipher
> encrypt info one bit/byte at a time
> susceptible to known plain-text attacks, reused key attacks

Ek(m) = key_stream(s) xor m
```Plain
E(A) = A xor C
E(B) = B xor C
=>
E(A) xor E(B) = (A xor C) xor (B xor C) = A xor B xor C xor C = A xor B
Even if neither message is known, as long as both messages are in a natural language,
such a cipher can often be broken by paper-and-pencil methods
```
### Key stream: RNG random number generator
**RC4** (deprecated)
**Rivest Cipher4/ARC4**
WEP Wired Equivalent Privacy
RC4_key = IV + SSID_password (IV is a 24-bit random number)
**Salsa20**
## Block Cipher
> info broken down into blocks of fixed size, chunks are encrypted individually (chunk size remains after the encryption) and then chained together

**DES Data Encryption Standard**
56-bit key
**AES Advanced Encryption Standard**
128/192/256-bit key
### Electronic Code Book ECB
All blocks use the same key for encryption
### Cipher Block Chaining CBC
Initialization Vector IV, still use an ECB

plain text block: m; encrypted text block: c
c(0) = apply_key(IV xor m(0))
c(i) = apply_key(c(i-1) xor m(i))