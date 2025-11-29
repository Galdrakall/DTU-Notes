
This example shows **every step** of ElGamal:

- Parameter setup
- Key generation
- Encryption
- Decryption

Numbers are tiny so you can recompute by hand. This is **not secure**, just for learning.

---

## 1. Public Parameters

We work in the multiplicative group modulo a prime $p$.

Choose:

- Prime: $p = 23$
- Generator: $g = 5$

So the group is $\mathbb{Z}_{23}^* = \{1, 2, \dots, 22\}$, and $g = 5$ generates (most of) it.

Everyone knows: **$(p, g) = (23, 5)$**

---

## 2. Key Generation (for the receiver)

The receiver (call them Bob) generates a **secret key** and a **public key**.

1. Choose a **secret exponent** $x$:

   Let
   $$
   x = 6
   $$

2. Compute the **public key**:
   $$
   h = g^x \bmod p = 5^6 \bmod 23
   $$

   Compute $5^6 \bmod 23$ step by step:

   - $5^1 \equiv 5 \pmod{23}$
   - $5^2 = 25 \equiv 2 \pmod{23}$ (since $25 - 23 = 2$)
   - $5^3 = 5^2 \cdot 5 \equiv 2 \cdot 5 = 10 \pmod{23}$
   - $5^4 = 5^3 \cdot 5 \equiv 10 \cdot 5 = 50 \equiv 50 - 46 = 4 \pmod{23}$
   - $5^5 = 5^4 \cdot 5 \equiv 4 \cdot 5 = 20 \pmod{23}$
   - $5^6 = 5^5 \cdot 5 \equiv 20 \cdot 5 = 100 \equiv 100 - 92 = 8 \pmod{23}$

   So:
   $$
   h = 8
   $$

**Bob’s keys**

- Secret key: $x = 6$
- Public key: $(p, g, h) = (23, 5, 8)$

---

## 3. Message to Encrypt

Suppose Alice wants to send the message

- $m = 10$

We require $m \in \mathbb{Z}_{23}^*$, so $1 \le m \le 22$. Here $m = 10$ is fine.

---

## 4. Encryption (Alice → Bob)

To encrypt $m$ using Bob’s public key $(p, g, h)$, Alice:

1. Chooses a fresh **random** $r$:
   $$
   r = 15
   $$

   (In practice this must be uniformly random and never reused.)

2. Computes the first ciphertext component:
   $$
   c_1 = g^r \bmod p = 5^{15} \bmod 23
   $$

   Compute $5^{15} \bmod 23$ step by step (iterative multiplication):

   - $5^1 \equiv 5$
   - $5^2 \equiv 5 \cdot 5 = 25 \equiv 2$
   - $5^3 \equiv 2 \cdot 5 = 10$
   - $5^4 \equiv 10 \cdot 5 = 50 \equiv 4$
   - $5^5 \equiv 4 \cdot 5 = 20$
   - $5^6 \equiv 20 \cdot 5 = 100 \equiv 8$
   - $5^7 \equiv 8 \cdot 5 = 40 \equiv 17$
   - $5^8 \equiv 17 \cdot 5 = 85 \equiv 16$
   - $5^9 \equiv 16 \cdot 5 = 80 \equiv 11$
   - $5^{10} \equiv 11 \cdot 5 = 55 \equiv 9$
   - $5^{11} \equiv 9 \cdot 5 = 45 \equiv 22$
   - $5^{12} \equiv 22 \cdot 5 = 110 \equiv 18$
   - $5^{13} \equiv 18 \cdot 5 = 90 \equiv 21$
   - $5^{14} \equiv 21 \cdot 5 = 105 \equiv 13$
   - $5^{15} \equiv 13 \cdot 5 = 65 \equiv 19$

   So:
   $$
   c_1 = 19
   $$

3. Computes the shared “masking” factor using the public key $h$:
   $$
   h^r = 8^{15} \bmod 23
   $$

   Compute powers of $8$ modulo $23$:

   - $8^1 \equiv 8$
   - $8^2 \equiv 8 \cdot 8 = 64 \equiv 18$  (since $64 - 46 = 18$)
   - $8^3 \equiv 18 \cdot 8 = 144 \equiv 6$  (since $144 - 138 = 6$)
   - $8^4 \equiv 6 \cdot 8 = 48 \equiv 2$
   - $8^5 \equiv 2 \cdot 8 = 16$
   - $8^6 \equiv 16 \cdot 8 = 128 \equiv 13$ (since $128 - 115 = 13$)
   - $8^7 \equiv 13 \cdot 8 = 104 \equiv 12$ (since $104 - 92 = 12$)
   - $8^8 \equiv 12 \cdot 8 = 96 \equiv 4$
   - $8^9 \equiv 4 \cdot 8 = 32 \equiv 9$
   - $8^{10} \equiv 9 \cdot 8 = 72 \equiv 3$
   - $8^{11} \equiv 3 \cdot 8 = 24 \equiv 1$
   - $8^{12} \equiv 1 \cdot 8 = 8$
   - $8^{13} \equiv 8^1 \cdot 8^{12} \equiv 8 \cdot 8 = 64 \equiv 18$
   - $8^{14} \equiv 18 \cdot 8 = 144 \equiv 6$
   - $8^{15} \equiv 6 \cdot 8 = 48 \equiv 2$

   So:
   $$
   h^r \equiv 2 \pmod{23}
   $$

4. Computes the second ciphertext component:
   $$
   c_2 = m \cdot h^r \bmod p = 10 \cdot 2 \bmod 23 = 20
   $$

So the final **ciphertext** is:
$$
c = (c_1, c_2) = (19, 20)
$$

Alice sends $(19, 20)$ to Bob.

---

## 5. Decryption (Bob)

Bob knows:

- Secret key: $x = 6$
- Ciphertext: $(c_1, c_2) = (19, 20)$

To decrypt he:

1. Recomputes the shared secret factor using his secret exponent $x$:
   $$
   s = c_1^x \bmod p = 19^6 \bmod 23
   $$

   Compute $19^6 \bmod 23$:

   - $19^1 \equiv 19$
   - $19^2 \equiv 19 \cdot 19 = 361 \equiv 16$  
     (since $23 \cdot 15 = 345$, $361 - 345 = 16$)
   - $19^3 \equiv 16 \cdot 19 = 304 \equiv 5$  
     (since $23 \cdot 13 = 299$, $304 - 299 = 5$)
   - $19^4 \equiv 5 \cdot 19 = 95 \equiv 3$  
     (since $23 \cdot 4 = 92$, $95 - 92 = 3$)
   - $19^5 \equiv 3 \cdot 19 = 57 \equiv 11$  
     (since $23 \cdot 2 = 46$, $57 - 46 = 11$)
   - $19^6 \equiv 11 \cdot 19 = 209 \equiv 2$  
     (since $23 \cdot 9 = 207$, $209 - 207 = 2$)

   So:
   $$
   s = 2
   $$

   Notice this matches $h^r$ (from encryption), as expected:
   $$
   s = c_1^x = (g^r)^x = g^{rx} = h^r
   $$

2. Computes the **inverse** of $s$ modulo $p$.

We need $s^{-1}$ such that:
$$
s \cdot s^{-1} \equiv 1 \pmod{23}
$$

Since $s = 2$, we look for $2^{-1} \bmod 23$.

Try $12$:
- $2 \cdot 12 = 24 \equiv 1 \pmod{23}$

So:
$$
s^{-1} \equiv 12 \pmod{23}
$$

3. Recover the plaintext:
   $$
   m' = c_2 \cdot s^{-1} \bmod p = 20 \cdot 12 \bmod 23
   $$

Compute:
- $20 \cdot 12 = 240$
- $23 \cdot 10 = 230$
- $240 - 230 = 10$

So:
$$
m' = 10
$$

This matches the original message $m = 10$.

---

## 6. Summary of the Example

- Public parameters: $p = 23,\ g = 5$
- Bob’s secret key: $x = 6$
- Bob’s public key: $h = g^x \bmod p = 8$

- Message: $m = 10$
- Random $r = 15$

**Encryption (Alice)**

- $c_1 = g^r \bmod p = 19$
- $h^r = 2$
- $c_2 = m \cdot h^r \bmod p = 20$
- Ciphertext: $(19, 20)$

**Decryption (Bob)**

- $s = c_1^x \bmod p = 2$
- $s^{-1} \equiv 12 \pmod{23}$
- $m' = c_2 \cdot s^{-1} \bmod p = 10$

Recovered message:
$$
m' = 10 = m
$$

---

## 7. What This Shows Conceptually

- $c_1 = g^r$ exposes the random exponent $r$ only as a group element.
- $c_2 = m \cdot h^r$ masks $m$ with the Diffie–Hellman shared value $h^r$.
- Only someone who knows $x$ can recreate $h^r$ as $c_1^x$ and undo the masking.

In other words, **ElGamal = “Diffie–Hellman + one extra multiply by the message”**.

