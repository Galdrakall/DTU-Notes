---
title: "Week 3 Notes – Pseudorandomness & PRFs"
week: 3
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/3, pseudorandomness]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_2_Notes]]"
  next: "[[Week_4_Notes]]"
---

# Week 3 – Pseudorandomness & PRFs

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_2_Notes]] · Next: [[Week_4_Notes]]

## Negligible functions

A function $\mu : \mathbb{N} \to \mathbb{R}$ is **negligible** if:

> For every polynomial $p(n)$, there exists $N$ such that for all $n > N$:
> $$
> |\mu(n)| < \frac{1}{p(n)}.
> $$

Examples:

- $2^{-n}$, $2^{-\sqrt{n}}$ are negligible.
- $1/n$, $1/\log n$ are **not** negligible.

We use negligible functions to formalize “success probability is so small that it’s essentially zero” for large security parameter $n$.

---

## Computational security

We consider an efficient adversary $\mathcal{A}$ (probabilistic polynomial-time algorithm) and a scheme with security parameter $n$.

A scheme is **computationally secure** if:

- Any PPT adversary’s advantage in breaking the scheme is **negligible** in $n$.

Typical pattern:

1. Define a game between a challenger and adversary.
2. Adversary’s advantage is some probability of success minus what a random guess would achieve.
3. The scheme is secure if any PPT adversary’s advantage is negligible.

Example: IND-CPA security of encryption, PRF security, MAC unforgeability, etc.

---

## Pseudorandom functions (PRFs)

A **pseudorandom function** family is a set of functions $\{ F_k \}_{k \in \mathcal{K}}$ where:

- Each $F_k : \{0,1\}^n \to \{0,1\}^m$ is efficiently computable given key $k$.
- $k$ is chosen uniformly at random from the key space.

Intuition:

- To any efficient adversary with oracle access, $F_k(\cdot)$ looks like a truly random function.

### PRF security game

The adversary interacts with an oracle:

- With a hidden bit $b \in \{0,1\}$.
- If $b = 0$: the oracle is $F_k(\cdot)$ for random secret key $k$.
- If $b = 1$: the oracle is a truly random function $R(\cdot)$.

Adversary outputs guess $b'$. Advantage:

$$
\text{Adv}_{\text{PRF}}(\mathcal{A}) = \left| P[b' = b] - \frac{1}{2} \right|.
$$

We call $F$ a secure PRF if this advantage is negligible for all PPT adversaries.

---

## PRP vs PRF

A **pseudorandom permutation (PRP)** is like a PRF but with extra structure:

- Domain = range (e.g. $\{0,1\}^n$).
- For each key $k$, $F_k$ is a **permutation**: bijective, with an efficient inverse.
- Example: block ciphers like AES are modeled as PRPs on 128-bit blocks.

Relationship:

- Every PRP is also a PRF (roughly, because a random permutation looks like a random function to an adversary that makes fewer queries than the domain size).
- Not every PRF is a PRP (PRF doesn’t need to be bijective).

PRPs are ideal for **block ciphers**; PRFs are used more generally (MACs, key derivation, etc.).

---

## Security reductions

A **reduction** is a proof technique:

> To show scheme $A$ is secure assuming primitive $B$ is secure, we construct an algorithm $R$ that breaks $B$ using any adversary that breaks $A$.

Steps:

1. Assume there exists an adversary $\mathcal{A}$ that breaks scheme $A$ with non-negligible advantage.
2. Construct a **reduction** $\mathcal{R}$ that uses $\mathcal{A}$ as a subroutine to break the underlying primitive $B$.
3. If $B$ is assumed secure, this leads to a contradiction unless $\mathcal{A}$’s advantage is negligible.

Intuition:

- “If you can break our construction, then you can also break the underlying assumption/primitive.”
- Security proofs rarely show absolute security; instead they show *relative* security: “as secure as AES/PRF/DLP/etc.”
