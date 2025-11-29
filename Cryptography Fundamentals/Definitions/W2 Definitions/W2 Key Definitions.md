## Plaintext
The original readable message before encryption. Denoted by $m$.

## Ciphertext
The encrypted output of the encryption algorithm. Denoted by $c$.

## Key
A secret value used for both encryption and decryption. Denoted by $k$.

## Symmetric-Key Encryption Scheme (SKE)
A cryptographic scheme with three algorithms:
- Key generation: $k \leftarrow \text{KeyGen}()$
- Encryption: $c = \text{Enc}_k(m)$
- Decryption: $m = \text{Dec}_k(c)$

Correctness requirement:
$$
\Pr[\text{Dec}_k(\text{Enc}_k(m)) = m] = 1
$$

## Perfect Secrecy
An encryption scheme has perfect secrecy if observing the ciphertext gives no information about the plaintext.

Formally:
$$
\Pr[M=m \mid C=c] = \Pr[M=m]
$$

Equivalent form:
$$
\Pr[C=c \mid M=m] = \Pr[C=c]
$$

## Information-Theoretic Security
Security that holds even against an attacker with unlimited computational power.

## Computational Security
Security that holds only against efficient (polynomial-time) attackers.