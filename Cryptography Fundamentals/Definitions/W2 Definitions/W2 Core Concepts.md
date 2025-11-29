## Classical Ciphers
Early encryption methods that rely on simple substitutions or shifts.
They are insecure due to small key spaces and statistical patterns.

---

## Caesar Cipher
A substitution cipher where each letter is shifted by a fixed amount.

Encryption:
$$
c = (m + k) \bmod 26
$$

Decryption:
$$
m = (c - k) \bmod 26
$$

Main weaknesses:
- Only 26 possible keys
- Broken by brute force
- Broken by frequency analysis

---

## Vigenère Cipher
A polyalphabetic substitution cipher using a repeating key.

Weaknesses:
- Key repetition leaks structure
- Vulnerable to frequency analysis (Kasiski test)
- Not securely random

---

## One-Time Pad (OTP)

Key generation:
$$
k \leftarrow \{0,1\}^n \text{ uniformly at random}
$$

Encryption:
$$
c = m \oplus k
$$

Decryption:
$$
m = c \oplus k
$$

Core properties:
- XOR is its own inverse
- Each ciphertext is uniformly distributed
- Provides perfect secrecy

---

## Why OTP is Different
- Security does not rely on computational hardness
- Security is purely probabilistic
- Unbreakable even with infinite computing power