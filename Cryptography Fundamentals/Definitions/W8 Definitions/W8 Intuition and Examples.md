
## Intuition: Public Paint Mixing Analogy
- Public color = $g$
- Alice mixes with secret color $a$
- Bob mixes with secret color $b$
- When they mix again, both get the same final color $g^{ab}$

Eavesdroppers never see $a$ or $b$.

---

## Intuition: Why Exponentiation Is Hard to Undo
Computing:
$$
g^x
$$
is easy.
Finding $x$ from $g^x$ is the discrete log problem and is hard.

---

## Example: Diffie–Hellman with Small Numbers

Public:
$$
p = 23, \quad g = 5
$$

Alice:
$$
a = 6, \quad A = 5^6 \bmod 23 = 8
$$

Bob:
$$
b = 15, \quad B = 5^{15} \bmod 23 = 19
$$

Shared key:

Alice:
$$
K = 19^6 \bmod 23 = 2
$$

Bob:
$$
K = 8^{15} \bmod 23 = 2
$$

Shared key:
$$
K = 2
$$

---

## Intuition: Why ElGamal Needs Randomness
If ElGamal were deterministic:
- Same message would encrypt to the same ciphertext
- IND-CPA security would fail

Random $r$ ensures different ciphertexts for the same message.

---

## Example: ElGamal High-Level Flow
- Public key: $h = g^x$
- Encrypt using $r$:
  $$
  (g^r, m \cdot g^{rx})
  $$
- Decrypt by dividing out $g^{rx}$
