
These practice problems have the purpose of helping you understand the material better and  
learning the skills that are necessary to analyze cryptographic constructions, and sometimes to  
prepare you for the next class. All answers should be supported by a written justification. To  
gauge whether a justification is sufficient, ask yourself if your peers would be convinced by it  
without additional explanations.

As in the lecture, we write  
$k \leftarrow K$  
if $k$ is sampled from the set $K$ such that it can be each element from $K$ with equal probability  
$1/|K|$.

For two strings $x, y$ we use $x|y$ to denote the string obtained from concatenating $x$ with $y$.

---

## Exercise 1 – Mix and Match for ECB-MAC′

  

We have a secure MAC scheme

$$\Theta = (\text{Keygen}, \text{MAC}),\qquad

\text{MAC} : \{0,1\}^{\lambda} \times \{0,1\}^{\lambda} \to \{0,1\}^{\lambda}. $$

  

The modified ECB-MAC′ works on two blocks $m_1,m_2 \in \{0,1\}^{\lambda-1}$:

  

### Keygen

1. $k \leftarrow \Theta.\text{Keygen}()$

  

### ECB–MAC′

Given $m_1|m_2$:

  

1. $t_1 \leftarrow \Theta.\text{MAC}(k, 0|m_1)$

2. $t_2 \leftarrow \Theta.\text{MAC}(k, 1|m_2)$

3. Output $(t_1, t_2)$.

  

---

  

### 1. Construct $m$ such that $\text{ECB–MAC′}(k,m) = (t_1,t_2')$

  

We are given:

  

- $(t_1,t_2)$ from $m = 0^{2\lambda-2}$

- $(t_1',t_2')$ from $m' = 1^{2\lambda-2}$

  

Break into blocks:

  

For $0^{2\lambda-2}$:

  

- $m_1 = 0^{\lambda-1}$

- $m_2 = 0^{\lambda-1}$

  

Thus:

$$

t_1 = \Theta.\text{MAC}(k,0|0^{\lambda-1}), \qquad

t_2 = \Theta.\text{MAC}(k,1|0^{\lambda-1}).

$$

  

For $1^{2\lambda-2}$:

  

- $m_1' = 1^{\lambda-1}$

- $m_2' = 1^{\lambda-1}$

  

Thus:

$$

t_1' = \Theta.\text{MAC}(k,0|1^{\lambda-1}),\qquad

t_2' = \Theta.\text{MAC}(k,1|1^{\lambda-1}).

$$

  

We want a message whose MAC output is $(t_1, t_2')$.

  

So choose:

- $m_1^* = 0^{\lambda-1}$ so that first tag is $t_1$

- $m_2^* = 1^{\lambda-1}$ so that second tag is $t_2'$

  

Thus the required message is:

  

$$

m = 0^{\lambda-1} \mid 1^{\lambda-1}.

$$

  

---

  

### 2. Distinguish MAC-real from MAC-fake

We construct adversary $\mathcal{A}$:

1. Query the MAC oracle on $m_0 := 0^{2\lambda-2}$ and obtain $(t_1,t_2) := \mathsf{Gettag}(m_0)$.
2. Query the MAC oracle on $m_1 := 1^{2\lambda-2}$ and obtain $(t_1',t_2') := \mathsf{Gettag}(m_1)$.
3. Define $m^* := 0^{\lambda-1} \mid 1^{\lambda-1}$ and $t^* := (t_1,t_2')$.
4. Let $b := \mathsf{Checktag}(m^*, t^*)$.
5. If $b = 1$ output "real", otherwise output "fake".


  

**In the MAC-real library:**

  

- $(t_1,t_2')$ is a valid tag for $m^*$, so `Checktag` returns **1**.

  

**In the MAC-fake library:**

  

- $(m^*, t^*)$ was never produced by `Gettag`, so `Checktag` returns **0**.

  

Thus the distinguisher succeeds with probability 1.

  

ECB–MAC′ is therefore **not a secure MAC**.

  

---

  

## Exercise 2 – CBC-MAC on Variable-Length Messages

  

CBC–MAC:

  

1. $t_0 = 0^\lambda$

2. For $i=1.. \ell$:

$$

t_i = \Theta.\text{MAC}(k, t_{i-1} \oplus m_i)

$$

3. Output $t_\ell$

  

Let

  

$$

t = \text{CBC–MAC}(k, m_1|m_2).

$$

  

---

  

### 1. Compute MAC of $m_1 | m_2 | (m_1 \oplus t) | m_2$

  

Compute:

  

- $t_1 = \Theta.\text{MAC}(k, m_1)$

- $t_2 = \Theta.\text{MAC}(k, t_1 \oplus m_2) = t$

  

Now the 3rd block:

  

$$

t_3 = \Theta.\text{MAC}(k, t_2 \oplus (m_1 \oplus t))

= \Theta.\text{MAC}(k, m_1)

= t_1

$$

  

(Using $t \oplus t = 0$.)

  

4th block:

  

$$

t_4 = \Theta.\text{MAC}(k, t_3 \oplus m_2)

= \Theta.\text{MAC}(k, t_1 \oplus m_2)

= t_2

= t.

$$

  

Thus:

  

$$

\text{CBC–MAC}(k, m_1|m_2|(m_1 \oplus t)|m_2) = t.

$$

  

We have extended the message but kept the same tag.

  

---

  

### 2. Distinguish MAC-real from MAC-fake for CBC-MAC

Adversary $\mathcal{A}$:

1. Pick arbitrary $m_1, m_2 \in \{0,1\}^\lambda$ and set $m := m_1 \mid m_2$.
2. Query the MAC oracle to get $t := \mathsf{Gettag}(m)$.
3. Construct $m' := m_1 \mid m_2 \mid (m_1 \oplus t) \mid m_2$.
4. Let $b := \mathsf{Checktag}(m', t)$.
5. If $b = 1$ output "real", otherwise output "fake".


  

**In MAC-real (CBC-MAC with fixed key):**

  

We showed above:

$$

\text{CBC–MAC}(k, m_1|m_2|(m_1 \oplus t)|m_2) = t.

$$

So `Checktag(m',t)` returns 1.

  

**In MAC-fake:**

  

- Only $(m,t)$ was returned by `Gettag`.

- $(m',t)$ was never returned → `Checktag` returns 0.

  

So this adversary also wins with probability 1, showing that CBC-MAC used on variable-length messages (without length-fixing) is **not a secure MAC**.

  

---

  

## Exercise 3 – Encrypt-then-MAC is CCA-Secure

  

We have:

  

- $\Omega = (\text{Keygen}, \text{Enc}, \text{Dec})$ is a **CPA-secure** encryption scheme.

- $\Theta = (\text{Keygen}, \text{MAC})$ is a **secure MAC scheme**.

- Ciphertexts of $\Omega$ lie in the message space of $\Theta$, i.e. $\Omega.C \subseteq \Theta.M$.

  

The Encrypt-then-MAC scheme $\Sigma$:

  

### $\Sigma.\text{Keygen}()$

  

1. $k_E \leftarrow \Omega.\text{Keygen}()$

2. $k_M \leftarrow \Theta.\text{Keygen}()$

3. Output $(k_E, k_M)$.

  

### $\Sigma.\text{Enc}((k_E,k_M), m)$

  

1. $c \leftarrow \Omega.\text{Enc}(k_E, m)$

2. $t \leftarrow \Theta.\text{MAC}(k_M, c)$

3. Output $(c,t)$.

  

### $\Sigma.\text{Dec}((k_E,k_M), (c,t))$

  

1. If $t \ne \Theta.\text{MAC}(k_M, c)$ then output $\bot$

2. Else output $\Omega.\text{Dec}(k_E, c)$.

  

We must show that $\Sigma$ is IND-CCA-secure.

  

---

  

### 1. Correctness of $\Sigma$

  

Assume $\Omega$ is correct:

$$

\forall k_E,m:\quad \Omega.\text{Dec}(k_E, \Omega.\text{Enc}(k_E,m)) = m.

$$

  

For $\Sigma$:

  

- Encrypt: $(c,t) = \Sigma.\text{Enc}((k_E,k_M),m)$ with

- $c = \Omega.\text{Enc}(k_E, m)$

- $t = \Theta.\text{MAC}(k_M, c)$

- Decrypt: $\Sigma.\text{Dec}((k_E,k_M),(c,t))$:

- MAC check passes since $t$ is valid

- Returns $\Omega.\text{Dec}(k_E, c) = m$

  

So $\Sigma$ is a correct encryption scheme.

  

---

  

### 2. Expand $L_{\Sigma}^{\text{CCA−0}}$ using $\Omega$ and $\Theta$

  

$L_{\Sigma}^{\text{CCA−0}}$:

  

1. $(k_E,k_M) \leftarrow \Sigma.\text{Keygen}()$

2. $S \leftarrow \emptyset$

  

**`CTXT(m_0,m_1)`**:

  

1. If $|m_0| \ne |m_1|$ then return $\bot$

2. $(c,t) \leftarrow \Sigma.\text{Enc}((k_E,k_M), m_0)$ (since bit $b=0$)

3. $S \leftarrow S \cup \{(c,t)\}$

4. Return $(c,t)$

  

**`Decrypt((c,t))`**:

  

1. If $(c,t) \in S$ then return $\bot$

2. Return $\Sigma.\text{Dec}((k_E,k_M),(c,t))$

  

Inlining $\Sigma$:

  

- Keygen:

  

- $k_E \leftarrow \Omega.\text{Keygen}()$

- $k_M \leftarrow \Theta.\text{Keygen}()$

  

- CTXT:

  

1. If $|m_0| \ne |m_1|$ then $\bot$

2. $c \leftarrow \Omega.\text{Enc}(k_E, m_0)$

3. $t \leftarrow \Theta.\text{MAC}(k_M, c)$

4. $S \leftarrow S \cup \{(c,t)\}$

5. Return $(c,t)$

  

- Decrypt:

  

1. If $(c,t) \in S$ then $\bot$

2. If $t \ne \Theta.\text{MAC}(k_M, c)$ then $\bot$

3. Else output $\Omega.\text{Dec}(k_E, c)$

  

Call this library $L_{\text{CCA-0-inline}}$.

  

---

  

### 3. Introduce $L_{\Theta}^{\text{MAC-real}}$: define $L_{\text{Step−1}}$

  

We now abstract $\Theta.\text{MAC}$ into an oracle from the MAC-real library:

  

- `Gettag(m)` returns $\Theta.\text{MAC}(k_M, m)$

- `Checktag(m,t)` returns $1$ iff $t=\Theta.\text{MAC}(k_M,m)$

  

**$L_{\text{Step−1}}$** (which will use $L_{\Theta}^{\text{MAC-real}}$):

  

1. $k_E \leftarrow \Omega.\text{Keygen}()$

2. $S \leftarrow \emptyset$

  

`CTXT(m_0,m_1)`:

  

1. If $|m_0| \ne |m_1|$ then $\bot$

2. $c \leftarrow \Omega.\text{Enc}(k_E, m_0)$

3. $t \leftarrow \textsf{Gettag}(c)$ (MAC-real)

4. $S \leftarrow S \cup \{(c,t)\}$

5. Return $(c,t)$

  

`Decrypt((c,t))`:

  

1. If $(c,t) \in S$ then $\bot$

2. If $\textsf{Checktag}(c,t)=0$ then $\bot$

3. Else output $\Omega.\text{Dec}(k_E, c)$

  

From the adversary’s perspective:

  

$$

L_{\Sigma}^{\text{CCA−0}} \equiv L_{\text{Step−1}} \circ L_{\Theta}^{\text{MAC-real}}.

$$

  

---

  

### 4. Replace $L_{\Theta}^{\text{MAC-real}}$ by $L_{\Theta}^{\text{MAC-fake}}$

  

By MAC security of $\Theta$:

  

$$

L_{\Theta}^{\text{MAC-real}} \approx L_{\Theta}^{\text{MAC-fake}}.

$$

  

Hence, plugging either into $L_{\text{Step−1}}$ gives indistinguishable behavior:

  

$$

L_{\text{Step−1}} \circ L_{\Theta}^{\text{MAC-real}}

\approx

L_{\text{Step−1}} \circ L_{\Theta}^{\text{MAC-fake}}.

$$

  

So we may safely switch to MAC-fake.

  

---

  

### 5. Inline $L_{\Theta}^{\text{MAC-fake}}$: $L_{\text{Step−2}}$

  

Internally, MAC-fake maintains a table $T$:

  

- On `Gettag(m)`:

- If $m \in T$, return stored $t$

- Else sample random $t$, store $T[m]=t$, return $t$

- On `Checktag(m,t)`:

- Return 1 iff $(m,t)$ is exactly in $T$

  

We inline this behavior:

  

**$L_{\text{Step−2}}$**

  

1. $k_E \leftarrow \Omega.\text{Keygen}()$

2. $S \leftarrow \emptyset$

3. $T \leftarrow \emptyset$ (for MAC-fake)

  

`CTXT(m_0,m_1)`:

  

1. If $|m_0| \ne |m_1|$ then $\bot$

2. $c \leftarrow \Omega.\text{Enc}(k_E, m_0)$

3. If $c \in \text{dom}(T)$ then $t := T[c]$

else sample $t \leftarrow \{0,1\}^\lambda$ and set $T[c] := t$

4. $S \leftarrow S \cup \{(c,t)\}$

5. Return $(c,t)$

  

`Decrypt((c,t))`:

  

1. If $(c,t) \in S$ then $\bot$

2. If $c \not\in \text{dom}(T)$ or $T[c] \ne t$ then $\bot$

3. Else output $\Omega.\text{Dec}(k_E, c)$

  

This is exactly the same as using $L_{\text{Step−1}}$ with MAC-fake, only with the MAC logic inlined.

  

So:

$$

L_{\text{Step−1}} \circ L_{\Theta}^{\text{MAC-fake}}

\equiv

L_{\text{Step−2}}.

$$

  

---

  

### 6. Show that decryption never reaches $\Omega.\text{Dec}$: define $L_{\text{Step−3}}$

  

Observe in $L_{\text{Step−2}}$:

  

- For any ciphertext $c$ that appears in a CTXT call:

- $T[c]$ is defined and stores *exactly* the tag $t$ paired with $c$ in that CTXT output.

- The pair $(c,t)$ is stored in $S$.

  

Consider a `Decrypt((c,t))` query:

  

1. If $(c,t) \in S$, we return $\bot$ immediately.

2. Suppose $(c,t) \not\in S$:

- If $T[c]$ undefined or $T[c] \ne t$, then MAC invalid ⇒ return $\bot$.

- If $T[c] = t$, then $(c,t)$ **must** be some challenge ciphertext previously returned by CTXT, hence in $S$. But we assumed $(c,t)\notin S$, contradiction.

  

Therefore, in fact, case “$T[c]=t$ with $(c,t)\notin S$” never happens. Decrypt *always* returns $\bot$ without calling $\Omega.\text{Dec}$.

  

We can remove the decryption call and define $L_{\text{Step−3}}$:

  

**$L_{\text{Step−3}}$**

  

- Same as $L_{\text{Step−2}}$, except `Decrypt((c,t))` **always returns $\bot$**.

  

Then:

$$

L_{\text{Step−2}} \equiv L_{\text{Step−3}}.

$$

  

---

  

### 7. Factor out $\Omega$’s CPA-0 oracle: define $L_{\text{Step−4}}$

  

Now the only remaining use of $\Omega$ is encryption in CTXT:

  

$$

c \leftarrow \Omega.\text{Enc}(k_E, m_0).

$$

  

We replace this with a LR oracle from $L_{\Omega}^{\text{CPA−0}}$:

  

- $L_{\Omega}^{\text{CPA−0}}$ chooses $k_E$,

- On input $(m_0,m_1)$, returns $\Omega.\text{Enc}(k_E, m_0)$.

  

**$L_{\text{Step−4}}$** will use that:

  

1. $S \leftarrow \emptyset$

2. $T \leftarrow \emptyset$

  

`CTXT(m_0,m_1)`:

  

1. If $|m_0| \ne |m_1|$ then $\bot$

2. $c \leftarrow \textsf{CTXT}_\Omega(m_0,m_1)$ (LR oracle with bit $b=0$)

3. If $c \in \text{dom}(T)$ then $t := T[c]$

else sample random $t$ and set $T[c]:=t$

4. $S \leftarrow S \cup \{(c,t)\}$

5. Return $(c,t)$

  

`Decrypt((c,t))`:

  

- Always return $\bot$.

  

Hence:

$$

L_{\text{Step−3}} \equiv L_{\text{Step−4}} \circ L_{\Omega}^{\text{CPA−0}}.

$$

  

---

  

### 8. Use CPA security of $\Omega$ and conclude CCA security of $\Sigma$

  

By CPA-security of $\Omega$:

$$

L_{\Omega}^{\text{CPA−0}} \approx L_{\Omega}^{\text{CPA−1}}.

$$

  

Thus:

$$

L_{\text{Step−4}} \circ L_{\Omega}^{\text{CPA−0}}

\approx

L_{\text{Step−4}} \circ L_{\Omega}^{\text{CPA−1}}.

$$

  

Tracing back all transformations:

  

- $L_{\text{Step−4}} \circ L_{\Omega}^{\text{CPA−0}} \equiv L_{\text{Step−3}} \equiv L_{\text{Step−2}} \equiv L_{\text{Step−1}} \circ L_{\Theta}^{\text{MAC-fake}}$

- By MAC security, $L_{\text{Step−1}} \circ L_{\Theta}^{\text{MAC-fake}} \approx L_{\text{Step−1}} \circ L_{\Theta}^{\text{MAC-real}}$

- And $L_{\text{Step−1}} \circ L_{\Theta}^{\text{MAC-real}} \equiv L_{\Sigma}^{\text{CCA−0}}$

  

Similarly, with CPA-1 for $\Omega$, we get $L_{\Sigma}^{\text{CCA−1}}$.

  

Therefore:

  

$$

L_{\Sigma}^{\text{CCA−0}} \approx L_{\Sigma}^{\text{CCA−1}},

$$

  

which shows that Encrypt-then-MAC $\Sigma$ is **IND-CCA secure**.

