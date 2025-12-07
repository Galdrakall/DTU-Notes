
These practice problems have the purpose of helping you understand the material better and learning the
skills that are necessary to analyze cryptographic constructions, and sometimes to prepare you for the next
class. All answers should be supported by a written justification. To gauge whether a justification is
sufficient, ask yourself if your peers would be convinced by it without additional explanations.

---

## Exercise 1. (Computing Discrete Logarithms with smooth group order)

Let $p$ be a prime and $g, h \in \mathbb{Z}_p^*$ where $g$ has order $p - 1$. This means that

$$
g^{p-1} = 1 \bmod p
$$

while

$$
g^x \ne 1 \bmod p \quad \text{for every } 1 \le x < p - 1.
$$

Given $p, g, h$ the discrete logarithm problem is to find the unique  
$a \in \mathbb{Z}_{p-1}$ such that

$$
g^a = h \bmod p.
$$

For this exercise, let

$$
p = 43, \quad g = 12, \quad h = 5.
$$

1. Try out all possible choices of $a$ (e.g. using a Python script or Excel) to find the discrete logarithm.
   For an arbitrary $p$, how many multiplications modulo $p$ would you have to do (in the worst case)
   to find $a$ this way?

2. We observe that $p - 1 = 2 \cdot 3 \cdot 7$ and want to use this to simplify the computation of the
   discrete logarithm. Let

   $$
   x = \frac{p - 1}{2}, \quad y = \frac{p - 1}{3}, \quad z = \frac{p - 1}{7}
   $$

   and consider the elements $g^x, g^y, g^z$.  
   What do you know about the order of these elements modulo $p$?

3. We can find the value $a \bmod 2$ by computing the discrete logarithm of $h^x$ for the base $g^x$.
   Similarly, we can obtain $a \bmod 3$ from $g^y, h^y$ and $a \bmod 7$ from $g^z, h^z$.
   Can you use this to find $a \in \mathbb{Z}_{42}$ more efficiently?

4. More generally, assume that $p - 1$ has $\ell$ prime factors that are all smaller than $B$.
   Can you (roughly) say how many multiplications modulo $p$ you have to do, in comparison to the
   trivial method that tries out all choices of $a$, to recover the discrete logarithm?

5. Given the results of this exercise, what can you say about the security of the discrete logarithm
   in groups of smooth order (i.e. where the order of the group only has small prime factors)?

---

## Exercise 2. (When the Decisional Diffie-Hellman Problem is easy)

Let $p$ be a prime. In the lecture, we considered the Decision Diffie-Hellman (DDH) problem in the
case when $g \in \mathbb{Z}_p^*$ was of large prime order $q$ such that $q \mid p - 1$.
Now instead, assume that $g \in \mathbb{Z}_p^*$ is a generator of the whole group $\mathbb{Z}_p^*$.

Show that, in this case, the DDH problem is easy: one can distinguish tuples of the form

$$
(g, g^a, g^b, g^{ab})
$$

for $a, b \in \mathbb{Z}_{p-1}$ from tuples of the form

$$
(g, g^a, g^b, g^c)
$$

for $a, b, c \in \mathbb{Z}_{p-1}$ with a very good chance.  
For this, use the observations from the previous exercise and consider what happens if you raise each
element in the tuple to $(p - 1)/2$.

---

## Exercise 3. (From Diffie-Hellman to Public-Key Encryption)

Let $p$ be a prime and $g \in \mathbb{Z}_p^*$ be of large prime order $q \mid p - 1$.
In the Diffie-Hellman Key Exchange Protocol, Alice and Bob exchange messages

$$
A = g^a \bmod p, \quad B = g^b \bmod p
$$

where $a, b \in \mathbb{Z}_q$.

1. Assume that Bob publishes the message $B$ as a public key, while he keeps $b$ as his secret key.
   Alice now encrypts a message $m \in \{0,1\}$ as follows:

   (a) She chooses $a \in \mathbb{Z}_q$, $r \in \mathbb{Z}_q^*$, and computes  
   $$
   c_1 = g^a \bmod p.
   $$

   (b) If $m = 0$ then she sets  
   $$
   c_2 = B^a \bmod p,
   $$
   otherwise she sets  
   $$
   c_2 = B^a \cdot g^r \bmod p.
   $$

   (c) She lets $(c_1, c_2)$ be the ciphertext for Bob.

   Show how Bob can recover the message.

2. Show that this encryption scheme is **IND-CPA secure** assuming DDH is hard in the group
   $\mathbb{Z}_p^*$ with generator $g$. Namely, show that if there exists an attacker that wins the
   IND-CPA security game with probability $P > 1/2$, then we can use it to construct an algorithm
   that breaks DDH with the same probability.

---

## Exercise 4. (Bad groups for Diffie–Hellman)

Explain in detail why the following groups are not suitable for use in the Diffie-Hellman key exchange
protocol. Below, let $p$ be a prime and let $(S_n, \circ)$ be the group of permutations of $n$ objects
($\circ$ denotes composition of permutations).

1. $(\mathbb{Z}_p, +)$, i.e. the group of remainders modulo $p$ with addition as group operation.

2. The group
   $$
   \left(
   \left\{
   \begin{pmatrix}
   1 & a \\
   0 & 1
   \end{pmatrix}
   \; \middle|\; a \in \mathbb{Z}_p
   \right\}, \cdot
   \right),
   $$
   where $\cdot$ denotes matrix multiplication modulo $p$.

3. $(\langle g \rangle, \circ)$ for a full cycle $g \in S_n$.

   A full cycle is a permutation $g \in S_n$ where
   $$
   \{1, g(1), g^2(1), \dots\} = \{1, 2, 3, \dots, n\}
   $$
   (as sets).

---

# Problem Sheet 10 – Full Step-by-Step Solutions

This section contains complete solutions and explanations for each exercise.

---

# Exercise 1 – Discrete logs with smooth group order

We work in $\mathbb{Z}_p^*$ with $p=43$, generator $g=12$ of order $p-1=42$, and target $h=5$. We must find $a \in \mathbb{Z}_{42}$ such that
$$
g^a \equiv h \pmod{p}.
$$

## 1. Brute-force discrete log and cost

For $p=43$ we can simply test all exponents $a=0,1,\dots,41$ and compute $12^a \bmod 43$ until we obtain 5. Doing this (by hand or script) yields
$$
12^{11} \equiv 5 \pmod{43},
$$
so $a=11$ is the discrete logarithm.

In general, a naive brute-force algorithm tries all $a \in \{0,\dots,p-2\}$ and for each computes $g^a$ incrementally as
$$
g^a = g^{a-1} \cdot g \bmod p.
$$
Thus it needs at most $p-1$ modular multiplications and comparisons, i.e. $O(p)$ multiplications in the worst case.

## 2. Orders of $g^x, g^y, g^z$

Here $p-1 = 42 = 2\cdot 3\cdot 7$ and we define
$$
x = \frac{p-1}{2} = 21, \quad y = \frac{p-1}{3} = 14, \quad z = \frac{p-1}{7} = 6.
$$

Since $g$ has order $42$, the order of $g^k$ is
$$
\operatorname{ord}(g^k) = \frac{42}{\gcd(42,k)}.
$$

Compute:

- For $g^x = g^{21}$: $\gcd(42,21)=21$, so $\operatorname{ord}(g^{21}) = 42/21 = 2$.
- For $g^y = g^{14}$: $\gcd(42,14)=14$, so $\operatorname{ord}(g^{14}) = 42/14 = 3$.
- For $g^z = g^{6}$: $\gcd(42,6)=6$, so $\operatorname{ord}(g^{6}) = 42/6 = 7$.

Thus $g^x, g^y, g^z$ generate subgroups of orders $2,3,7$ respectively.

## 3. Recovering $a \bmod 2,3,7$ and then $a$ modulo 42

We know $g^a \equiv h$. Raise both sides to the powers $x,y,z$:
$$
h^x \equiv (g^a)^x = (g^x)^a, \quad
h^y \equiv (g^y)^a, \quad
h^z \equiv (g^z)^a \pmod{p}.
$$

Since $g^x$ has order 2, any exponent is determined modulo 2. Thus from the equation
$$
(g^x)^a \equiv h^x \pmod{p}
$$
we can brute-force $a_2 := a \bmod 2$ by trying $0,1$.

Similarly, from
$$
(g^y)^a \equiv h^y \pmod{p}
$$
and the fact that $g^y$ has order 3, we can find $a_3 := a \bmod 3$ by trying $0,1,2$.

From
$$
(g^z)^a \equiv h^z \pmod{p}
$$
and the fact that $g^z$ has order 7, we can find $a_7 := a \bmod 7$ by trying $0,1,\dots,6$.

For our concrete numbers, one can compute (e.g. in a script) that
$$
a \equiv 1 \pmod 2, \quad a \equiv 2 \pmod 3, \quad a \equiv 4 \pmod 7.
$$
Solving this system using the Chinese Remainder Theorem (CRT) over modulus $42 = 2\cdot 3\cdot 7$ yields the unique solution
$$
a \equiv 11 \pmod{42},
$$
which matches the brute-force value.

This method needed only a few exponentiations and at most $2+3+7=12$ small discrete-log checks, versus up to 42 steps in the naive search.

## 4. General complexity when $p-1$ is $B$-smooth

Suppose $p-1$ factors as
$$
p-1 = \prod_{i=1}^{\ell} q_i^{e_i},
$$
where each prime $q_i < B$ (so $p-1$ is $B$-smooth). Pohlig–Hellman-style attacks compute $a \bmod q_i^{e_i}$ for each factor separately, using discrete logs in subgroups of order $q_i^{e_i}$.

Rough complexity (per factor) is about $O(q_i^{e_i})$ operations, so the total cost is roughly
$$
O\Big(\sum_{i=1}^{\ell} q_i^{e_i}\Big),
$$
which is much smaller than $O(p)$ when all $q_i$ are small compared to $p$. In contrast, the naive method costs $O(p)$ multiplications.

Thus, when $p-1$ is smooth, discrete logarithms can be computed in **substantially fewer** than $p$ multiplications.

## 5. Security in groups of smooth order

The above shows that if the group order $p-1$ factors into only small primes, one can efficiently solve the discrete logarithm problem using the Pohlig–Hellman approach.

Therefore, groups of **smooth order** (where $p-1$ has only small prime factors) are **insecure** for discrete-log-based cryptography: discrete logs can be computed much faster than generic algorithms (like baby-step/giant-step or Pollard’s rho) would suggest.

In practice, we choose groups where the order has at least one large prime factor, ensuring that the discrete-log problem remains hard.

---

# Exercise 2 – When DDH is easy

Now $g \in \mathbb{Z}_p^*$ is a generator of the *entire* group $\mathbb{Z}_p^*$, so it has order $p-1$. Consider tuples
$$
(g, g^a, g^b, g^{ab})
$$
vs.
$$
(g, g^a, g^b, g^c)
$$
for random $a,b,c \in \mathbb{Z}_{p-1}$.

We show DDH is easy by using the fact that $(p-1)/2$ exposes the parity of exponents.

Raise each component of the tuple to the power $(p-1)/2$:

- In $\mathbb{Z}_p^*$, by Euler’s criterion,
   $$
   g^{(p-1)/2} \equiv -1 \pmod{p},
   $$
   since $g$ is a generator and thus a quadratic non-residue.
- Also,
   $$
   (g^a)^{(p-1)/2} = g^{a(p-1)/2} \equiv (-1)^a,\\
   (g^b)^{(p-1)/2} = g^{b(p-1)/2} \equiv (-1)^b.
   $$

For the fourth component:

- If it is $g^{ab}$, then
   $$
   (g^{ab})^{(p-1)/2} = g^{ab(p-1)/2} \equiv (-1)^{ab}.
   $$
- If it is $g^c$, then
   $$
   (g^{c})^{(p-1)/2} = g^{c(p-1)/2} \equiv (-1)^{c}.
   $$

Now observe:

- From $(g^a)^{(p-1)/2}$ we learn the parity of $a$ (whether $a$ is even or odd).
- From $(g^b)^{(p-1)/2}$ we learn the parity of $b$.
- From the fourth entry’s $(p-1)/2$-power we learn the parity of $ab$ (in the Diffie–Hellman case) or of $c$ (in the random case).

In the Diffie–Hellman (DH) case, $ab$ mod 2 satisfies
$$
ab \equiv (a \bmod 2) \cdot (b \bmod 2) \pmod{2}.
$$
Thus the parity of $ab$ is the product of the parities of $a$ and $b$.

In the random (non-DH) case, $c$ is independent of $a,b$, so its parity is independent of the parities of $a$ and $b$.

Hence a distinguisher can:

1. Compute $\alpha = (g^a)^{(p-1)/2}$ and $\beta = (g^b)^{(p-1)/2}$ to get the parities of $a$ and $b$.
2. Compute $\gamma = (g^d)^{(p-1)/2}$ for the 4th component $g^d$.
3. Check whether $\gamma$ matches $(-1)^{ab}$, derived as $(-1)^{(a\bmod 2)(b\bmod 2)}$.

In a DH tuple, this always holds; in a random tuple, it holds only with probability about $1/2$. Thus DDH is easy in the full group $\mathbb{Z}_p^*$.

In practice we therefore restrict to prime-order subgroups where such leakage does not occur.

---

# Exercise 3 – From Diffie–Hellman to public-key encryption

We have a group $\langle g \rangle \subseteq \mathbb{Z}_p^*$ of large prime order $q \mid p-1$. Bob publishes $B = g^b$ as his public key (with secret key $b \in \mathbb{Z}_q$). Alice encrypts $m \in \{0,1\}$ as follows:

1. Choose $a \in \mathbb{Z}_q$, $r \in \mathbb{Z}_q^*$.
2. Set $c_1 = g^a$.
3. If $m=0$ set $c_2 = B^a$, else set $c_2 = B^a g^r$.
4. Ciphertext is $(c_1,c_2)$.

## 1. Decryption

Bob knows $b$ and receives $(c_1,c_2)$. He computes
$$
K := c_1^b = (g^a)^b = g^{ab},
$$
and also observes that
$$
B^a = (g^b)^a = g^{ab} = K.
$$

Thus the structure of $c_2$ is
$$
\begin{cases}
 c_2 = K & \text{if } m=0, \\
 c_2 = K g^r & \text{if } m=1.
\end{cases}
$$

Bob now checks whether $c_2$ equals $K$ or not:

- If $c_2 = K$, he sets $m' = 0$.
- Otherwise, he sets $m' = 1$.

Since $r \ne 0$ and the group has prime order, $g^r \ne 1$, so $K g^r \ne K$. Therefore this decryption recovers $m$ exactly.

## 2. IND-CPA security from DDH

We reduce IND-CPA security to the DDH assumption. Suppose there is an IND-CPA adversary $\mathcal{A}$ that wins the IND-CPA game with advantage $P - 1/2 > 0$.

We construct a DDH distinguisher $\mathcal{D}$ that, given a tuple $(g, X, Y, Z)$, must decide whether it is a DH tuple $(g, g^x, g^y, g^{xy})$ or a random tuple $(g, g^x, g^y, g^z)$.

Intuitively, $\mathcal{D}$ will treat $X$ as the public key $B$, use $Y$ as $c_1$, and embed $Z$ into $c_2$ so that in the DH case the ciphertext is a real encryption, and in the random case it is independent of the bit.

Construction of $\mathcal{D}$:

1. Receive a DDH instance $(g, X, Y, Z)$ where
    - either $X = g^b, Y = g^a, Z = g^{ab}$ (DH case),
    - or $X = g^b, Y = g^a, Z = g^c$ with random $c$ (random case).

2. Give $B := X$ to $\mathcal{A}$ as Bob’s public key.
3. $\mathcal{A}$ outputs two challenge messages $(m_0,m_1) \in \{0,1\}^2$.
4. $\mathcal{D}$ chooses a random bit $b \in \{0,1\}$ and sets
    - $c_1 := Y$, so effectively $c_1 = g^a$ for some hidden $a$,
    - if $m_b = 0$ set $c_2 := Z$,
    - if $m_b = 1$ set $c_2 := Z \cdot g^r$ for a fresh random $r \in \mathbb{Z}_q^*$ (chosen by $\mathcal{D}$).
5. Give $(c_1,c_2)$ to $\mathcal{A}$ as the challenge ciphertext.
6. $\mathcal{A}$ outputs a guess $b'$. If $b' = b$, $\mathcal{D}$ outputs "DH"; otherwise it outputs "random".

Analysis:

- **If $(g,X,Y,Z)$ is DH**: then $X = g^b, Y = g^a, Z = g^{ab}$. Conditioned on $m_b$ and the random $r$, the ciphertext $(c_1,c_2)$ has exactly the same distribution as in the real encryption scheme:
   - when $m_b = 0$, $c_2 = g^{ab} = B^a$,
   - when $m_b = 1$, $c_2 = g^{ab} g^r = B^a g^r$.
   So $\mathcal{A}$'s advantage in guessing $b$ is exactly its IND-CPA advantage $P - 1/2$.

- **If $(g,X,Y,Z)$ is random**: then $Z = g^c$ with $c$ independent of $a,b$. From Bob’s viewpoint (who would know $b$), $c_2$ is independent of $m_b$ and unrelated to $B^a$ except for a random multiplicative factor. Thus the ciphertext leaks no information about $b$; $\mathcal{A}$'s success probability is at most $1/2$.

Therefore $\mathcal{D}$ distinguishes DH tuples from random tuples with advantage exactly $P - 1/2$. If $P > 1/2$ is non-negligible, this breaks DDH.

Hence, under the DDH assumption in $\langle g \rangle$, the encryption scheme is IND-CPA secure.

---

# Exercise 4 – Bad groups for Diffie–Hellman

We explain why each listed group is unsuitable for DH key exchange.

## 1. $(\mathbb{Z}_p, +)$

The group operation is addition modulo $p$. The "exponentiation" operation corresponding to repeated group operation is
$$
n \cdot a := \underbrace{a + a + \dots + a}_{n \text{ times}} = na \bmod p.
$$

In a DH-like protocol, Alice and Bob would compute keys of the form $na$ or $mb$ and combine them by addition. However, computing the "discrete log" in this additive group is trivial:

- Given $a,na \in \mathbb{Z}_p$, we can recover $n$ as
   $$
   n \equiv a^{-1} (na) \pmod p
   $$
   since $a \in \mathbb{Z}_p^*$ has an inverse.

Thus the analogue of the discrete logarithm problem is easy: a passive adversary can recover the private scalars and the shared key by a single modular inverse and multiplication. This makes the group completely insecure for DH.

## 2. Upper-triangular unipotent matrices

Consider the group
$$
G = \left\{ \begin{pmatrix} 1 & a \\ 0 & 1 \end{pmatrix} : a \in \mathbb{Z}_p \right\}
$$
under matrix multiplication modulo $p$.

Compute the group operation:
$$
\begin{pmatrix} 1 & a \\ 0 & 1 \end{pmatrix}
\begin{pmatrix} 1 & b \\ 0 & 1 \end{pmatrix}
=
\begin{pmatrix} 1 & a + b \\ 0 & 1 \end{pmatrix} \pmod p.
$$

So this group is actually **isomorphic** to $(\mathbb{Z}_p,+)$ via the map
$$
\phi: a \mapsto \begin{pmatrix} 1 & a \\ 0 & 1 \end{pmatrix}.
$$

As in case 1, "exponentiation" (repeated group operation) corresponds to multiplying the upper-right entry by the scalar:
$$
\phi(a)^n = \begin{pmatrix} 1 & na \\ 0 & 1 \end{pmatrix}.
$$

Thus the discrete-log-like problem is again trivial: given $\phi(a)$ and $\phi(a)^n$, an adversary reads off the upper-right entries and computes $n$ via a modular inverse. Therefore DH in this group is also completely insecure.

## 3. A full cycle in $S_n$

Let $g \in S_n$ be a full $n$-cycle, so that
$$
\{1, g(1), g^2(1), \dots\} = \{1,2,\dots,n\}.
$$
The subgroup $\langle g \rangle \subset S_n$ generated by $g$ has size $n$ and is cyclic:
$$
\langle g \rangle = \{ g^0, g^1, \dots, g^{n-1} \}.
$$

Again, "exponentiation" corresponds to repeated application (composition) of the permutation. However, the discrete-log-like problem in this group is easy:

- Given $g$ and $g^k$, one can find $k$ by tracking the image of a single element, say $1$:
   - compute $g(1), g^2(1), g^3(1), \dots$ until you reach $g^k(1)$.
   - This takes at most $n$ steps.

Thus the discrete log in $\langle g \rangle$ can be solved in linear time in $n$. Since $n$ is the group order, this is essentially a brute-force attack that is still very efficient when $n$ is not astronomically large.

Therefore, using such a group for DH yields no meaningful security: an eavesdropper can recover the exponents (and hence the shared key) in $O(n)$ time.

In all three cases, the group structure makes the discrete-log-type problem easy, so these groups are unsuitable for secure Diffie–Hellman key exchange.

