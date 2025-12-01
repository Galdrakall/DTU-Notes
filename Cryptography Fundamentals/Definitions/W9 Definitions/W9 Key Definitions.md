
## Digital Signature Scheme
A digital signature scheme consists of three algorithms:

- KeyGen: $(pk, sk) \leftarrow \text{KeyGen}()$
- Sign: $\sigma = \text{Sign}_{sk}(m)$
- Verify: $\text{Verify}_{pk}(m, \sigma) \in \{0,1\}$

---

## Correctness
For all messages $m$:
$$
\Pr[\text{Verify}_{pk}(m, \text{Sign}_{sk}(m)) = 1] = 1
$$

---

## Unforgeability (EUF-CMA)
A signature scheme is **existentially unforgeable under chosen-message attack** if no PPT adversary can forge a valid signature on a new message.

---

## Non-Repudiation
The property that a signer cannot later deny having signed a message.

---

## Hash-and-Sign Paradigm
A construction where:
1. The message is hashed
2. The hash is signed with a signing algorithm

---

## RSA Signatures
A signature scheme based on RSA:
$$
\sigma = H(m)^d \bmod N
$$

---

## DSA / ECDSA
Signature schemes based on the discrete logarithm problem (and elliptic curves for ECDSA).
