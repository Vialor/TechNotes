# History
## Substitution Ciphers: Caesar Cipher

> Substitution Ciphers / Monoalphabetic Ciphers: change the symbols of letters
```Plain
shift the alphabet
encode(key, p) = (p + key) % 26
decode(key, c) = (c - key) % 26
```
Key Space: 25 rotations (~5 bits security)
## Polyalphabetic Ciphers: Vigenère Cipher
> Polyalphabetic Ciphers: like a substitution method, but use multiple substitution alphabets 

Repeat key till it has the same length of the plaintext. Combine the repeated key and plaintext by adding up the corresponding letters and mod 26.
## Transposition Cipher: Columnar Transposition Cipher
> Transposition Ciphers / Permutation Ciphers: switch letters around a permutation

Put plaintext in a matrix and then use the transpose of the matrix
## One-Time Pad: Vernam Cipher
> One-Time Pad: the key changes for each encryption

pseudo solution:
	use a large book as the key
	new key is the initial key followed by previous messages themselves
	random number sequence based on a seed
# Other Techniques
**Steganography**: hiding data inside other data to avoid detection
**Covert Channel**: message is hidden within the traffic of an overt communication channel through steganography