
## Collision Attacks
Used to:
- Break digital signatures
- Forge commitments
- Undermine certificates

Example: Practical collisions found for MD5 and SHA-1.

---

## Length-Extension Attacks
Affect constructions like:
$$
\text{MAC}_k(m) = H(k \parallel m)
$$
Attacker can compute:
$$
H(k \parallel m \parallel m')
$$
without knowing $k$.

---

## Unsalted Password Hashes
If passwords are hashed as:
$$
H(p)
$$
then:
- Same passwords → same hashes
- Vulnerable to rainbow tables
- Offline dictionary attacks are trivial

---

## Fast Hashes for Password Storage
SHA-256 is too fast for password storage.
Attackers can test billions of guesses per second.

---

## Using Hash as Encryption
Hash functions are not invertible in a controlled way.
They cannot be used to encrypt and later recover plaintext.

---

## Broken Hash Functions
Do NOT use:
- MD5
- SHA-1
for security-critical purposes.
