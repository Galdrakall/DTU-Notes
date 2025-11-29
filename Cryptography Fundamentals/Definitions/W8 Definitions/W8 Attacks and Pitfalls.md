
## Man-in-the-Middle (MITM) Attack on DH
If DH is unauthenticated:
- An attacker intercepts $A$ and $B$
- Replaces them with their own values
- Establishes two separate shared keys

Both parties think they share a key, but they do not.

---

## Lack of Authentication
Diffie–Hellman alone does NOT authenticate identities.
It must be combined with:
- Digital signatures
- Certificates
- PSKs

---

## Reusing ElGamal Randomness
If the same $r$ is reused:
- Relationships between messages leak
- Security collapses

---

## Small Subgroup Attacks
If:
- The group has small subgroups
- Parameters are not validated

Attackers can recover bits of the secret key.

---

## Weak Group Parameters
Using:
- Small primes
- Non-safe groups

Makes discrete logarithms feasible.

---

## Direct ElGamal Malleability
ElGamal ciphertexts are multiplicatively malleable:
$$
(c_1, c_2) \to (c_1, \alpha \cdot c_2)
$$
This changes the plaintext in a predictable way.
