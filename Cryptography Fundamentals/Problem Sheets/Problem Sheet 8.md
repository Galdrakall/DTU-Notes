
These practice problems have the purpose of helping you understand the material better and learning the
skills that are necessary to analyze cryptographic constructions, and sometimes to prepare you for the next
class. All answers should be supported by a written justification. To gauge whether a justification is
sufficient, ask yourself if your peers would be convinced by it without additional explanations.

We denote vectors as  
$\vec{x} \in \{0, 1\}^{\lambda}$.  
By $\vec{x}[i]$ we denote the $i$th index of $\vec{x}$, where $i \in \{1, \dots, \lambda\}$.

As in the lecture, we write  
$k \leftarrow K$  
if $k$ is sampled from the set $K$ such that it can be each element from $K$ with equal probability $1/|K|$.

For two strings $x, y$ we use $x|y$ to denote the string obtained from concatenating $x$ with $y$.

Throughout this problem sheet, let $p$ be a prime.

---

## Discrete Gaussian Distribution

Recall the definition of the discrete Gaussian probability distribution

$$
q_\sigma(z) = \frac{d_\sigma(z)}{\nu_\sigma},
$$

where

$$
d_\sigma(z) = \frac{1}{\sqrt{2\pi}\sigma} e^{- \frac{z^2}{2\sigma^2}}
$$

is the probability density of the normal distribution $N(0, \sigma^2)$ with mean 0 and variance $\sigma^2$, and

$$
\nu_\sigma = \sum_{z \in \mathbb{Z}} d_\sigma(z).
$$

Some helpful facts about the discrete Gaussian distribution:

1. $\nu_\sigma \ge 1 - d_\sigma(0)$.
2. For a random variable $X \sim q_\sigma$ the following tail bound holds. For any integer $x > 1$,

$$
\Pr[X \ge x] \le \frac{\sigma}{\nu_\sigma \cdot (x - 1)} \cdot \sqrt{2\pi} \cdot e^{- \frac{(x-1)^2}{2\sigma^2}}.
$$

---

## Recap: Regev Public-Key Encryption Scheme

### Keygen()
1. Sample $A \leftarrow \mathbb{Z}_p^{\ell \times n}$ and $s \leftarrow \mathbb{Z}_p^{n}$ uniformly at random.
2. Let $e \in \mathbb{Z}_p^{\ell}$ where each $e_i$ is distributed according to $q_\sigma$.
3. Set $b = As + e \bmod p$.
4. Output $pk = (A, b)$ as public key and $sk = (s)$ as private key.

---

### Enc$(pk, m)$

To encrypt the message $m \in \{0, 1\}$ using public key $pk = (A, b)$:

1. Let $r \in \{0, 1\}^{\ell}$ be uniformly random.
2. Set  
   $$
   c_0 \leftarrow r^\top A \bmod p
   $$
   and
   $$
   c_1 = r^\top b + m \cdot \frac{p-1}{2}.
   $$
3. Output $c = (c_0, c_1)$.

---

### Dec$(sk, c)$

To decrypt a ciphertext $c = (c_0, c_1)$ using private key $sk = (s)$:

1. Compute
   $$
   m' = \left\lfloor \frac{2(c_1 - c_0^\top s) \bmod p}{p} \right\rceil
   $$
2. Output $m'$.

---

## Exercise 1. (Decryption failure probability)

In this exercise, we derive a bound on the probability that the encryption algorithm of Regev’s
encryption scheme, on input a message $m$ produces a ciphertext $c$ such that the decryption
algorithm decrypts $c$ to a different message $m'$.

Assume that

$$
\sigma \le \frac{\varepsilon p}{\ell}
$$

for some small $\varepsilon > 0$.

1. Derive a lower bound for the probability

$$
\Pr\left[ e_i \le \frac{p}{4\ell} \ \forall i = 1, \dots, \ell \right]
$$

using the facts given above and a union bound.

2. What does this lower bound mean for the probability of decryption failure, i.e. for the probability
that encrypting a message $m$ and decrypting it again returns $m' \ne m$?

**Note:** Our analysis is quite loose here, it suffices to pick  
$\sigma = \sqrt{\varepsilon q/\ell}$,  
but it is slightly harder to show that.

---

## Exercise 2. (Broken variants of Regev’s encryption scheme)

In this exercise, you will find attacks against insecure modifications of Regev’s encryption scheme.
For each attack, give a detailed argument why it works.

1. Describe how to break Regev’s encryption scheme for $\sigma = 0$, i.e. in the case where it always
   holds that $e_i = 0$ for all $i$. More precisely, describe an algorithm that given a public key $pk$
   and a ciphertext $c = \text{Enc}_{pk}(m)$, recovers the plaintext $m$.

2. Describe how to break Regev’s encryption scheme where instead of picking the $r_i$ at random,
   $r_i = 1$ for all $i$.

3. Let $\sigma$ be chosen such that

$$
\Pr_{x \leftarrow q_\sigma}[x \ne 0 \bmod p] = \frac{1}{2\ell}.
$$

Describe how to break Regev’s encryption scheme in that case.

---

## Exercise 3. (Towards homomorphic encryption from LWE)

Consider the following symmetric-key encryption scheme for messages
$m \in \{0, 1, \dots, \ell - 1\}$.

- **Key generation:** choose uniform $s \in \mathbb{Z}_p^{n}$.

- **Encryption of message $m \in \{0, 1, \dots, \ell - 1\}$:**  
  Choose uniform $c_0 = a \in \mathbb{Z}_p^{n}$ and $e \leftarrow q_\sigma$.  
  Output $(c_0, c_1)$ with

  $$
  c_1 = a^\top \cdot s + e + \frac{m(p-1)}{\ell}.
  $$

1. Describe a decryption algorithm that gets as input a secret key $s$ and a ciphertext
   $c = (c_0, c_1)$ and outputs a plaintext, and show that your decryption algorithm is
   correct if $\sigma$ is small enough.

2. Let $c = (c_0, c_1)$ and $c' = (c'_0, c'_1)$ be ciphertexts obtained by encrypting messages
   $m$ and $m'$.

   Consider $c'' = (c''_0, c''_1)$ given by

   $$
   c''_i = c_i + c'_i \quad \text{for } i = 0,1.
   $$

   Apply the decryption algorithm you described in 1) to $c''$.

   Under which condition do you get $m + m' \bmod \ell$?

---

# Problem Sheet 8 – Full Step-by-Step Solutions

This section contains full derivations and explanations for each exercise.

---

# Exercise 1 – Decryption failure probability

Recall that $b = As + e \bmod p$, where each $e_i$ is sampled from the discrete Gaussian $q_\sigma$ and $r \in \{0,1\}^\ell$. For an encryption of $m \in \{0,1\}$ we have
$$
c_0 = r^\top A, \qquad c_1 = r^\top b + m \cdot \frac{p-1}{2},
$$
and decryption computes
$$
m' = \left\lfloor \frac{2(c_1 - c_0^\top s) \bmod p}{p} \right\rceil.
$$

Plugging in $b = As + e$ gives
$$
\begin{aligned}
c_1 - c_0^\top s &= r^\top (As + e) + m \cdot \frac{p-1}{2} - (r^\top A) s \\
&= r^\top e + m \cdot \frac{p-1}{2}.
\end{aligned}
$$
So the "noise term" is $E := r^\top e = \sum_{i=1}^\ell r_i e_i$, a sum of some of the $e_i$'s.

## 1. Lower bound on $\Pr[ e_i \le p/(4\ell) \ \forall i]$

Fix $\alpha := p/(4\ell)$. We first bound the probability that a single coordinate $e_i$ exceeds $\alpha$, then apply a union bound.

By symmetry of $q_\sigma$ and the given tail bound, for any integer $x>1$,
$$
\Pr[X \ge x] \le \frac{\sigma}{\nu_\sigma (x-1)} \sqrt{2\pi} e^{-\frac{(x-1)^2}{2\sigma^2}}.
$$
Apply this at $x = \alpha$ (rounding $\alpha$ to the nearest integer if needed). Then
$$
\Pr[e_i > \alpha] = \Pr[X > \alpha] \le \Pr[X \ge \alpha] \le \frac{\sigma}{\nu_\sigma (\alpha-1)} \sqrt{2\pi} e^{-\frac{(\alpha-1)^2}{2\sigma^2}}.
$$
Using $\sigma \le \varepsilon p/\ell$ and $\alpha = p/(4\ell)$, this upper bound becomes
$$
\Pr[e_i > p/(4\ell)] \le \frac{\varepsilon p/\ell}{\nu_\sigma (p/(4\ell)-1)} \sqrt{2\pi}\, e^{-\Theta\big((p/\ell\sigma)^2\big)},
$$
which is exponentially small in the security parameter for suitable choices of $p,\ell,\sigma$. By a union bound over all $\ell$ coordinates,
$$
\Pr\Big[\exists i : e_i > \tfrac{p}{4\ell}\Big] \le \ell \cdot \Pr[e_1 > p/(4\ell)],
$$
so we obtain the explicit lower bound
$$
\Pr\Big[ e_i \le \tfrac{p}{4\ell} \\ \forall i\Big] \ge 1 - \ell \cdot \Pr[e_1 > p/(4\ell)],
$$
where $\Pr[e_1 > p/(4\ell)]$ is given by the tail-expression above and is negligible for the chosen parameters.

## 2. Implication for decryption failure probability

Condition on the event that $e_i \le p/(4\ell)$ for all $i$ and that $r_i \in \{0,1\}$. Then
$$
|E| = \left|\sum_{i=1}^\ell r_i e_i\right| \le \sum_{i=1}^\ell |r_i e_i| \le \sum_{i=1}^\ell \frac{p}{4\ell} = \frac{p}{4}.
$$

Thus
- if $m = 0$, we have $c_1 - c_0^\top s = E$ with $|E| < p/4$;
- if $m = 1$, we have $c_1 - c_0^\top s = E + \frac{p-1}{2}$, so this quantity lies in an interval of length $p/2$ centered around $\frac{p-1}{2}$ and at distance at least $p/4$ from 0 (mod $p$).

In both cases the value $(c_1 - c_0^\top s) \bmod p$ stays well inside the interval that decodes correctly to $m$ when we compute
$$
\left\lfloor \frac{2(\cdot)}{p} \right\rceil.
$$
Hence, conditioned on the event $e_i \le p/(4\ell)$ for all $i$, decryption succeeds.

Therefore the decryption failure probability is at most the probability that this event fails, i.e.
$$
\Pr[m' \ne m] \le \Pr\Big[\exists i : e_i > \frac{p}{4\ell}\Big] \le \ell \cdot \delta(\varepsilon,\ell,p),
$$
which is negligible for appropriate parameter choices.

---

# Exercise 2 – Broken variants of Regev’s encryption scheme

We analyze three insecure modifications.

## 1. Case $\sigma = 0$ (all $e_i = 0$)

If $\sigma = 0$, then $e = 0$ always and the public key satisfies
$$
b = As \bmod p.
$$
Thus $(A,b)$ is an exact linear system with no noise. An attacker can solve for $s$ efficiently:

1. Treat $A$ as an $\ell \times n$ matrix over $\mathbb{Z}_p$ and $b$ as an $\ell$-dimensional vector.
2. Solve the linear system $As = b$ over $\mathbb{Z}_p$ using Gaussian elimination to obtain $s$.

Once $s$ is known, any ciphertext $c = (c_0,c_1)$ can be decrypted by computing
$$
m = \left\lfloor \frac{2(c_1 - c_0^\top s) \bmod p}{p} \right\rceil,
$$
exactly as the legitimate receiver. Hence the scheme is completely broken: the attacker recovers the secret key and decrypts all ciphertexts.

## 2. Case $r_i = 1$ for all $i$

If $r_i = 1$ for all $i$, then $r = (1,\dots,1)$ is fixed and public. Encryption becomes
$$
\begin{aligned}
c_0 &= r^\top A = \sum_{i=1}^\ell A_i,\\
c_1 &= r^\top b + m \cdot \frac{p-1}{2} = \sum_{i=1}^\ell b_i + m \cdot \frac{p-1}{2},
\end{aligned}
$$
where $A_i$ and $b_i$ are the rows of $A$ and entries of $b$, respectively.

Critically, the randomness $r$ is no longer sampled per encryption; for a fixed public key, $r$ and hence $r^\top A$ and $r^\top b$ are fixed. The only message-dependent term is the additive offset $m\cdot\tfrac{p-1}{2}$.

Attack (breaking IND-CPA):

1. In the IND-CPA game, ask for encryptions of $m_0 = 0$ and $m_1 = 1$ (or equivalently, run the public encryption algorithm on both messages) to obtain reference ciphertexts $c^{(0)} = (c_0, c^{(0)}_1)$ and $c^{(1)} = (c_0, c^{(1)}_1)$ under the same key. Note that $c_0$ is the same in both cases.
2. When given a challenge ciphertext $c^* = (c_0^*, c_1^*)$ for an unknown bit $b$, compare $c_1^*$ to $c^{(0)}_1$ and $c^{(1)}_1$:
   - If $c_1^* \equiv c^{(0)}_1 \pmod p$, output $b'=0$.
   - If $c_1^* \equiv c^{(1)}_1 \pmod p$, output $b'=1$.

Because $r$ is fixed and the only difference between encrypting 0 and 1 is the constant offset $\tfrac{p-1}{2}$, this adversary recovers $b$ with probability 1 (ignoring negligible noise effects), so the scheme is not semantically secure.

## 3. Case $\Pr_{x \leftarrow q_\sigma}[x \ne 0 \bmod p] = 1/(2\ell)$

In this case, each error coordinate $e_i$ is nonzero modulo $p$ with probability $1/(2\ell)$ and zero otherwise. Let $e$ be the error vector in the public key and, for simplicity, assume the $e_i$ are independent.

The expected number of nonzero coordinates in $e$ is
$$
\mathbb{E}[\#\{i : e_i \ne 0\}] = \ell \cdot \frac{1}{2\ell} = \frac{1}{2}.
$$
Thus, with noticeable probability, **no** error occurs (all $e_i = 0$). More precisely,
$$
\Pr[\forall i : e_i = 0] = \left(1 - \frac{1}{2\ell}\right)^\ell \approx e^{-1/2},
$$
which is a constant bounded away from 0.

Condition on the event that $e = 0$. Then the public key again satisfies $b = As \bmod p$, and we are back in the $\sigma = 0$ case:

1. With constant probability over key generation, $e = 0$.
2. For such a public key, an attacker can solve $As = b$ to recover $s$ as before.

Once $s$ is known for such a "weak" key, the attacker can decrypt all ciphertexts under that key. Because the fraction of weak keys is non-negligible, the scheme is insecure.

Alternative view: the public key distribution is a mixture of an LWE-like distribution and a pure linear system distribution. A distinguisher can detect when $e=0$ by checking whether $b$ lies exactly in the column span of $A$, and then mount the key-recovery attack.

---

# Exercise 3 – Towards homomorphic encryption from LWE

We consider a symmetric-key scheme for messages $m \in \{0,1,\dots,\ell-1\}$.

- Key generation: choose $s \leftarrow \mathbb{Z}_p^n$ uniformly at random.
- Encryption of $m$:
  - Choose $a = c_0 \leftarrow \mathbb{Z}_p^n$ uniformly.
  - Sample $e \leftarrow q_\sigma$.
  - Set
    $$
    c_1 = a^\top s + e + \frac{m(p-1)}{\ell}.
    $$
  - Output $c = (c_0,c_1) = (a,c_1)$.

## 1. Decryption algorithm and correctness

Given secret key $s$ and ciphertext $c = (a,c_1)$, compute
$$
v := c_1 - a^\top s = e + \frac{m(p-1)}{\ell}.
$$
If $\sigma$ is small, $e$ will be small with high probability (in absolute value, seen as an integer in $(-p/2,p/2]$).

Natural decryption procedure:

1. Compute $v = c_1 - a^\top s \bmod p$ and interpret $v$ as an integer in $(-p/2,p/2]$.
2. Scale and round:
   $$
   t := \frac{\ell}{p-1} \cdot v.
   $$
3. Output
   $$
   m' = \operatorname*{round}(t),
   $$
   where we round to the nearest integer in $\{0,1,\dots,\ell-1\}$.

**Correctness condition.** Write
$$
v = e + \frac{m(p-1)}{\ell}.
$$
Then
$$
t = \frac{\ell}{p-1} v = m + \frac{\ell}{p-1} e.
$$
If $\left|\frac{\ell}{p-1} e\right| < \tfrac{1}{2}$, then rounding $t$ recovers $m$ exactly. That is, if
$$
|e| < \frac{p-1}{2\ell},
$$
decryption succeeds. For sufficiently small $\sigma$ this event happens with overwhelming probability, so the decryption algorithm is correct except with negligible error.

## 2. Homomorphic addition: decrypting $c'' = c + c'$

Let $c = (a,c_1)$ encrypt $m$ and $c' = (a',c_1')$ encrypt $m'$, with corresponding errors $e,e'$. Then
$$
\begin{aligned}
c_1 &= a^\top s + e + \frac{m(p-1)}{\ell},\\
c_1' &= {a'}^\top s + e' + \frac{m'(p-1)}{\ell}.
\end{aligned}
$$

Define
$$
c'' = (c_0'',c_1'') \quad\text{with}\quad c_i'' = c_i + c'_i \text{ for } i=0,1,
$$
so $c_0'' = a + a'$ and $c_1'' = c_1 + c_1'$.

Compute the decryption offset for $c''$:
$$
\begin{aligned}
v'' &= c_1'' - (c_0'')^\top s \\[2pt]
    &= (c_1 + c_1') - (a + a')^\top s \\[2pt]
    &= (a^\top s + e + \tfrac{m(p-1)}{\ell} + {a'}^\top s + e' + \tfrac{m'(p-1)}{\ell}) - (a^\top s + {a'}^\top s) \\[2pt]
    &= e + e' + \frac{(m + m')(p-1)}{\ell}.
\end{aligned}
$$

Applying the same decryption rule as before, we form
$$
t'' = \frac{\ell}{p-1} v'' = m + m' + \frac{\ell}{p-1}(e + e').
$$

We then output
$$
m'' = \operatorname*{round}(t'') \bmod \ell.
$$

**Condition for obtaining $m + m' \bmod \ell$.** We need the rounding to recover $m + m'$ (modulo $\ell$), so we require
$$
\left|\frac{\ell}{p-1}(e + e')\right| < \tfrac{1}{2},
$$
i.e.
$$
|e + e'| < \frac{p-1}{2\ell}.
$$

Under this condition, decrypting $c''$ yields
$$
m'' \equiv m + m' \pmod{\ell}.
$$

Thus the scheme admits (noisy) homomorphic addition of plaintexts mod $\ell$, as long as the sum of the errors $e+e'$ remains small enough in magnitude.
