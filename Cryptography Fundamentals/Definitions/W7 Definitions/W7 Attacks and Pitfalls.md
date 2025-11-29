
## Deterministic RSA Attack
Because RSA is deterministic:
- The same message always encrypts to the same ciphertext
- Attackers can build lookup tables
- IND-CPA security is violated

---

## Low Public Exponent Attacks
If $e$ is small (e.g., $e=3$):
- Certain messages can be recovered using cube roots
- Especially dangerous without padding

---

## No Padding Attack
If RSA is used without padding:
- It is vulnerable to:
  - Chosen-plaintext attacks
  - Algebraic manipulation attacks

---

## Malleability of RSA
Because:
$$
(m_1 \cdot m_2)^e \equiv m_1^e \cdot m_2^e \pmod N
$$

An attacker can modify ciphertexts in a controlled way.

---

## Weak Prime Generation
If:
- $p$ and $q$ are too small
- Not random
- Reused across keys

Then $N$ can be factored easily.

---

## Using RSA for Large Data
RSA is computationally expensive and should not be used to encrypt large messages directly.
