
We denote vectors as $x \in \{0,1\}^\lambda$. By $x[i]$ we denote the $i$-th index of $x$, where $i \in \{1,\dots,\lambda\}$.  
As in the lecture, we write $k \leftarrow K$ if $k$ is sampled uniformly at random from $K$ with probability $1/|K|$.

---

## Exercise 1 — Triple the DES!

In the lecture you learned about the DES pseudorandom permutation (PRP) or block cipher, which has block size $B = 64$ and key length $\lambda = 56$. We denote its algorithms as  
$\text{DES.Keygen}$, $\text{DES.Enc}$, $\text{DES.Dec}$.

A well-known variation of DES is **Triple-DES (3DES)**, defined as follows:

### 3DES.Keygen
1. Compute $k_1, k_2, k_3 \leftarrow \text{DES.Keygen()}$.
2. Output $k = (k_1, k_2, k_3)$.

### 3DES.Enc$(k, m)$
1. Check that $k = (k_1, k_2, k_3)$.
2. Output
   $$
   \text{DES.Enc}(k_3,\ \text{DES.Dec}(k_2,\ \text{DES.Enc}(k_1, m))).
   $$

3. Find the missing **3DES.Dec** algorithm and argue why the overall scheme is correct.
4. In a brute-force attack, an attacker tries all possible keys.
   Assume we are given a plaintext/ciphertext pair
   $$
   c = \text{DES.Enc}(k, m)
   $$
   for unknown $k$.
   Assume one $\text{DES.Enc}$ or $\text{DES.Dec}$ operation takes **1 ns**.
   
   **(a)** How many *days* does it take to recover $k$?  
   **(b)** If you have **1024 machines**, how long does the attack take now?  
   **(c)** Compute the same attack runtime for **3DES**. Compare it to the age of the universe
   (about $13.8 \times 10^9$ years).

---

## Exercise 2 — There Are More PRFs

In class, you learned that a pseudorandom function (PRF) is a function

$$
F : \{0,1\}^\lambda \times \{0,1\}^{\text{in}} \to \{0,1\}^{\text{out}}
$$

such that the libraries $L_{\text{PRF-Real}}^F$ and $L_{\text{PRF-Rand}}^F$ are indistinguishable.

Assume such a PRF $F$ is given. Let $r \in \{0,1\}^{\text{out}}$ be any fixed, publicly known string, and define

$$
G(k, x) = F(k, x) \oplus r.
$$

Show that $G$ is also a PRF. To prove this, follow these steps:

1. Write down the Lookup functions in the libraries  
   $L_{\text{PRF-Real}}^G$ and $L_{\text{PRF-Rand}}^G$.
2. Rewrite $L_{\text{PRF-Real}}^G$ to use $L_{\text{PRF-Rand}}^G$.
3. Apply the hybrid argument to replace  
   $L_{\text{PRF-Real}}^F$ with $L_{\text{PRF-Rand}}^F$.
4. Argue why the resulting library is indistinguishable from  
   $L_{\text{PRF-Rand}}^G$.

---

## Exercise 3 — Birthday Bounds in Practice

In class, you learned about the function $\text{BirthdayProb}(q, N)$, the probability of sampling two or more identical items after $q$ random queries from a set of size $N$.

1. Write a program that computes **exact**
   $$
   \text{BirthdayProb}(q, N)
   $$
   and also its **upper bound** from the lecture.  
   How close is the bound to the exact value?
2. Compute the probability that **two students** in your course share the same birthday
   (assume uniform distribution over the year).
3. In class, we used birthday bounds to argue that PRFs and PRPs are sometimes indistinguishable.
   Assume you have:
   $$
   F : \{0,1\}^{32} \times \{0,1\}^{32} \to \{0,1\}^{32} \quad (\text{PRF})
   $$
   $$
   G : \{0,1\}^{32} \times \{0,1\}^{32} \to \{0,1\}^{32} \quad (\text{PRP})
   $$

   What is the probability of distinguishing $F$ from $G$ for the following values of $q$?

   - $q = 2$
   - $q = 2^8$
   - $q = 2^{12}$
   - $q = 2^{16}$
