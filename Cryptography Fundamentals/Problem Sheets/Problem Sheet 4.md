
These practice problems have the purpose of helping you understand the material better and learning the skills that are necessary to analyze cryptographic constructions, and sometimes to prepare you for the next class. All answers should be supported by a written justification. To gauge whether a justification is sufficient, ask yourself if your peers would be convinced by it without additional explanations.

We denote vectors as $x \in \{0,1\}^\lambda$. By $x[i]$ we denote the $i$-th index of $x$, where $i \in \{1,\dots,\lambda\}$.  
As in the lecture, we write $k \leftarrow K$ if $k$ is sampled from the set $K$ uniformly at random with probability $1/|K|$.
---

## Exercise 1 – Never use ECB

### 1. The basic problem

ECB mode encrypts each block independently:

$$
	\text{ECB}_k(m) = E_k(m[1]) \,\|\, E_k(m[2]) \,\| \cdots
$$

Consider two 2-block messages (block size $B$):

- $m_0 = 0^B \,\|\, 1^B$
- $m_1 = 0^B \,\|\, 0^B$

Then

- $\text{ECB}_k(m_0) = E_k(0^B) \,\|\, E_k(1^B)$
- $\text{ECB}_k(m_1) = E_k(0^B) \,\|\, E_k(0^B)$

Since $E_k$ is a permutation, $E_k(0^B) \neq E_k(1^B)$. Hence:

- For $m_1$, the two ciphertext blocks are **equal**.
- For $m_0$, the two ciphertext blocks are **different**.

So the ciphertext reveals whether the two plaintext blocks are equal. ECB leaks structure and cannot be IND‑CPA secure.

---

### 2. CPA adversary exploiting ECB leakage

An adversary $\mathcal{A}$ in the LR IND‑CPA experiment:

```text
A^LR(1^λ):

1. Define:
   m0 := 0^B || 1^B
   m1 := 0^B || 0^B

2. C := LR(m0, m1)

3. Parse C as C1 || C2

4. If C1 == C2:
	   output 1   // guess b = 1
   else:
	   output 0   // guess b = 0
```

Correctness:

- If $b = 0$: $C = \text{ECB}_k(m_0) = E_k(0^B)\,\|\,E_k(1^B)$, so $C_1 \neq C_2$ and $\mathcal{A}$ outputs $0$ (correct).
- If $b = 1$: $C = \text{ECB}_k(m_1) = E_k(0^B)\,\|\,E_k(0^B)$, so $C_1 = C_2$ and $\mathcal{A}$ outputs $1$ (correct).

Thus

$$
\Pr[\mathcal{A} \text{ guesses correctly}] = 1
$$

and the advantage is

$$
	ext{Adv}^{LR}_\mathcal{A} = \left|1 - \tfrac12\right| = \tfrac12.
$$

So ECB is **not CPA‑secure**.

  

---

  

## Exercise 2 – Padding for modes of operation

  

ANSI X.923 padding:

Append zero bytes and then a final byte containing the number of bytes added.

  

### 1. Why add a whole block when $m$ is already a multiple of $B$ and ends with 01?

If $m$ ends with the byte `01` and we **do not pad** when its length is already a multiple of $B$, then during unpadding we:

- Read the last byte $b = 1$.
- Remove the last $b$ bytes (1 byte) as “padding”.

But this byte is real data, so the message is corrupted.

To avoid that ambiguity, ANSI X.923 always appends a **full block** when the length is already a multiple of $B$:

$$
00 || 00 || \dots || 00 || (B/8)
$$

with $B/8-1$ zero bytes followed by a byte of value $B/8$. This guarantees the last byte is always padding, not data.

---

### 2. Maximum block size supported

The padding length $b$ is stored in **one byte**, so:

$$

b \le 255.

$$

Worst case requires $b = B/8$, so:

$$

B/8 \le 255 \quad\Rightarrow\quad B \le 2040 \text{ bits}.

$$

**Maximum block size: 2040 bits.**

---

### 3. CTR mode for messages with length not multiple of $B$

CTR mode encrypts by XORing plaintext with keystream blocks and **does not require padding**, even for partial blocks.

Algorithm (block size $B$):

```text
Input: key k, message m of L bits, initial counter ctr0

1. Split m into blocks:
	M1, ..., M_{ℓ-1} of B bits, and M_ℓ of t ≤ B bits.

2. For i = 1,...,ℓ-1:
	S_i := E_k(ctr0 + (i-1))
	C_i := M_i XOR S_i

3. For the last (possibly partial) block:
	S_ℓ := E_k(ctr0 + (ℓ-1))
	C_ℓ := M_ℓ XOR (first t bits of S_ℓ)

4. Output C1 || ... || C_ℓ (and ctr0).
```
---

## Exercise 3 – Adversarial Sampling

Two libraries:

- **LGenSample**: sample uniformly from $\{0,1\}^\lambda$

- **LGenSampleIgnore**: sample uniformly from $\{0,1\}^\lambda \setminus R$

Difference occurs only if the output lands in $R$.

---

### 1. Distinguishing strategy

Adversary $A$:

```text

A^Sample(1^λ):

1. Choose any non-empty R.

2. r := Sample(R)

3. If r ∈ R then output 1 else output 0.

```

Under LGenSampleIgnore: always outputs 0.

Under LGenSample: outputs 1 iff $r \in R$.

---

### 2. Advantage for one call, $|R| = n$

Under LGenSample:

$$

\Pr[r \in R] = \frac{n}{2^\lambda}.

$$

Under LGenSampleIgnore:

$$

\Pr[r \in R] = 0.

$$

Thus max advantage:

$$

\text{Adv}^{(1)}_A = \frac{n}{2^\lambda}.

$$

---

### 3. Two calls: sets $R_1$, $R_2$

Difference occurs only when:

$$

E = \{ r_1 \in R_1 \} \cup \{ r_2 \in R_2 \}.

$$

Using union bound:

$$

\Pr[E] \le \frac{|R_1|}{2^\lambda} + \frac{|R_2|}{2^\lambda}.

$$

Thus:

$$

\text{Adv}^{(2)}_A \le \frac{|R_1| + |R_2|}{2^\lambda}.

$$

---

### 4. $q$ calls

Bad event:

$$

E = \bigcup_{i=1}^q \{r_i \in R_i\}.

$$

Union bound:

$$

\Pr[E] \le \sum_{i=1}^q \frac{|R_i|}{2^\lambda}.

$$

Thus:

$$

\text{Adv}^{(q)}_A \le \frac{1}{2^\lambda} \sum_{i=1}^q |R_i|.

$$

---

### 5. Polynomial-time adversary → negligible advantage

Since the adversary must **explicitly write all elements of each $R_i$**, and has only **polynomial time**:

- Number of queries $q \le p_1(\lambda)$

- Each $|R_i| \le p_2(\lambda)$

Thus:

$$

\sum_{i=1}^q |R_i| \le p_1(\lambda) p_2(\lambda) = p(\lambda).

$$

Then:

$$

\text{Adv}^{(q)}_A \le \frac{p(\lambda)}{2^\lambda},

$$

which is negligible.

Therefore every polynomial-time adversary has **negligible advantage**.

---