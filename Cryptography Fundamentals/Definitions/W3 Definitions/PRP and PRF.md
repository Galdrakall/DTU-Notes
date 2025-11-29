## 1. Setup and Notation

- Security parameter: $\lambda \in \mathbb{N}$
- Key space: $\mathcal{K}$
- Domain: $\mathcal{X}$
- Range: $\mathcal{Y}$
- PPT: probabilistic polynomial-time (efficient) algorithms

A cryptographic **adversary** is always modeled as a PPT algorithm $\mathcal{A}$.

---

## 2. Negligible Functions and Advantage

### Negligible Function

A function $f : \mathbb{N} \to \mathbb{R}$ is negligible if:

$$
\forall \text{polynomials } p(\lambda), 
\quad \lim_{\lambda \to \infty} p(\lambda)\cdot f(\lambda) = 0.
$$

We write this as: $f(\lambda) = \text{negl}(\lambda)$.

---

### Adversary Advantage (General Form)

For many security games, the **advantage** of an adversary $\mathcal{A}$ is:

$$
\text{Adv}_\mathcal{A}^{\text{game}}(\lambda) 
= \left| \Pr[\mathcal{A} \text{ wins}] - \tfrac12 \right|.
$$

A scheme is secure if for **all PPT** $\mathcal{A}$:

$$
\text{Adv}_\mathcal{A}^{\text{game}}(\lambda) \le \text{negl}(\lambda).
$$

---

## 3. PRF – Formal Definition and Security Game

### 3.1 PRF Informal Definition

A **Pseudorandom Function (PRF)** is a keyed function that:
- Is deterministic
- Takes an input $x \in \mathcal{X}$
- Outputs $y \in \mathcal{Y}$
- Looks like a truly random function to any efficient adversary without the key

We write:
$$
F : \mathcal{K} \times \mathcal{X} \to \mathcal{Y}, 
\quad F_k(\cdot) := F(k,\cdot).
$$

---

### 3.2 PRF Security Game (Real vs Random)

We define two oracles for the adversary:

- **Real world (PRF):**
  - Sample $k \leftarrow \mathcal{K}$ uniformly.
  - Oracle answers queries $x$ with $F_k(x)$.

- **Random world (truly random function):**
  - Sample a random function $f : \mathcal{X} \to \mathcal{Y}$.
  - Oracle answers queries $x$ with $f(x)$.

The adversary $\mathcal{A}$ interacts with one of these oracles and outputs a bit $b'$.

We define:

$$
\text{Adv}_\mathcal{A}^{\text{PRF}}(\lambda)
=
\left|
\Pr[\mathcal{A}^{F_k(\cdot)}(\lambda) = 1]
-
\Pr[\mathcal{A}^{f(\cdot)}(\lambda) = 1]
\right|.
$$

**PRF security**: For all PPT $\mathcal{A}$,

$$
\text{Adv}_\mathcal{A}^{\text{PRF}}(\lambda) \le \text{negl}(\lambda).
$$

---

### 3.3 Equivalent “Bit-Guessing” Formulation

Alternatively, choose a random bit $b \in \{0,1\}$:

- If $b = 0$ use a random function oracle $f(\cdot)$
- If $b = 1$ use PRF oracle $F_k(\cdot)$

Adversary $\mathcal{A}$ outputs $b'$.

Advantage:

$$
\text{Adv}_\mathcal{A}^{\text{PRF}}(\lambda)
=
\left|
\Pr[b'=b] - \tfrac12
\right|.
$$

This formulation is equivalent to the one above.

---

## 4. PRP – Formal Definition and Security Game

### 4.1 PRP Informal Definition

A **Pseudorandom Permutation (PRP)** is a keyed permutation that:
- Is bijective on a finite domain
- Is efficiently invertible given the key
- Looks like a random permutation (and its inverse looks like a random inverse)

We write:

$$
E : \mathcal{K} \times \mathcal{X} \to \mathcal{X},
$$

with:
- For each $k$, $E_k(\cdot)$ is a permutation on $\mathcal{X}$.
- There exists an efficient inverse $D_k(\cdot)$ such that:
  $D_k(E_k(x)) = x$ for all $x$.

Example: AES is modeled as a PRP on $\{0,1\}^{128}$.

---

### 4.2 PRP Security Game (Real Permutation vs Random Permutation)

We define two worlds:

- **Real world (PRP):**
  - Sample $k \leftarrow \mathcal{K}$.
  - Oracle answers $E_k(x)$ and also an inverse oracle $D_k(y)$ if needed.

- **Random world (uniform random permutation):**
  - Sample a random permutation $\pi$ over $\mathcal{X}$.
  - Oracle answers $\pi(x)$ and $\pi^{-1}(y)$.

The adversary interacts with one of these oracles and outputs a bit indicating which world it believes it is in.

Advantage:

$$
\text{Adv}_\mathcal{A}^{\text{PRP}}(\lambda)
=
\left|
\Pr[\mathcal{A}^{E_k, D_k}(\lambda) = 1]
-
\Pr[\mathcal{A}^{\pi, \pi^{-1}}(\lambda) = 1]
\right|.
$$

**PRP security**: For all PPT $\mathcal{A}$,

$$
\text{Adv}_\mathcal{A}^{\text{PRP}}(\lambda) \le \text{negl}(\lambda).
$$

---

## 5. Relationship Between PRFs and PRPs

### 5.1 PRP is a Special Case of PRF

Fix a PRP $E_k$ over domain $\mathcal{X}$.

If we only ever query $E_k$ (and not its inverse), we can treat it as a function:
$$
F_k(x) := E_k(x).
$$

Intuition:
- From the adversary’s view, $E_k$ behaves like a PRF on $\mathcal{X}$.
- The only difference from a random function: a random function does not have to be a permutation.

The **PRP–PRF switching lemma** quantifies how close these views are.

---

## 6. PRP–PRF Switching Lemma (High Level)

### Lemma (Informal)

Let an adversary make at most $q$ queries to an oracle.  
Then the view of the adversary when interacting with:

- A truly random function $f$  
vs.  
- A random permutation $\pi$

differs in advantage by at most about:

$$
\frac{q^2}{|\mathcal{X}|}.
$$

So, as long as $q^2 \ll |\mathcal{X}|$, the difference is negligible.

---

### Why This Matters

- Analyzing PRPs directly is harder.
- It is often simpler to:
  1. Model the block cipher as a random function.
  2. Prove security in the PRF model.
  3. Use the switching lemma to carry the result over to PRPs.

This is a standard technique in block cipher–based constructions (e.g., modes of operation).

---

## 7. Proof Sketch: PRF Security (Hybrid Argument)

Goal: Show that no PPT adversary can distinguish $F_k$ from a truly random function.

### Basic Hybrid Idea

1. **Hybrid 0**: Oracle is $F_k(\cdot)$.
2. **Hybrid 1**: Oracle is a random function $f(\cdot)$.

If we can show that for any PPT adversary $\mathcal{A}$:

$$
\left|
\Pr[\mathcal{A}^{F_k} = 1]
-
\Pr[\mathcal{A}^{f} = 1]
\right| \le \text{negl}(\lambda),
$$

then $F$ is a secure PRF.

### Typical Structure
- Assume there exists an adversary that distinguishes them with non-negligible advantage.
- Build a reduction $\mathcal{R}$ that uses $\mathcal{A}$ to break some underlying hardness assumption.
- Conclude that $F$ must be secure as long as the assumption holds.

---

## 8. PRF-Based Constructions (Formulas)

### 8.1 PRF-based Stream Cipher (CTR-like)

Given a PRF $F_k$ and nonce/counter values $r, r+1, \dots$:

Key stream blocks:
$$
K_i = F_k(r + i)
$$

Encryption of message blocks $m_i$:
$$
c_i = m_i \oplus K_i = m_i \oplus F_k(r + i)
$$

Decryption:
$$
m_i = c_i \oplus F_k(r + i)
$$

Security relies on:
- $F_k$ being secure PRF
- Nonce/counter $r$ never repeating under the same key

---

### 8.2 PRF-based MAC (Basic Form)

Given a PRF $F_k$:
$$
\text{Tag} = F_k(m)
$$

Security:
- If $F_k$ is a secure PRF, then forging a new $(m, \text{Tag})$ pair is hard.

(This is oversimplified; actual MACs often use length encoding, domain separation, etc.)

---

## 9. Attacks When PRFs / PRPs Are Weak

### Distinguishing Attack

If an adversary can find patterns such that:

$$
\left|
\Pr[\mathcal{A}^{F_k} = 1]
-
\Pr[\mathcal{A}^{f} = 1]
\right| \ge \epsilon(\lambda)
$$

for non-negligible $\epsilon(\lambda)$, then:
- The PRF is **not secure**.
- All constructions built on top may be broken.

---

### Non-Random-Looking Block Cipher

If a block cipher:
- Has non-uniform distribution of outputs
- Preserves some structure
- Has detectable bias

Then it fails as a PRP/PRF.

Attackers can:
- Distinguish it from random
- Break higher-level constructions (e.g., modes, MACs)

---

## 10. Summary Cheat Sheet

- PRF: $F_k$ looks like a random function.
- PRP: $E_k$ is a bijective, invertible PRF (a permutation).
- Advantage is always measured as a distinguishing gap.
- PRF security: no PPT adversary can distinguish $F_k$ from random.
- PRP security: no PPT adversary can distinguish $E_k$ (and $D_k$) from a random permutation (and its inverse).
- PRP–PRF Switching Lemma:
  - Random permutation $\approx$ random function, as long as $q^2/|\mathcal{X}|$ is small.
- Block ciphers (AES) are modeled as PRPs, which can be treated as PRFs via switching.
- PRFs are used for:
  - Stream ciphers
  - MACs
  - Key derivation
  - Many symmetric constructions

---