Encryption: plaintext → ciphertext
Decryption: ciphertext → plaintext
Cryptographic Algorithm: methods to do encryption and decryption
Cryptographic Key: an input variable used by cryptographic algorithms to help the transformation
N-bit Security Entropy (Key Space): The number of bits necessary to encode the number of possible keys
# Assumptions on attackers’ resources
> **The Kerckhoffs’ Principle**: A cryptosystem should be secure even if everything about the system, except the key, is public knowledge. “No Security By Obscurity”

**Exhaustive Search**: brute force
**Ciphertext Only**: know many ciphertexts
**Known Plaintext**: know certain plaintexts to ciphertexts mapping
**Chosen Plaintext**: the ability to choose plaintexts and to view their corresponding encryptions
**Chosen Ciphertext**: : the ability to choose ciphertexts and to view their corresponding plaintexts

Goal: f(Data, Secret) => Output (fixed length): computationally infeasible to figure out Secret if the attackers have Data, Output and f
# Basic Approaches
**Confusion**: The cryptanalyst should not be able to predict what changing one character in the plaintext will do to the ciphertext.
**Diffusion**: Changes in the key should affect many parts in the ciphertext.
**Randomization**: Encryptions of the same texts are different from time to time
# Cryptography Methods
[[Classical Cryptography]]
[[Symmetric Cryptography]]: good for bulk amounts of data
[[Asymmetric Cryptography]]: good for authentication