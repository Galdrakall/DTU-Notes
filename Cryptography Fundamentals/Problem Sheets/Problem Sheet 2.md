
We denote vectors as $x \in \{0,1\}^\lambda$. By $x[i]$ we denote the $i$-th index of $x$, where $i \in \{1,\dots,\lambda\}$.  
As in the lecture, we write $k \leftarrow K$ if $k$ is sampled from the set $K$ uniformly at random with probability $1/|K|$.

---

## AND Function

For the first exercise, we denote by $\&$ the AND-function, which is defined by the following truth table:

| $x$ | $y$ | $x \& y$ |
|-----|-----|----------|
| 0   | 0   | 0        |
| 0   | 1   | 0        |
| 1   | 0   | 0        |
| 1   | 1   | 1        |

For two vectors $x, y$, we define the coordinate-wise AND of both vectors, $x \& y$, by

$$
(x \& y)[i] = x[i] \& y[i].
$$

This means that $x \& y$ is computed by applying $\&$ to the individual coordinates of $x, y$ and concatenating the outcomes.

---

## Exercise 1 — Not All Bit Operations Are Equal

Let $\lambda$ be a positive integer. We define

$$
K = M = C = \{0,1\}^\lambda
$$

and consider the following algorithms:

### Keygen
1. Output $k \leftarrow K$.

### Enc$(k, m \in M)$
1. Output $k \& m$.

### Dec$(k, c \in C)$
1. Output $k \& c$.

Show that these three algorithms **do not form a SKE scheme**, because they are **not correct**.  
You can show this by giving an example where decryption fails.

### Solution 1)
We have SKE is correct when $\forall k \in K$, $\forall m \in M$: Pr(Dec(k,Enc(k,m))=m) = 1
We set $\lambda = 2, k=01, m=10$. Then Dec(k, Enc(k,m)) == 00 but m = 10

---

## Permutations on Bitstrings

A permutation of a string is a rearrangement of all the entries of that string.  
For example, let $a,b \in \{0,1\}$ be bits and $ab$ be a bitstring. Then both $ab$ and $ba$ are permutations of the original string.

Note that $aa$ is **not** a permutation of $ab$, as $b$ also has to appear.

For a string of length $n$, there are $n!$ permutations (but they are not all distinct as functions).

We denote the set of all permutation functions on $n$-bit strings by $S_n$.

For a string $x \in \{0,1\}^n$, we write

$$
y := \pi(x)
$$

to say that $y$ is the string $x$ permuted under $\pi \in S_n$. Formally:

$$
y[\pi(i)] = x[i] \quad \text{for all } i \in \{1,\dots,n\}.
$$

Let $\pi^{-1}$ be the inverse permutation.

---

## Exercise 2 — Permutations for Security?

Let $\lambda$ be a positive integer and let $S_\lambda$ be the set of all permutations on strings of length $\lambda$.

We define

$$
M = C = \{0,1\}^\lambda
$$

and consider the following algorithms:

### Keygen
1. Output $\pi \leftarrow S_\lambda$.

### Enc$(\pi, m \in M)$
1. Output $\pi(m)$.

### Dec$(\pi, c \in C)$
1. Output $\pi^{-1}(c)$.
### Solution 2
1. Show that these three algorithms form a SKE scheme, i.e. that they are correct.
	- Permutation $\pi$ is applied to the indices of the message so the message is shuffled so, $$
	Dec(k,Enc(k,m)) = Dec(k, \pi (m)) = \pi^{-1}(\pi (m) =(\pi^{-1} \circ \pi)(m) = Id(m)=m $$
2. Let $\text{LOTS-}0$, $\text{LOTS-}1$ be the two worlds in the left-or-right one-time security game.  
   Show that the above algorithms are **not one-time indistinguishable**, i.e.

$$
\text{LOTS-}0 \not\equiv \text{LOTS-}1.
$$

Construct an explicit algorithm $B$ which sends one query to the library. Based on the response $c$, $B$ outputs $0$ if it thinks it interacted with $\text{LOTS-}0$ and $1$ otherwise.  
What is the probability that $B$ gives the correct answer?
### Solution 2
![[mynd1.png]]
$$Pr(b = b') = Pr(b=b' \cap LOTS-0) + r(b=b' \cap LOTS-1)$$
$$= Pr(0=b' |LOTS-0)Pr(LOTS-0) + Pr(1=b' |LOTS-1)Pr(LOTS-1)=1 $$
For any event A and B, $Pr(A\cap B)=Pr(A|B)Pr(B) = Pr(B|A)Pr(A)$

---

## XOR Function

For the last exercise, we denote by $\oplus$ the XOR-function, defined by:

| $x$ | $y$ | $x \oplus y$ |
|-----|-----|--------------|
| 0   | 0   | 0            |
| 0   | 1   | 1            |
| 1   | 0   | 1            |
| 1   | 1   | 0            |

As for the AND-function, it is defined coordinate-wise for vectors.

---

## Exercise 3 — Why Keygen Is Important

Let $\lambda$ be a positive integer. Define

$$
K = M = C = \{0,1\}^\lambda
$$

and let $B_p$ be the probability distribution that outputs $0$ with probability $p$ and $1$ with probability $1-p$.

Consider the following algorithms:

### Keygen
1. Let $k$ be a vector of length $\lambda$.
2. For each $i \in \{1,\dots,\lambda\}$ set
   $$
   k[i] \leftarrow B_{0.75}.
   $$
### Enc$(k, m \in M)$
1. Output $k \oplus m$.

### Dec$(k, c \in C)$
1. Output $k \oplus c$.
### Solution 3

1. Show that these three algorithms form a SKE scheme.
	- $Dec(k(Enc(k,m)) = Dec(k, k\oplus m)= k\oplus (k \oplus m) = m$
2. Let $\lambda = 3$ and let $A$ query the $\text{LOTS-Real}$ library with input $m = 111$.  
   Compute the probability of each possible ciphertext.
3. Let $B$ send $m = 111$ and receive $c$.  
   $B$ outputs $1$ if $c = m$ and $0$ otherwise.  
   What is the output probability in $\text{LOTS-Real}$ and $\text{LOTS-Rand}$?
4. Show that the scheme is **not real-or-random secure**, i.e.

$$
\text{LOTS-Real} \not\equiv \text{LOTS-Rand}.
$$

5. **Bonus:** Is $B$ optimal? If not, find a better attack.

![[mynd2.png]]