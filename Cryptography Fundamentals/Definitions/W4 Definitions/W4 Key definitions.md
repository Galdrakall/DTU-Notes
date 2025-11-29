## Block Cipher
A deterministic, keyed permutation:
$$
E_k : \{0,1\}^n \to \{0,1\}^n
$$
that is efficiently invertible with inverse:
$$
D_k(E_k(x)) = x
$$

## Block Size
The fixed length $n$ (in bits) of each input and output block.

## Key Size
The length (in bits) of the secret key $k$.

## Pseudorandom Permutation (PRP)
A block cipher that is computationally indistinguishable from a uniform random permutation.

## Mode of Operation
A method for encrypting long messages using a block cipher on fixed-size blocks.

## Initialization Vector (IV)
A random or unpredictable value used to randomize encryption.

## Nonce
A value that must be unique (never repeated) under the same key.

## Electronic Codebook (ECB)
A mode where each block is encrypted independently:
$$
c_i = E_k(m_i)
$$

## Cipher Block Chaining (CBC)
A mode where each block depends on the previous ciphertext:
$$
c_i = E_k(m_i \oplus c_{i-1})
$$

## Counter Mode (CTR)
A mode that turns a block cipher into a stream cipher:
$$
c_i = m_i \oplus E_k(\text{ctr} + i)
$$