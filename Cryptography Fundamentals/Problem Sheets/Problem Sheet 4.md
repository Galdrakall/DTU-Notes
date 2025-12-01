
These practice problems have the purpose of helping you understand the material better and learning the skills that are necessary to analyze cryptographic constructions, and sometimes to prepare you for the next class. All answers should be supported by a written justification. To gauge whether a justification is sufficient, ask yourself if your peers would be convinced by it without additional explanations.

We denote vectors as $x \in \{0,1\}^\lambda$. By $x[i]$ we denote the $i$-th index of $x$, where $i \in \{1,\dots,\lambda\}$.  
As in the lecture, we write $k \leftarrow K$ if $k$ is sampled from the set $K$ uniformly at random with probability $1/|K|$.

---

## Exercise 1 — Never Use ECB

In the lecture, it was mentioned that the ECB mode of operation should never be used for anything, under any circumstances. In this exercise we will see that it is not even CPA secure.

1. Assume ECB is used with a block cipher of block length $B$. Consider the encryptions of the messages

   $$
   0^B \,\|\, 1^B
   \quad \text{and} \quad
   0^B \,\|\, 0^B.
   $$

   Can you see a problem?

2. Turn the discovered problem into an attacker against left-right CPA security.  
   Your attacker should make **one** query to the library and always succeed.

---

## Exercise 2 — Padding for Modes of Operation

In the lecture, we saw that modes of operation allow encryption of messages of length $\ell \cdot B$ bits, where $B$ is the block length and $\ell \in \mathbb{N}$. Unfortunately, not every message is an exact multiple of $B$. To support messages of arbitrary length, a **padding scheme** is used.

The **ANSI X.923 padding** works as follows:

1. Let $m$ be a message of $n$ bytes, such that
   $$
   \ell = \left\lfloor \frac{8n}{B} \right\rfloor .
   $$

2. Output the padded message
   $$
   m' = m \,\|\, 00 \,\|\, 00 \,\|\, \cdots \,\|\, 00 \,\|\, b,
   $$

   where the total length is $(\ell+1)\cdot(B/8)$ bytes, and $b$ is the number of bytes that were added to $m$.  
   The value $b$ itself is encoded in one byte (8 bits).

### Examples

- If $m$ is **4 bytes short** of a multiple of $B$, append:
### Questions

1. Assume that $m$ already has a length that is a multiple of $B$ and that it ends with `01`.  
 Why must we still add a **full block** of length $B$ before encrypting, instead of encrypting the original message?

2. What is the **maximal block size** (in bits) that can be supported by the ANSI X.923 padding scheme?

3. Consider the **CTR mode** of operation. It can be modified to encrypt messages whose lengths are not multiples of $B$.  
 How can this be done?

---

## Exercise 3 — Adversarial Sampling

In class, it was claimed that any polynomial-time (in $\lambda$) attacker only has **negligible** distinguishing advantage for the following libraries:

### Library $L_{\text{GenSample}}$
Sample(R ⊆ {0,1}^λ):

1. r ← {0,1}^λ
2. Output r

### Library $L_{\text{GenSampleIgnore}}$
Sample(R ⊆ {0,1}^λ):

1. r ← {0,1}^λ \ R
2. Output r

We will now show that this is true **if the attacker has to explicitly write down every element of $R$** before passing it to `Sample`.

1. Devise a strategy to distinguish both libraries. Write down an adversary as an algorithm $A$.  
   You may use one or more queries to `Sample`.

2. Assume $A$ makes **one** query and $|R| = n$.
   - For which outputs of $L_{\text{GenSample}}$ does $A$ behave identically to $L_{\text{GenSampleIgnore}}$?
   - For which outputs does $A$ behave differently?
   - How does this translate into the distinguishing advantage?

3. Assume that $A$ makes **two** calls to `Sample`.  
   What is the **maximal distinguishing advantage** now?

4. Generalize your argument to $q \in \mathbb{N}$ calls to `Sample`.  
   Use the **Birthday Bound** technique to derive an upper bound.

5. Assume $A$ makes $q \in \mathbb{N}$ calls and that generating each item of $R$ costs 1 unit of time.
   Since $A$ runs in polynomial time in $\lambda$, conclude:
   - $q$ is polynomial in $\lambda$
   - $|R|$ is polynomial in $\lambda$

   Finally, conclude why the distinguishing advantage must be **negligible**.

We will now show that this is true **if the attacker has to explicitly write down every element of $R$** before passing it to `Sample`.

1. Devise a strategy to distinguish both libraries. Write down an adversary as an algorithm $A$.  
   You may use one or more queries to `Sample`.

2. Assume $A$ makes **one** query and $|R| = n$.
   - For which outputs of $L_{\text{GenSample}}$ does $A$ behave identically to $L_{\text{GenSampleIgnore}}$?
   - For which outputs does $A$ behave differently?
   - How does this translate into the distinguishing advantage?

3. Assume that $A$ makes **two** calls to `Sample`.  
   What is the **maximal distinguishing advantage** now?

4. Generalize your argument to $q \in \mathbb{N}$ calls to `Sample`.  
   Use the **Birthday Bound** technique to derive an upper bound.

5. Assume $A$ makes $q \in \mathbb{N}$ calls and that generating each item of $R$ costs 1 unit of time.
   Since $A$ runs in polynomial time in $\lambda$, conclude:
   - $q$ is polynomial in $\lambda$
   - $|R|$ is polynomial in $\lambda$

   Finally, conclude why the distinguishing advantage must be **negligible**.
