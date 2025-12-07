
These practice problems have the purpose of helping you understand the material better and
learning the skills that are necessary to analyze cryptographic constructions, and sometimes to
prepare you for the next class. All answers should be supported by a written justification. To
gauge whether a justification is sufficient, ask yourself if your peers would be convinced by it
without additional explanations.

We denote vectors as  
$x \in \{0,1\}^{\lambda}$.  
By $x[i]$ we denote the $i$th index of $x$, where $i \in \{1,\dots,\lambda\}$.

As in the lecture, we write  
$k \leftarrow K$  
if $k$ is sampled from the set $K$ such that it can be each element  
from $K$ with equal probability $1/|K|$.

For two strings $x, y$ we use $x|y$ to denote the string obtained  
from concatenating $x$ with $y$.

---

## Exercise 1. (Implementing RSA)

We recap the RSA algorithm from the lecture:

### Keygen()
1. Sample two different odd prime numbers $p, q$ and set $N = p \cdot q$.
2. Set $\varphi(N) = (p-1)\cdot(q-1)$ and choose $e$ such that  
   $\gcd(e, \varphi(N)) = 1$. Then compute  
   $d = e^{-1} \bmod \varphi(N)$.
3. Output $pk = (e, N)$ as public key and $sk = (d, N)$ as private key.

### Enc$(pk, m)$  
To encrypt the message $m \in \mathbb{Z}_N^*$ using public key $pk = (e, N)$:

1. Compute $c = m^e \bmod N$
2. Output $c$.

### Dec$(sk, c)$  
To decrypt a ciphertext $c \in \mathbb{Z}_N^*$ using private key $sk = (d, N)$:

1. Compute $m = c^d \bmod N$
2. Output $m$.

---

### Tasks

1. Implement the three algorithms constituting RSA in a programming language of your
   choice. For simplicity, you may hard-code the primes for key generation into the algo-
   rithm (i.e. pick them in advance). Make sure that $p, q$ are sufficiently large, i.e. that
   their size is $\ge 2^{100}$ each.

2. Test that your implementation yields a correct cryptosystem. Moreover, add code to
   measure how fast encryption and decryption are.

---

# Problem Sheet 7 – Full Step-by-Step Solutions

This section contains full derivations, logical steps, and explanations for each exercise, not just the final results.

---

# Exercise 1 – Implementing RSA

We recap RSA and show correctness and efficiency properties that your implementation should satisfy.

## Step 1 – Key generation

1. Choose two distinct large primes $p,q$ (for the exercise, at least 100 bits each; in practice much larger).
2. Compute
   - $N = p \cdot q$,
   - $\varphi(N) = (p-1)(q-1)$.
3. Choose a public exponent $e$ such that $\gcd(e,\varphi(N)) = 1$ (commonly $e = 65537 = 2^{16} + 1$).
4. Compute the modular inverse
   $$d \equiv e^{-1} \pmod{\varphi(N)},$$
   i.e. find $d$ such that $ed \equiv 1 \pmod{\varphi(N)}$ using the extended Euclidean algorithm.
5. Output
   - public key $pk = (e,N)$,
   - secret key $sk = (d,N)$.

## Step 2 – Encryption

Message space: $m \in \mathbb{Z}_N^*$. Given $pk = (e,N)$:

1. Compute the ciphertext
   $$c = m^e \bmod N$$
   using fast exponentiation (square-and-multiply).
2. Output $c$.

## Step 3 – Decryption

Given $sk = (d,N)$ and $c \in \mathbb{Z}_N^*$:

1. Compute
   $$m' = c^d \bmod N.$$
2. Output $m'.$

## Step 4 – Correctness

Let $m \in \mathbb{Z}_N^*$. Encryption gives $c = m^e \bmod N$. Decryption computes
$$
m' = c^d \bmod N = (m^e)^d \bmod N = m^{ed} \bmod N.
$$
Since $ed \equiv 1 \pmod{\varphi(N)}$, there exists $k$ such that $ed = 1 + k\varphi(N)$. Then
$$
m^{ed} = m^{1 + k\varphi(N)} = m \cdot (m^{\varphi(N)})^k.
$$
By Euler's theorem $m^{\varphi(N)} \equiv 1 \pmod N$, so
$$
m^{ed} \equiv m \cdot 1^k \equiv m \pmod N,
$$
and hence $m' = m$. Thus decryption always recovers the original message.

## Step 5 – Runtime intuition

Using binary exponentiation, computing $m^e \bmod N$ or $c^d \bmod N$ needs $O(\log e)$ or $O(\log d)$ multiplications modulo $N$. For a small fixed $e$ (like $65537$), encryption is significantly faster than decryption.

## Exercise 2. (The (Simple) Chinese Remainder Theorem)

Assume that we have 2 equations

- $x = a_1 \bmod b_1$
- $x = a_2 \bmod b_2$

Then the Chinese Remainder Theorem (CRT) states that there exists a unique solution for
$x \bmod (b_1 \cdot b_2)$ if and only if $\gcd(b_1, b_2) = 1$.

For example: Let  
$x = 3 \bmod 7$ and $x = 5 \bmod 9$, then since $\gcd(7, 9) = 1$ they must have a
unique solution.

To find the solution, compute  
$T \leftarrow b_1^{-1} \bmod b_2$,  
let $u \leftarrow (a_2 - a_1) \cdot T \bmod b_2$  
and set  
$x \leftarrow a_1 + u \cdot b_1$.

---

### Tasks

1. For the example $x = 3 \bmod 7$ and $x = 5 \bmod 9$ find the unique solution
   $x \bmod 63$.

2. Show that this algorithm always terminates if $\gcd(b_1, b_2) = 1$ and that its
   output is correct.

---

# Exercise 2 – The (Simple) Chinese Remainder Theorem

We have two congruences

- $x \equiv a_1 \pmod{b_1}$,
- $x \equiv a_2 \pmod{b_2}$,

with $\gcd(b_1,b_2) = 1$. The algorithm is:

1. Compute $T \leftarrow b_1^{-1} \bmod b_2$.
2. Let $u \leftarrow (a_2 - a_1) \cdot T \bmod b_2$.
3. Set $x \leftarrow a_1 + u \cdot b_1$.

We solve the concrete example and then prove termination and correctness.

## Part 1 – Example $x \equiv 3 \pmod 7$, $x \equiv 5 \pmod 9$

Here $a_1 = 3$, $b_1 = 7$, $a_2 = 5$, $b_2 = 9$ and $\gcd(7,9)=1$.

1. Compute $T = b_1^{-1} \bmod b_2 = 7^{-1} \bmod 9$.
   We need $7T \equiv 1 \pmod 9$. Try $T=4$:
   $$7 \cdot 4 = 28 \equiv 1 \pmod 9,$$
   so $T = 4$.
2. Compute
   $$u = (a_2 - a_1)T \bmod b_2 = (5-3) \cdot 4 \bmod 9 = 2 \cdot 4 \bmod 9 = 8.$$
3. Set
   $$x = a_1 + u b_1 = 3 + 8 \cdot 7 = 3 + 56 = 59.$$

Check:

- $59 \bmod 7 = 59 - 8 \cdot 7 = 59 - 56 = 3$,
- $59 \bmod 9 = 59 - 6 \cdot 9 = 59 - 54 = 5$.

Thus the unique solution modulo $63 = 7\cdot 9$ is
$$x \equiv 59 \pmod{63}.$$

## Part 2 – Termination and correctness

**Termination.** Computing $T = b_1^{-1} \bmod b_2$ uses the extended Euclidean algorithm, which always terminates and succeeds because $\gcd(b_1,b_2) = 1$. The remaining arithmetic is simple modular multiplication/addition, so the algorithm always terminates.

**Correctness.** Let $T,u,x$ be defined as above.

- Modulo $b_1$:
  $$x = a_1 + u b_1 \equiv a_1 \pmod{b_1},$$
  since $b_1 \mid u b_1$. Hence $x \equiv a_1 \pmod{b_1}$.

- Modulo $b_2$:
  Use $u \equiv (a_2 - a_1)T \pmod{b_2}$ and $b_1 T \equiv 1 \pmod{b_2}$:
  $$\begin{aligned}
  x &\equiv a_1 + u b_1 \\[-2pt]
    &\equiv a_1 + b_1 (a_2 - a_1)T \\[-2pt]
    &\equiv a_1 + (a_2 - a_1) \cdot 1 \\[-2pt]
    &\equiv a_2 \pmod{b_2}.
  \end{aligned}$$

Thus $x$ satisfies both congruences.

**Uniqueness modulo $b_1b_2$.** If $x$ and $x'$ both satisfy the system, then

- $x \equiv x' \pmod{b_1}$ and $x \equiv x' \pmod{b_2}$,
- so $b_1 \mid (x-x')$ and $b_2 \mid (x-x')$.

With $\gcd(b_1,b_2)=1$, it follows that $b_1b_2 \mid (x-x')$, i.e.
$$x \equiv x' \pmod{b_1b_2}.$$
So the solution is unique modulo $b_1b_2$.

## Exercise 3. (The Chinese Remainder Theorem and RSA)

When using RSA in practice, one usually chooses  
$e = 2^{16} + 1$  
independent of $N$ so $e$ is small and prime. Since it is small, encrypting will only need
17 multiplications modulo $N$. Moreover, since $e$ is prime it will almost always be
coprime to $\varphi(N)$.

1. Show that you can compute $c = m^e \bmod N$ with only 17 multiplications modulo $N$.

2. Unfortunately, this means that $d$ may now be a very large number, so decryption will
   take a long time. For example, if $p, q$ are each 1000 bits long then $N$ is around
   2000 bits long so we may have to perform many operations modulo an integer of size
   2000 bits.

   Show that, assuming Keygen outputs  
   $sk' = (d, p, q)$ instead of $(d, N)$,  
   then you can do a different, possibly faster decryption using the Chinese Remainder
   Theorem!

3. Update your encryption and decryption algorithm from Exercise 1 to use the fixed expo-
   nent $e = 2^{16} + 1$ and the new decryption method. Measure how fast your new
   encryption and decryption code is and compare to your implementation of Exercise 1.

4. **Bonus question:** If $N$ has 2000 bits then also $\varphi(N)$ will have the same
   length (or maybe 1 bit shorter). Assuming $e = 2^{16} + 1$, show that in this case
   $d$ requires $> 1980$ bits to write it down, i.e.  
   $d > 2^{1980}$.

---

# Exercise 3 – The Chinese Remainder Theorem and RSA

We fix $e = 2^{16} + 1 = 65537$.

## Part 1 – 17 multiplications for $c = m^e \bmod N$

Write $e$ in binary:
$$e = 2^{16} + 1 = 1\,0000000000000001_2,$$
which has 17 bits and exactly two 1-bits.

Using binary exponentiation (square-and-multiply) to compute $m^e \bmod N$:

- We process the 17 bits of $e$ from most significant to least.
- For each bit we square once (one modular multiplication).
- When the bit is 1 we also multiply by $m$ once (one more modular multiplication).

Thus exponentiation by a 17-bit exponent requires at most 17–18 modular multiplications, and with a careful implementation one can arrange the algorithm so that it uses **at most 17 multiplications modulo $N$**. The key point is that the number of multiplications grows linearly with the bitlength of $e$, and here $e$ has only 17 bits.

## Part 2 – Faster decryption using CRT and $sk' = (d,p,q)$

Naive decryption computes
$$m = c^d \bmod N,$$
where $N = pq$ is about 2000 bits (if $p,q$ are 1000 bits). This is expensive.

With $sk' = (d,p,q)$ we can use CRT to speed this up:

1. Precompute
   $$d_p = d \bmod (p-1), \quad d_q = d \bmod (q-1).$$
2. Given ciphertext $c$:
   - Compute $m_p = c^{d_p} \bmod p$.
   - Compute $m_q = c^{d_q} \bmod q$.
3. Recombine $m_p, m_q$ to get $m \bmod N$ using CRT:
   - Compute $q_{\text{inv}} = q^{-1} \bmod p$.
   - Let $h = (m_p - m_q) q_{\text{inv}} \bmod p$.
   - Set
     $$m = m_q + h q \bmod N.$$

Here exponentiation is done modulo the 1000-bit numbers $p$ and $q$, which is much faster than working modulo the 2000-bit $N$. The extra CRT recombination work is negligible compared to exponentiation.

## Part 3 – Updated implementation (conceptual)

- **Keygen**: same as Exercise 1, but keep $p,q$ and optionally $d_p,d_q,q_{\text{inv}}$ with the secret key.
- **Enc(pk,m)**: unchanged, still compute $c = m^e \bmod N$ with $e = 65537$.
- **Dec(sk',c)**: use the CRT-based method above.

If you time both versions in code (direct $c^d \bmod N$ vs CRT-based), you will observe a significant speedup for decryption using CRT.

## Part 4 – Bonus: $d > 2^{1980}$ when $N$ has 2000 bits

Assume $N = pq$ has 2000 bits, so roughly $2^{1999} \le N < 2^{2000}$ and $p,q$ are about 1000-bit primes. Then $\varphi(N) = (p-1)(q-1)$ satisfies
$$\varphi(N) = pq - (p+q) + 1 \approx N,$$
so $\varphi(N)$ is also about 2000 bits long.

We know $e d \equiv 1 \pmod{\varphi(N)}$ and $0 < d < \varphi(N)$. Thus there exists some integer $k$ such that
$$e d = 1 + k\varphi(N).$$
The smallest positive solution has $k = 1$, giving approximately
$$d \approx \frac{\varphi(N)}{e}.$$

Since $e = 2^{16} + 1 < 2^{17}$ and $\varphi(N) \ge 2^{1999}$ for a 2000-bit $N$, we get
$$
d > \frac{2^{1999} - 1}{2^{17}} > 2^{1999-17} - 1 = 2^{1982} - 1 > 2^{1980}.
$$
So $d$ must have **more than 1980 bits**, as claimed.

## Exercise 4. (Finding $\varphi(N)$ implies factoring)

Assume you have an algorithm $A$ which, on input $N$, outputs the value $\varphi(N)$.
Then show that any such algorithm $A$ can efficiently factor RSA numbers in approximately
the same runtime.

Your approach may look as follows:

1. Show that knowledge of $\varphi(N)$ and $N$ allows you to reconstruct $p + q$. To see
   this, look at how $\varphi(N)$ is defined if $N = p \cdot q$ for two different primes
   $p, q$.

2. Consider the polynomial  
   $f(X) = (X - p) \cdot (X - q)$.  
   What do you know about its coefficients and how can you compute its roots?

---

# Exercise 4 – Finding $\varphi(N)$ implies factoring

Assume we have an algorithm $A$ that, on input an RSA modulus $N$, outputs $\varphi(N)$. We show how to use $A$ to factor $N$ efficiently.

## Part 1 – Recover $p+q$ from $N$ and $\varphi(N)$

For an RSA modulus $N = pq$ with distinct primes $p,q$ we have
$$\varphi(N) = (p-1)(q-1) = pq - p - q + 1 = N - (p+q) + 1.$$
Rearranging gives
$$p + q = N + 1 - \varphi(N).$$

So, given $N$ and $\varphi(N)$ from $A(N)$, we can compute the sum
$$S := p + q = N + 1 - \varphi(N).$$

## Part 2 – Use the polynomial $f(X) = (X-p)(X-q)$

Consider
$$f(X) = (X-p)(X-q) = X^2 - (p+q)X + pq.$$  
We know

- $N = pq$ (the modulus),
- $S = p+q$ (from Part 1).

Thus we know the polynomial
$$f(X) = X^2 - S X + N.$$

Its roots in $\mathbb{Z}$ are exactly $p$ and $q$.

## Part 3 – Compute the roots $p,q$

The discriminant of $f$ is
$$\Delta = S^2 - 4N.$$
Using $S = p+q$ and $N = pq$ we get
$$\Delta = (p+q)^2 - 4pq = p^2 + 2pq + q^2 - 4pq = (p-q)^2.$$
Thus $\sqrt{\Delta} = |p-q|$ is an integer.

The two roots are then
$$
p = \frac{S + \sqrt{\Delta}}{2}, \qquad q = \frac{S - \sqrt{\Delta}}{2}
$$
(or swapped). All operations (computing $S$, $\Delta$, integer square root, and dividing by 2) can be done in polynomial time in the bitlength of $N$.

Therefore, given $A$ that outputs $\varphi(N)$, we can factor any RSA modulus $N$ in polynomial time:

1. Query $A(N)$ to get $\varphi(N)$.
2. Compute $S = N + 1 - \varphi(N)$.
3. Compute $\Delta = S^2 - 4N$ and $r = \sqrt{\Delta}$.
4. Output
   $$p = \frac{S + r}{2}, \quad q = \frac{S - r}{2}.$$

This shows that computing $\varphi(N)$ is (essentially) as hard as factoring $N$.
