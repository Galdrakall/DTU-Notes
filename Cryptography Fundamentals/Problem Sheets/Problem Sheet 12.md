
---

## Exercise 1. (Period finding for breaking Diffie–Hellman)

Recall the discrete logarithm problem: given  
$p \in \mathbb{N}$ and $g, h \in \mathbb{Z}_p^*$, find $a \in \mathbb{N}$ such that

$$
g^a = h \bmod p.
$$

It turns out that the discrete logarithm problem can also be solved via **period finding**. In this case,
however, a two-dimensional version of period finding is needed. This 2D period finding problem is as
follows:

For a function  
$$
f : \mathbb{N}^2 \to \mathbb{N},
$$
find $\alpha, \beta \in \mathbb{N}$ such that

$$
f(x + \alpha, y + \beta) = f(x, y)
$$

for all $x, y \in \mathbb{N}$.

For the discrete logarithm problem as defined above, define

$$
f(x, y) = g^x h^{-y} \bmod p.
$$

Show that the function is periodic. Given a period $(\alpha, \beta)$, how can you compute the discrete
logarithm?

---

## Exercise 2. (When will we have a cryptographically relevant quantum computer?)

Take a look at the **2024 Quantum Threat Timeline Report** from the Global Risk Institute, available
here:

https://globalriskinstitute.org/publication/2024-quantum-threat-timeline-report/

When do we need to expect **RSA and Diffie–Hellman** as we use it today to be broken? What are the
implications, when do you think do we need to switch to using **post-quantum cryptography**?

---

# Problem Sheet 12 – Full Step-by-Step Solutions

---

# Exercise 1 – Period finding for breaking Diffie–Hellman

We are given $p$, a generator $g \in \mathbb{Z}_p^*$, and $h = g^a$ for some unknown $a$. We define
$$
f(x,y) = g^x h^{-y} \bmod p.
$$

## 1. Show that $f$ is periodic

Using $h = g^a$, we can rewrite
$$
f(x,y) = g^x (g^a)^{-y} = g^{x - ay} \bmod p.
$$

So $f(x,y)$ depends only on the exponent $x - ay$ modulo the order of $g$ (which we denote by $q$; typically $q = p-1$ if $g$ generates $\mathbb{Z}_p^*$, or $q \mid p-1$ if $g$ generates a subgroup of order $q$).

Now consider a pair $(\alpha,\beta)$ with
$$
\alpha - a\beta \equiv 0 \pmod q.
$$
Then for any $(x,y)$,
$$
\begin{aligned}
f(x+\alpha, y+\beta)
&= g^{x+\alpha} h^{-(y+\beta)} \\
&= g^{x+\alpha} (g^a)^{-(y+\beta)} \\
&= g^{x+\alpha} g^{-a(y+\beta)} \\
&= g^{x - ay + (\alpha - a\beta)}.
\end{aligned}
$$

Because $\alpha - a\beta \equiv 0 \pmod q$, we have
$$
g^{x - ay + (\alpha - a\beta)} = g^{x - ay} = f(x,y) \pmod p.
$$

Thus any $(\alpha,\beta)$ with $\alpha \equiv a\beta \pmod q$ is a **period** of $f$ in the sense that
$$
f(x+\alpha, y+\beta) = f(x,y) \quad \text{for all } x,y.
$$

In particular, the set of periods is exactly the set of integer pairs $(\alpha,\beta)$ satisfying
$$
\alpha - a\beta \equiv 0 \pmod q.
$$

## 2. Computing the discrete logarithm from a period

Suppose a 2D period-finding algorithm gives us a **non-trivial** period $(\alpha,\beta)$ such that
$$
\alpha - a\beta \equiv 0 \pmod q.
$$

Then
$$
\alpha \equiv a\beta \pmod q.
$$

If $\beta$ is invertible modulo $q$ (i.e. $\gcd(\beta,q)=1$), we can solve for $a$:
$$
a \equiv \alpha\, \beta^{-1} \pmod q.
$$

This gives the discrete logarithm $a$ satisfying $h = g^a$. So, once we have any non-zero $(\alpha,\beta)$ with $\alpha - a\beta \equiv 0 \pmod q$ and $\beta$ coprime to $q$, recovering $a$ is a simple modular inversion.

If we obtain multiple periods, we can choose one with $\gcd(\beta,q)=1$ (or combine several relations) to ensure invertibility.

---

# Exercise 2 – Quantum threat timeline and PQC

The 2024 Quantum Threat Timeline Report aggregates expert estimates on when large, fault-tolerant quantum computers capable of running Shor’s algorithm at cryptographically relevant sizes may appear.

While the report stresses high uncertainty, a common theme is:

- There is a **non-negligible probability** that today’s RSA and Diffie–Hellman parameters (e.g. 2048–4096-bit RSA, standard finite-field DH) become breakable **within a few decades** (roughly by mid-century), with some scenarios suggesting serious risk already in the **2030s–2040s**.

### When to expect RSA and DH to be broken

Mathematically, Shor’s algorithm breaks both integer factorization and discrete logarithms in polynomial time once we have sufficiently powerful quantum hardware. The open question is **when** such hardware arrives, not whether the underlying problems remain hard.

Given the report’s timelines, it is prudent to assume that classical RSA/DH could be at serious risk **well before 2050**, and possibly earlier in more aggressive technology scenarios.

### Implications and when to switch to PQC

Two key points drive the migration timeline:

- **Harvest now, decrypt later:** Adversaries can record encrypted traffic today and decrypt it once a large quantum computer exists. Any data needing confidentiality for 10–30+ years is already at risk if it is protected only by RSA/DH.
- **Slow migration:** Replacing cryptography in large systems (internet protocols, hardware, embedded devices, critical infrastructure) takes many years.

Consequently:

- We should **start/continue deploying post-quantum cryptography now**, at least in hybrid mode (classical + PQC), for new systems and for data with long-term confidentiality requirements.
- Existing deployments that rely solely on RSA/DH should plan to transition to PQC **well before** the earliest plausible break dates, to allow time for standards, implementation, and roll-out.

In summary, even if large quantum computers may still be years away, the combination of “harvest now, decrypt later” and long migration times means that the switch to post-quantum schemes should not be postponed until a concrete break is demonstrated.

