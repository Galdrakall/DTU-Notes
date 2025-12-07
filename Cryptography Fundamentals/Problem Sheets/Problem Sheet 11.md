
These practice problems have the purpose of helping you understand the material better and learning the
skills that are necessary to analyze cryptographic constructions, and sometimes to prepare you for the next
class. All answers should be supported by a written justification. To gauge whether a justification is
sufficient, ask yourself if your peers would be convinced by it without additional explanations.

---

## Exercise 1. (An elliptic curve group)

Let $p$ be a prime and $a, b \in \mathbb{Z}_p$. Define the set

$$
G = \{(x, y) \mid x, y \in \mathbb{Z}_p,\; y^2 = x^3 + a \cdot x + b\} \cup \{O\}.
$$

$O$ is sometimes called *the point at infinity*. We define the group law as follows. For all  
$P, Q \in G$,  
$P = (x_1, y_1)$, $Q = (x_2, y_2)$,

- $O + P = P + O = P$.
- $-P = (x, -y)$
- $P - P = O$
- If $Q \ne -P$, set

$$
\lambda =
\begin{cases}
\frac{y_2 - y_1}{x_2 - x_1} & \text{if } x_1 \ne x_2 \\
\frac{3x_1^2 + a}{2y_1} & \text{if } x_1 = x_2 \text{ and } y_1 \ne 0
\end{cases}
$$

and define $R = P + Q$,  
$R = (x_3, y_3)$ with  

$$
x_3 = \lambda^2 - x_1 - x_2, \quad
y_3 = (x_1 - x_3)\lambda - y_1.
$$

**Note:** Here all computations are mod $p$, so  
$\frac{s}{t} = s \cdot t^{-1}$ where $t^{-1}$ is the inverse of $t$ mod $p$.

### Tasks

1. For $p = 11$, $a = 2$, $b = 7$, compute  
   $P = (6,2) + (7,1)$,  
   $Q = -(7,1)$ and  
   $R = P + Q$.

2. *(Bonus exercise)* Show that the defined group addition is commutative, i.e. show that for all  
   $P, Q \in G$ it holds that $P + Q = Q + P$.

---

## Exercise 2. (X3DH)

Consider the X3DH protocol as introduced in the lecture, *without* the optional 4th Diffie–Hellman
exchange which uses $k_4 = \text{DH}(\text{EKA}, \text{EKB})$.

1. For each of the three intermediate keys $k_i$, $i = 1, 2, 3$, describe what security property is lost
   if key $k_i$ is omitted from the final key derivation.

   Example for $i = 1$: in what way is the protocol insecure if we compute the final key as  
   $k = H(k_2, k_3)$?

---

## Exercise 3. (Schnorr signatures)

In the lecture we learned about RSA-FDH signatures. There are also signature schemes which rely on
the DLOG problem, one of which is called **Schnorr’s signature scheme**.

As before, let $p$ be a prime and $g \in \mathbb{Z}_p$ of order $q$. Furthermore, let  

$$
H : \{0, 1\}^* \to \mathbb{Z}_q
$$

be a collision-resistant hash function. The Schnorr signature scheme consists of the following
three algorithms:

### Keygen

1. Sample $s \leftarrow \mathbb{Z}_q$ uniformly at random.
2. Compute $S = g^s \bmod p$.
3. Output $(pk, sk) = (S, s)$.

---

### Sign on input $m \in \{0,1\}^*$ and $sk$

- Sample $a \leftarrow \mathbb{Z}_q$ uniformly at random.
- Compute $A = g^a \bmod p$.
- Compute $e = H(A, m)$ and $z = a + e \cdot s \bmod q$.
- Output $\sigma = (A, z)$.

---

### Verify on input $pk = S$, $m \in \{0,1\}^*$ and $\sigma = (A, z)$

- Compute $e = H(A, m)$.
- Output 1 if  

$$
A \cdot S^e = g^z \bmod p,
$$

otherwise output 0.

---

### Tasks

1. Verify that the Schnorr signature scheme is correct. For that, imagine that $pk, sk$ is generated
   correctly and $\sigma$ is a correctly generated signature on $m$. Then  
   $\text{Verify}(pk, m, \sigma) = 1$ should always hold.

2. Assume that there exists an algorithm $\mathcal{A}$ which, on input $g, A \in \mathbb{Z}_p$ outputs
   $a \in \mathbb{Z}_q$ such that  

$$
g^a = A \bmod p.
$$

   How can you use $\mathcal{A}$ on a Schnorr public key $pk$ to generate signatures on arbitrary
   messages that are valid for $pk$?

---

## Exercise 4. (DDH and key reuse)

For both the El Gamal encryption scheme and for X3DH, it is important that Diffie–Hellman remains
secure even if one of the private exponents has been used before. For $g \in \mathbb{Z}_p$ of order $q$,
consider therefore the following variant of the DDH problem:

**DDH′:**  
Sample $a, b, c, d, e \in \mathbb{Z}_q$ uniformly random and independent.  
The attacker must distinguish

$$
(g, g^a, g^b, g^c, g^d, g^e)
$$

from

$$
(g, g^a, g^b, g^c, g^{ab}, g^{ac}).
$$

We can write this as two libraries, similar to $L_{\text{ddh-real}}$ and $L_{\text{ddh-ideal}}$ from the
lecture, as follows:

### $L_{\text{ddhp-ideal}}$

- Initially, sample $a, b, c, d, e \in \mathbb{Z}_q$ uniformly random and independent.
- The function `GetInstance()` outputs  

$$
(g, g^a, g^b, g^c, g^d, g^e).
$$

---

### $L_{\text{ddhp-real}}$

- Initially, sample $a, b, c \in \mathbb{Z}_q$ uniformly random and independent.
- The function `GetInstance()` outputs  

$$
(g, g^a, g^b, g^c, g^{ab}, g^{ac}).
$$

---

### Tasks

1. Let $\mathcal{A}$ be an algorithm that distinguishes $L_{\text{ddhp-real}}$ from
   $L_{\text{ddhp-ideal}}$. What in the behavior of both libraries is different, so that $\mathcal{A}$
   can distinguish it?

2. We want to construct an algorithm $\mathcal{B}$ that uses $\mathcal{A}$ as a sub-routine, so that
   $\mathcal{B}$ solves DDH. The algorithm $\mathcal{B}$ would interact either with
   $L_{\text{ddh-real}}$ or $L_{\text{ddh-ideal}}$ and has to tell which one it is.

   It therefore locally simulates a library (based on the output of the library it talks to) and lets
   $\mathcal{A}$ talk to the simulated library. Can you describe this process?

   In your simulation, you should simulate $L_{\text{ddhp-real}}$ when $\mathcal{B}$ talks to
   $L_{\text{ddh-real}}$ and simulate $L_{\text{ddhp-ideal}}$ when you talk to
   $L_{\text{ddh-ideal}}$.

3. Based on your simulation, define how $\mathcal{B}$ uses the output of $\mathcal{A}$ to tell the two
   libraries apart. How successful will $\mathcal{B}$ be, if $\mathcal{A}$ is correct
   $\alpha$ percent of the time?

---

## Exercise 5. (The Pedersen Commitment)

Commitments are an advanced cryptographic primitive. They allow a sender to “commit” to a message
$m$ towards the receiver by sending a value $c$. Having only $c$ (i.e. before $m$ is “opened” to the
receiver), the receiver cannot say what message $m$ is contained inside $c$. At the same time, once
$c$ is sent to the receiver then the sender cannot change his mind and open $c$ to another message
$m'$ anymore towards the sender.

More formally, a commitment scheme consists of two algorithms:

- **Commit** – A `Com` algorithm which, on input $m$, outputs values $c, d$.
- **Open** – An `Open` algorithm which, on input $m, c, d$ outputs a bit.

It is required that the commitment scheme is **binding** and **hiding**.

- **Binding:** It should be computationally difficult for a sender to generate values
  $m, m', d, d', c$ such that  

$$
\text{Open}(m, c, d) =
\text{Open}(m', c, d') = 1
$$

  while $m \ne m'$. Note that sender has a free choice of all these values, as long as both
  messages $m, m'$ are different but open the same commitment $c$ which the
  sender can also choose.

- **Hiding:** Given $m_0, m_1$ by an adversary, this adversary should not be able to
  decide if it is a commitment to $m_0$ or $m_1$ for an honestly generated commitment $c$
  (similar to the IND-CPA property for encryption schemes, where the adversary can pick
  two “potential” messages but cannot say which one is ultimately encrypted in the ciphertext).

---

Towards constructing a commitment scheme, let us, as before, assume that $p$ is a prime and
$g \in \mathbb{Z}_p^*$ is of large prime order $q \mid p - 1$. We assume that $p, q, g$ are public
knowledge for everyone.

A first attempt for a commitment scheme is the following:

### Commit
On input $m \in \mathbb{Z}_q$, output  

$$
c = g^m \bmod p
$$

and $d = \bot$.

### Open
On input $m \in \mathbb{Z}_q$, $c \in \mathbb{Z}_p^*$ output 1 if  

$$
c = g^m \bmod p
$$

and 0 otherwise.

Show that this construction is insecure because it is *not hiding*.

---

A version of this, which bears resemblance to the previous exercises, is actually secure!
It is called the **Pedersen Commitment** and it works as follows, assuming an additional
$h \in \langle g \rangle$ (i.e. $h = g^a$ for some value $a$). $h$ is also of prime order $q$
and also publicly known (and fixed for sender and receiver):

### Commit
On input $m \in \mathbb{Z}_q$, sample a random $r \in \mathbb{Z}_q$ and output  

$$
c = g^m h^r \bmod p
$$

and $d = r$. The receiver will obtain $c$ while the sender keeps $d$ to itself.

### Open
On input $m \in \mathbb{Z}_q$, $c \in \mathbb{Z}_p^*$, $d \in \mathbb{Z}_q$ output 1 if  

$$
c = g^m h^r \bmod p
$$

otherwise output 0.

---

### Tasks

1. Assume that neither sender nor receiver know the discrete logarithm of $h$ to the base $g$
   modulo $p$. Then the aforementioned commitment scheme is **binding**.

   To prove this, assume for contradiction that there exists a sender algorithm that can, on input
   $p, q, g, h$, generate values $m, m', c, r, r'$ such that

$$
g^m h^r \bmod p = g^{m'} h^{r'} \bmod p
$$

   where $m \ne m'$. Then show that you can use this sender algorithm to compute the discrete
   logarithm of $h$ to base $g$ modulo $p$!

2. Show that the commitment scheme is also **hiding**! You can show that an honestly generated
   commitment  

$$
c = g^m h^r \bmod p
$$

   could have been generated by any other message $m'$ using a different randomness $r'$.

---

# Problem Sheet 11 – Full Step-by-Step Solutions

This section contains complete solutions and explanations for each exercise.

---

# Exercise 1 – An elliptic curve group

We work over $\mathbb{Z}_{11}$ with $a=2$, $b=7$ and curve
$$
E: y^2 = x^3 + 2x + 7 \pmod{11}.
$$

## 1. Compute $P = (6,2) + (7,1)$, $Q = -(7,1)$, and $R = P+Q$

First compute $P = (6,2) + (7,1)$.

Here $P_1=(x_1,y_1)=(6,2)$ and $P_2=(x_2,y_2)=(7,1)$ with $x_1 \ne x_2$, so
$$
\lambda = \frac{y_2-y_1}{x_2-x_1} = \frac{1-2}{7-6} = \frac{-1}{1} \equiv -1 \equiv 10 \pmod{11}.
$$

Then
$$
\begin{aligned}
x_3 &= \lambda^2 - x_1 - x_2 
       = 10^2 - 6 - 7 = 100 - 13 = 87 \equiv 10 \pmod{11},\\
y_3 &= (x_1 - x_3)\lambda - y_1 
       = (6 - 10)\cdot 10 - 2 = (-4)\cdot 10 - 2 = -40 - 2 = -42 \equiv 2 \pmod{11}.
\end{aligned}
$$

So
$$
P = (6,2) + (7,1) = (10,2).
$$

Next, $Q = -(7,1)$ is obtained by negating the $y$-coordinate:
$$
Q = (7,-1) \equiv (7,10) \pmod{11}.
$$

Finally, $R = P + Q = (10,2) + (7,10)$. We have $x_1=10$, $y_1=2$, $x_2=7$, $y_2=10$ and $x_1 \ne x_2$:
$$
\lambda = \frac{y_2 - y_1}{x_2 - x_1} = \frac{10-2}{7-10} = \frac{8}{-3}.
$$
Compute inverses modulo 11: $-3 \equiv 8$, and $8^{-1} \equiv 7$ since $8\cdot 7 = 56 \equiv 1$.
Thus
$$
\lambda = 8 \cdot 7 = 56 \equiv 1 \pmod{11}.
$$
Then
$$
\begin{aligned}
x_3 &= \lambda^2 - x_1 - x_2 = 1 - 10 - 7 = -16 \equiv 6 \pmod{11},\\
y_3 &= (x_1 - x_3)\lambda - y_1 = (10 - 6)\cdot 1 - 2 = 4 - 2 = 2.
\end{aligned}
$$

So
$$
R = P + Q = (6,2).
$$

This matches the intuition that adding a point and the negative of another point effectively reflects across the curve.

*(Bonus commutativity is omitted here, but follows from the geometric/chord-and-tangent interpretation and can be proved algebraically.)*

---

# Exercise 2 – X3DH (without $k_4$)

We consider X3DH without the optional $k_4 = \mathrm{DH}(\mathrm{EKA},\mathrm{EKB})$. The three remaining DH contributions (using Signal’s notation IK=identity key, SPK=static prekey, OPK=one-time prekey, EK=ephemeral key) typically are:

- $k_1 = \mathrm{DH}(\mathrm{IK_A}, \mathrm{SPK_B})$,
- $k_2 = \mathrm{DH}(\mathrm{EK_A}, \mathrm{IK_B})$,
- $k_3 = \mathrm{DH}(\mathrm{EK_A}, \mathrm{SPK_B})$ or $\mathrm{DH}(\mathrm{EK_A}, \mathrm{OPK_B})$.

For each, we describe what is lost if it is omitted from the final KDF input.

## 1. Omitting $k_1$ (use only $k = H(k_2,k_3)$)

$k_1$ ties Alice’s *long-term* identity key to Bob’s static prekey. If we omit $k_1$:

- The resulting key no longer depends on Alice’s long-term identity key.
- This weakens **authentication / identity binding**: a man-in-the-middle who controls Alice’s identity key (or replaces it) may not be detected, since the derived key does not incorporate $\mathrm{IK_A}$.

So omitting $k_1$ compromises the assurance that the key is bound to Alice’s identity.

## 2. Omitting $k_2$ (use only $k = H(k_1,k_3)$)

$k_2 = \mathrm{DH}(\mathrm{EK_A},\mathrm{IK_B})$ ties Alice’s ephemeral key to Bob’s **identity** key. If we omit $k_2$:

- The derived key may no longer be strongly bound to Bob’s long-term identity.
- This weakens **server or directory compromise resistance**: an attacker who registers a fake prekey for Bob can induce sessions that do not depend on Bob’s true identity key.

So omitting $k_2$ hurts Bob’s authentication: Alice cannot be sure she is talking to Bob’s long-term identity rather than an attacker’s substituted static key.

## 3. Omitting $k_3$ (use only $k = H(k_1,k_2)$)

$k_3 = \mathrm{DH}(\mathrm{EK_A}, \mathrm{SPK_B} / \mathrm{OPK_B})$ is the main *ephemeral-ephemeral or ephemeral-static* DH that provides **forward secrecy** (and in the OPK case, some deniability and replay protection).

If we omit $k_3$:

- The resulting key depends only on long-term/static keys ($\mathrm{IK_A}, \mathrm{IK_B}, \mathrm{SPK_B}$).
- If any long-term secret is later compromised, past session keys can be recomputed.

Thus omitting $k_3$ largely destroys **forward secrecy**, since no ephemeral contribution remains in the KDF.

---

# Exercise 3 – Schnorr signatures

We have group $\langle g \rangle$ of order $q$ in $\mathbb{Z}_p^*$ and hash $H: \{0,1\}^* \to \mathbb{Z}_q$. Keys and algorithms:

- Keygen: $s \leftarrow \mathbb{Z}_q$, $S = g^s$, $pk = S$, $sk = s$.
- Sign: pick $a \leftarrow \mathbb{Z}_q$, compute $A = g^a$, $e = H(A,m)$, $z = a + e s \bmod q$, output $\sigma = (A,z)$.
- Verify: recompute $e = H(A,m)$, accept if
   $$
   A S^e = g^z \pmod p.
   $$

## 1. Correctness

Assume $pk,sk$ are generated correctly and $\sigma = (A,z)$ is produced by the signing algorithm. Then
$$
z \equiv a + e s \pmod q.
$$
Compute the right-hand side of the verification equation:
$$
g^z = g^{a + e s} = g^a g^{e s} = A \cdot (g^s)^e = A S^e \pmod p.
$$

Thus the verifier’s check $A S^e = g^z$ always holds for correctly generated signatures, so $\mathrm{Verify}(pk,m,\sigma) = 1$.

## 2. Using a DLOG oracle to forge Schnorr signatures

Suppose we have an algorithm $\mathcal{A}$ that, given $(g,A)$, returns $a$ such that $g^a \equiv A \pmod p$ (solves the discrete log). We want to use $\mathcal{A}$ on a Schnorr public key $S = g^s$ to sign arbitrary messages.

Given message $m$ and public key $S$:

1. Pick a random $e \leftarrow \mathbb{Z}_q$.
2. Compute $A := S^{-e} \pmod p$.
3. Use $\mathcal{A}$ to find $a$ with $g^a = A$.
4. Define $z := a + e s \bmod q$.

Observe that
$$
g^z = g^{a + e s} = g^a g^{e s} = A S^e.
$$

So $(A,z)$ satisfies the Schnorr verification equation for any message $m$ **provided** we set
$$
e = H(A,m).
$$
In the random oracle model (or if we can program $H$), we can choose $e$ first, compute $A$ and $z$ as above, and then define $H(A,m)=e$. This yields a valid signature on $m$ without knowing $s$.

Thus, an oracle that solves discrete logs for arbitrary group elements suffices to forge Schnorr signatures for any message under any public key $S$.

---

# Exercise 4 – DDH and key reuse (DDH′)

We compare
$$
L_{\text{ddhp-ideal}}: (g,g^a,g^b,g^c,g^d,g^e)
$$
vs.
$$
L_{\text{ddhp-real}}: (g,g^a,g^b,g^c,g^{ab},g^{ac}),
$$
with random $a,b,c$ (and independent random $d,e$ in the ideal case).

## 1. What differs between the two libraries?

In $L_{\text{ddhp-real}}$:

- The last two components are **correlated** with the earlier ones:
   - $g^{ab}$ is determined by $g^a$ and $g^b$;
   - $g^{ac}$ is determined by $g^a$ and $g^c$.

In $L_{\text{ddhp-ideal}}$:

- $g^d$ and $g^e$ are independent random group elements, unrelated to $g^a,g^b,g^c$.

Thus, any algorithm $\mathcal{A}$ distinguishing the two must be exploiting this structural correlation: in the real library, there exist exponents $a,b,c$ such that the exponents of the last two elements equal the pairwise products $ab$ and $ac$; in the ideal library, no such relation holds.

## 2. Building a DDH solver $\mathcal{B}$ using $\mathcal{A}$

Now $\mathcal{B}$ is given a standard DDH instance
$$
(g, g^x, g^y, g^z),
$$
which is either DH (i.e. $z = xy$) or random ($z$ independent). $\mathcal{B}$ wants to decide which.

Idea: $\mathcal{B}$ will simulate for $\mathcal{A}$ either $L_{\text{ddhp-real}}$ or $L_{\text{ddhp-ideal}}$ depending on whether $(g,g^x,g^y,g^z)$ comes from $L_{\text{ddh-real}}$ or $L_{\text{ddh-ideal}}$.

Simulation by $\mathcal{B}$:

1. Receive $(g, X, Y, Z) = (g,g^x,g^y,g^z)$ from the DDH challenger.
2. Locally choose a fresh random exponent $c \leftarrow \mathbb{Z}_q$ and compute $C = g^c$.
3. Define a 6-tuple to give to $\mathcal{A}$ as the output of `GetInstance()`:
    $$
    (g, X, Y, C, Z, W)
    $$
    where $W := Z^c$.

Interpretation:

- Map $a \mapsto x$, $b \mapsto y$, $c \mapsto c$.
- In the **real DDH case** ($Z = g^{xy}$):
   - $Z = g^{xy}$ plays the role of $g^{ab}$.
   - $W = Z^c = g^{xyc}$ is consistent with $g^{ac}$ up to relabelling (or, more simply, we could set $W = X^c = g^{xc} = g^{ac}$ and use $Z$ only as $g^{ab}$).
- In the **ideal DDH case** ($Z = g^z$ with random $z$):
   - Both $Z$ and $W = Z^c = g^{zc}$ are independent of $X,Y,C$.

More cleanly, we can simulate exactly the ddhp structure as follows:

- Set
   $$
   (g, g^a, g^b, g^c, g^{ab}, g^{ac}) := (g, X, Y, C, Z, X^c).
   $$

Then:

- If $(g,X,Y,Z)$ is from $L_{\text{ddh-real}}$ (DH case), i.e. $Z = g^{xy}$, we have
   $$
   (g, X, Y, C, Z, X^c) = (g, g^x, g^y, g^c, g^{xy}, g^{xc})
   $$
   which is **distributed exactly** as $L_{\text{ddhp-real}}$ where $a=x$, $b=y$, $c$ is as chosen.

- If $(g,X,Y,Z)$ is from $L_{\text{ddh-ideal}}$ (random case), i.e. $Z = g^z$ independent, then
   $$
   (g, X, Y, C, Z, X^c) = (g, g^x, g^y, g^c, g^z, g^{xc})
   $$
   and $g^z$ is independent of $(g^x,g^y,g^c)$, so the 5th component is random as in $L_{\text{ddhp-ideal}}$, while $X^c$ is just another random-looking group element independent of $Z$.

Thus $\mathcal{B}$ perfectly simulates $L_{\text{ddhp-real}}$ when given a DH instance, and $L_{\text{ddhp-ideal}}$ when given a random instance.

## 3. Using $\mathcal{A}$’s output

$\mathcal{B}$ now simply runs $\mathcal{A}$ on the simulated 6-tuple and outputs whatever $\mathcal{A}$ outputs:

- If $\mathcal{A}$ says "real" (i.e. believes it talks to $L_{\text{ddhp-real}}$), then $\mathcal{B}$ outputs "DH".
- If $\mathcal{A}$ says "ideal", $\mathcal{B}$ outputs "random".

If $\mathcal{A}$ distinguishes $L_{\text{ddhp-real}}$ vs $L_{\text{ddhp-ideal}}$ correctly with probability $\alpha$, then $\mathcal{B}$ distinguishes $L_{\text{ddh-real}}$ vs $L_{\text{ddh-ideal}}$ with the same success probability $\alpha$.

Thus any advantage against DDH′ propagates directly to an advantage against standard DDH.

---

# Exercise 5 – The Pedersen commitment

We work in a group $\langle g \rangle \subseteq \mathbb{Z}_p^*$ of prime order $q$. Public parameters: $(p,q,g,h)$ with $h = g^a$ for some unknown $a \in \mathbb{Z}_q$.

## 1. First attempt $c = g^m$ is not hiding

The insecure scheme:

- Commit: on $m$, output $c = g^m$, $d = \bot$.
- Open: accept if $c = g^m$.

This is **not hiding** because given $c$ anyone can simply compute the discrete logarithm base $g$ (at least conceptually):

- The mapping $m \mapsto g^m$ is **deterministic and injective** on $\mathbb{Z}_q$.
- If an adversary is able to compute discrete logs (or even just enumerate over a small message space), they can recover $m$ from $c$.

Thus, given two candidate messages $m_0,m_1$, the adversary can compute $g^{m_0},g^{m_1}$ and compare to $c$; they learn exactly which one was committed.

Therefore the scheme is not hiding.

## 2. Pedersen commitment is binding

Pedersen scheme:

- Commit: on $m \in \mathbb{Z}_q$ pick random $r \in \mathbb{Z}_q$ and output
   $$
   c = g^m h^r, \quad d = r.
   $$
- Open: accept $(m,r)$ for $c$ if $c = g^m h^r$.

Assume **no one** knows $a$ with $h=g^a$ (the discrete log of $h$ base $g$). We show binding: if a sender can find two different openings for the same $c$, we can recover $a$.

Suppose there exists an algorithm that outputs $m,m',r,r',c$ with $m \ne m'$ and
$$
g^m h^r \equiv g^{m'} h^{r'} \pmod p.
$$

Rewrite:
$$
g^m h^r = g^{m'} h^{r'} \iff g^{m-m'} = h^{r'-r}.
$$
Since $h = g^a$, we also have
$$
h^{r'-r} = (g^a)^{r'-r} = g^{a(r'-r)}.
$$
Thus
$$
g^{m-m'} = g^{a(r'-r)} \implies m-m' \equiv a(r'-r) \pmod q.
$$
If $r' \ne r$ (which must hold when $m \ne m'$ in a prime-order group), then $r'-r$ is invertible modulo $q$, so
$$
a \equiv (m-m') (r'-r)^{-1} \pmod q.
$$

Therefore, from a single binding violation we can compute $a$, i.e. the discrete logarithm of $h$ to base $g$. By assumption this is hard, so such violations are computationally infeasible; hence the scheme is *computationally binding*.

## 3. Pedersen commitment is hiding

To show hiding, fix a message $m$ and consider the distribution of commitments
$$
c = g^m h^r = g^m g^{ar} = g^{m + ar}, \quad r \leftarrow \mathbb{Z}_q.
$$

As $r$ ranges uniformly over $\mathbb{Z}_q$, the exponent $m + ar$ also ranges uniformly over $\mathbb{Z}_q$, because $a \in \mathbb{Z}_q^*$ and multiplication by $a$ is a permutation of $\mathbb{Z}_q$.

Thus for **any** fixed $m$, the distribution of $c$ is uniform over $\langle g \rangle$.

Given two messages $m_0,m_1$, the distributions
$$
c_0 = g^{m_0} h^{r_0}, \quad c_1 = g^{m_1} h^{r_1}
$$
with uniform $r_0,r_1$ are both uniform over the same group and hence **identical**. An adversary seeing a single commitment $c$ cannot distinguish whether it was generated from $m_0$ or $m_1$.

Equivalently, for any $c = g^m h^r$ and any other message $m'$, we can write
$$
c = g^{m'} h^{r'}
$$
for a suitable $r'$:
$$
g^m h^r = g^{m'} h^{r'} \iff g^{m-m'} = h^{r'-r} = g^{a(r'-r)},
$$
which has a solution $r'$ for any $m'$. This shows that every commitment can be viewed as a commitment to any message $m'$ with an appropriate randomness $r'$, so the commitment value $c$ alone carries no information about which $m$ was actually used.

Therefore Pedersen commitments are perfectly hiding and computationally binding under the discrete-log assumption.

