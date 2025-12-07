
These practice problems have the purpose of helping you understand the material better and
learning the skills that are necessary to analyze cryptographic constructions, and sometimes to
prepare you for the next class. All answers should be supported by a written justification. To
gauge whether a justification is sufficient, ask yourself if your peers would be convinced by it
without additional explanations.

We denote bitstrings as $x \in \{0,1\}^{\lambda}$. By $x[i]$ we denote the $i$th bit of $x$, where $i \in \{1,\dots,\lambda\}$.

As in the lecture, we write  
$k \leftarrow K$  
if $k$ is sampled from the set $K$ such that it can be each element from $K$ with equal probability
$1/|K|$.

For two strings $x, y$ we use $x \mid y$ to denote the string obtained from concatenating $x$ with $y$.

---

## Exercise 1 – Length extension attacks for Merkle–Damgård

Let $h : \{0,1\}^{\ell+\lambda} \to \{0,1\}^\lambda$ be a collision-resistant compression function.

MDPad$_\ell(m)$:
1. Let $e$ be $|m|$ in binary, encoded in $\ell$ bits.
2. Pad $m$ with zeros until its length is a multiple of $\ell$.
3. Output $m \mid e$.

MDHash$(m)$:
1. Let $m_1,\dots,m_\tau = \text{MDPad}_\ell(m)$.
2. Set $y_0 := 0^\lambda$.
3. For $i=1,\dots,\tau$ set $y_i := h(m_i \mid y_{i-1})$.
4. Output $y_\tau$.

MDMAC$(k,m)$ is the same as MDHash except $y_0 := k$.

---

# Problem Sheet 6 – Full Step-by-Step Solutions

This file contains full derivations, logical steps, and explanations for each exercise, not just the final results.

---

# Exercise 1 – Length Extension Attacks for Merkle–Damgård

We are given:

- A compression function  
  $$h : \{0,1\}^{\ell+\lambda} \to \{0,1\}^{\lambda}.$$

- A padding rule MDPad$_\ell(m)$ that pads $m$ to blocks of length $\ell$ and appends a length block.

- MDHash computes:
  1. Pad: $m_1,\dots,m_\tau = \text{MDPad}_\ell(m)$.
  2. Initialize $y_0 = 0^\lambda$.
  3. Iterate: $y_i = h(m_i \mid y_{i-1})$.
  4. Output $y_\tau$.

- MDMAC sets $y_0 = k$ instead of $0^\lambda$.

Goal: **Given $(m,t)$ with $t=\text{MDMAC}(k,m)$, find $(m',t')$ with $m'\ne m$ such that $t'=\text{MDMAC}(k,m')$ (a MAC forgery).**

---

## Step-by-step reasoning

### Step 1 — Expand the MDMAC computation

Let  
$$m_1,\dots,m_\tau = \text{MDPad}_\ell(m).$$  
Define the internal chain:

- $y_0 = k$  
- $y_1 = h(m_1 \mid y_0)$  
- …  
- $y_\tau = h(m_\tau \mid y_{\tau-1}) = t$

The output $t$ is *also the internal state* after hashing $m$.

### Step 2 — Observe attacker’s capabilities

The attacker **does not know $k$**, but:

- knows the compression function $h$,
- knows $m$ and can compute its padded blocks,
- knows $t = y_\tau$.

The chaining value $t$ is exactly what the compression function expects as previous state when hashing more blocks. Thus anyone can **continue the hash/MAC**, even without the key.

### Step 3 — Construct a longer message

Take any non-empty extension string $z$.

In a true length-extension attack you must respect how MDPad encodes the length. Conceptually, there exists a (deterministically computable) padded extension $m'$ of the form

$$
m' = m \parallel \text{pad}_{\text{MD}}(m,z)
$$

such that, when MDMAC is run on $m'$, the internal state right after the original part $m$ is still $y_\tau = t$. (In practice one reconstructs the MD padding of $m$ and then appends $z$.)

### Step 4 — Compute the new tag

Let $z_1,\dots,z_\nu$ be the padded blocks resulting from the extension.

Then:

1. Start at state $t$.
2. Compute $t_1 := h(z_1 \mid t)$.
3. Compute $t_2 := h(z_2 \mid t_1)$.
4. Continue until the final state $t' := h(z_\nu \mid t_{\nu-1})$.

By construction this equals $\text{MDMAC}(k,m')$ because MDMAC performs *exactly the same computation* starting from the same chaining value $t$.

---

### Final result for Exercise 1

Conceptually, a valid forgery is:

- $m'$ = “$m$ extended with MD padding and extra blocks $z$”,
- $t'$ = $\text{Extend}(t,z)$ computed by running $h$ on the extension blocks starting from $t$.

Here we abstract away the low-level details of how the key length interacts with the MD padding; in practice, including the key before MD padding is what enables this type of length-extension attack on constructions like MDMAC.

Thus a length-extension attack always works on MDMAC.

---

# Exercise 2 – SMDHash Is Not Collision-Resistant

We are given SMDPad$_\ell$ and SMDHash defined as:

- First block is the message prefix of length $\lambda$.
- Later blocks are length $\ell$ and hashed with the compression function.

Goal: **Show a collision exists.**

---

## Step-by-step reasoning

### Step 1 — Understand SMDPad

If message length > $\ell+\lambda$:

- Split: $m = m_1 \mid m_2$, where $|m_1| = \lambda$.
- Pad $m_2$ to a multiple of $\ell$.

Thus padded message is:
$$
m_1, m_2[1], m_2[2], \dots
$$

### Step 2 — Understand SMDHash

- Set $y_1 = m_1$.
- For $i\ge2$:  
  $$y_i = h(m_i \mid y_{i-1}).$$
- Output $y_\tau$.

Unlike MDHash, **the first block is not hashed**; it becomes the internal state directly.

### Step 3 — Set up a collision strategy

We want to take an existing message $m$ and create a *different* message $m'$ that yields the same output.

Suppose padded $m$ gives blocks:
$$
m_1, m_2, m_3, \dots, m_\tau.
$$

Then:
- $y_1 = m_1$
- $y_2 = h(m_2 \mid y_1)$
- $y_3 = h(m_3 \mid y_2)$
- …
- $y_\tau$ is output.

### Step 4 — Create new message $m'$ that “starts at” $y_2$

Let

- New first block $m_1' = y_2$.
- Remaining blocks are shifted:
  $$
  m_2' = m_3, \quad m_3' = m_4, \dots, m_{\tau-1}' = m_\tau.
  $$

Now compute SMDHash on $m'$:

- $y_1' = m_1' = y_2$.
- $y_2' = h(m_2' \mid y_1') = h(m_3 \mid y_2) = y_3$.
- …
- $y_{\tau-1}' = h(m_\tau \mid y_{\tau-2}') = y_\tau$.

Thus:

$$
\text{SMDHash}(m') = y_\tau = \text{SMDHash}(m).
$$

Since $m_1' = y_2 \ne m_1$, the messages differ.

---

### Final result for Exercise 2

A collision is:

- original: $m$
- modified: $m' = y_2 \mid m_3 \mid \dots \mid m_\tau$

Both hash to the same value.

---

# Exercise 3 – Homomorphic Hashes Cannot Be Collision-Resistant

We assume:

$$
H(x) \oplus H(y) = H(x \oplus y)
$$

for all same-length bitstrings.

Goal: **Show we can efficiently find collisions.**

---

## Step-by-step reasoning

### Step 1 — Fix a length and treat $H$ as a linear map

For strings of length $n$, define:

$$
H_n : \{0,1\}^n \to \{0,1\}^\lambda.
$$

Homomorphism implies $H_n$ is **linear** over $\mathbb{F}_2$.

### Step 2 — A linear map from dimension $n > \lambda$ has a non-trivial kernel

Since $n > \lambda$, the kernel of $H_n$ contains some nonzero vector $v$ with:

$$
H_n(v) = 0^\lambda.
$$

This immediately gives a collision:

- $v$
- $0^n$

### Step 3 — Compute $v$ efficiently

Construct matrix $M$ whose columns are $H_n(e_i)$ where $e_i$ are unit vectors:

1. Query $H(e_i)$ for all $n$ basis vectors.
2. Form matrix $M$ of size $\lambda \times n$.
3. Solve $Mv = 0$ using Gaussian elimination.
4. Obtain nonzero $v$.

Thus:

$$
H(v) = 0^\lambda = H(0^n).
$$

This yields a collision.

### Step 4 — Distinguish real vs fake CR library

- In the real library, this method **always** outputs a collision.
- In a collision-free fake library, the probability of such a collision is negligible.

Thus $H$ is not collision-resistant.

---

# Exercise 4 – PRP-Based Hash Is Not Collision-Resistant

We define:

$$
H(x \mid y) = F(x,y)
$$

where $F$ is a PRP on $B$ bits under key $x$.

Goal: **Find collisions efficiently.**

---

## Step-by-step reasoning

### Step 1 — Choose two different keys

Pick $x \ne x'$.

### Step 2 — Choose any $y$ and compute its image

Let $z = F(x,y)$.

### Step 3 — Invert $z$ under the other key

Compute:

$$
y' = F^{-1}(x', z).
$$

This is possible because PRPs are efficiently invertible.

### Step 4 — Verify collision

Compute:

- $H(x \mid y) = F(x,y) = z$  
- $H(x' \mid y') = F(x',F^{-1}(x',z)) = z$

Thus:

- Inputs differ: $(x \mid y) \ne (x' \mid y')$
- Outputs equal.

### Step 5 — Distinguish real vs fake CR library

- Real library: always finds collision.
- Fake library: negligible probability.

Thus $H$ is not collision-resistant.

---

# End of Problem Sheet 6 – Fully Worked Solutions
