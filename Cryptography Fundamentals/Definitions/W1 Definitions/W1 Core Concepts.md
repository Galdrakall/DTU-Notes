
---
# Perfect Security
## Definition
A cryptosystem has perfect (information-theoretic) security if the ciphertext gives absolutely no information about the plaintext, even to an attacker with unlimited computational power.

## Formal Definition
Let:
- $M$ be the random variable for plaintexts
- $C$  be the random variable for ciphertexts

Perfect secrecy holds if for all plaintexts $m$ and ciphertexts $c$:

$$
\Pr[M = m \mid C = c] = \Pr[M = m]
$$

That is, observing the ciphertext does not change the probability of any plaintext.

### Equivalent Formulation
For all $m, c$:

$$
\Pr[C = c \mid M = m] = \Pr[C = c]
$$

Meaning: the ciphertext distribution is independent of the plaintext.

## Information-Theoretic Interpretation
Perfect security means:
- The attacker learns **zero bits of information**
- Even an attacker with infinite computing power gains nothing

This is stronger than computational security.


## Example: One-Time Pad
Perfect security is achieved when:
- $k \leftarrow \{0,1\}^n$ uniformly at random
- Encryption:  
$$
c = m \oplus k
$$
- Decryption:
$$
m = c \oplus k
$$

Each plaintext is equally likely for any fixed ciphertext.

## Limitations
- Key length must satisfy:  
$$
|K| \ge |M|
$$
- Secure key distribution is required
- Keys cannot be reused

## Exam Notes
- Perfect security is defined using **conditional probability**
- It does **not rely on computational hardness**
- OTP is the canonical example

---

# Computational Security

## Definition
A cryptosystem has computational security if no **polynomial-time (PPT)** adversary can break it with non-negligible probability.

## Formal Notion of Security
Let $\lambda$ be the security parameter and let $\mathcal{A}$ be any PPT adversary.

A scheme is computationally secure if:

$$
\Pr[\mathcal{A}(\lambda) = 1] \le \text{negl}(\lambda)
$$

where $\text{negl}(\lambda)$ is a negligible function.

## Negligible Function (Formal)
A function  $f : \mathbb{N} \to \mathbb{R}$ is negligible if:

$$
\forall \text{polynomial } p(\lambda), \quad \lim_{\lambda \to \infty} p(\lambda) \cdot f(\lambda) = 0
$$

Examples:
- $2^{-\lambda}$ is negligible
- $\frac{1}{\lambda}$ is **not** negligible

## Security Meaning
- Attacks may exist in theory
- But they require unrealistic resources
- Example: $2^{128}$ operations

## Used In Practice
AES, RSA, Diffie–Hellman, ECC, MACs, signatures

## Exam Notes
- “Secure” always means **negligible attacker advantage**
- Security depends on assumptions about computational power

---

# Worst-Case Attacker Model

## Definition
Assumes the strongest possible attacker consistent with physical and computational limits.

## Formal Quantification
Security statements take the form:

$$
\forall \text{PPT adversaries } \mathcal{A},\quad
\Pr[\mathcal{A} \text{ wins}] \le \text{negl}(\lambda)
$$

This means:
- Security holds against **all efficient attackers**
- Not just specific strategies

## Typical Capabilities
- Full network control
- Adaptive queries
- Unlimited passive observation

## Purpose
- Avoids false security from weak assumptions
- Guarantees robustness

## Exam Notes
- “For all PPT adversaries” always appears in formal definitions