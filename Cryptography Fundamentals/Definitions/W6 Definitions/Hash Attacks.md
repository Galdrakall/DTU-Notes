
This sheet compares the **major cryptographic attacks on hash functions**, what security property they break, and how to defend against them.

Use this for:
- Fast exam revision
- Attack identification questions
- Secure construction design

---

## 1. Quick Overview Table

| Attack | Breaks Which Property | Main Idea | Practical Impact | Defense |
|--------|------------------------|-----------|-------------------|---------|
| Preimage Attack | Preimage resistance | Invert hash | Rare in practice for secure hashes | Large output size |
| Second Preimage Attack | 2nd preimage resistance | Find different input with same hash | Digital signature forgeries | Strong hash design |
| Collision Attack | Collision resistance | Find any $m \neq m'$ with same hash | Certificate, signature forgery | Use SHA-256+ |
| Birthday Attack | Collision resistance | Probabilistic collision search | Theoretical + practical | Large output |
| Length-Extension Attack | MAC security, integrity | Extend hash without key | Breaks naive MACs | Use HMAC |
| Rainbow Table Attack | Password security | Precomputation | Mass password cracking | Use salt |
| Dictionary Attack | Password security | Guess common passwords | Fast offline cracking | Salt + slow hash |

---

## 2. Preimage Attack

### Goal
Given:
$$
y = H(m)
$$
find **any** $m$ such that:
$$
H(m) = y
$$

### Complexity
For an $n$-bit hash:
$$
O(2^n)
$$

### What It Breaks
- One-wayness
- Preimage resistance

### Practical Relevance
- Infeasible for modern hashes (SHA-256, SHA-3)
- Relevant only if hash output is too short

### Defense
- Use large output sizes (256 bits or more)

---

## 3. Second Preimage Attack

### Goal
Given a specific message $m$, find:
$$
m' \neq m \quad \text{such that} \quad H(m') = H(m)
$$

### Complexity
For ideal $n$-bit hash:
$$
O(2^n)
$$

### What It Breaks
- Second preimage resistance

### Practical Relevance
- Attacks against signed documents
- Attacker replaces signed file with different one of same hash

### Defense
- Use strong collision-resistant hash functions

---

## 4. Collision Attack

### Goal
Find **any** two distinct messages:
$$
m \neq m' \quad \text{such that} \quad H(m) = H(m')
$$

### Complexity (Birthday Bound)
For $n$-bit output:
$$
O(2^{n/2})
$$

### What It Breaks
- Collision resistance

### Practical Relevance
- MD5 and SHA-1 are broken via collision attacks
- Enables:
  - Forged digital certificates
  - Forged software signatures

### Defense
- Use SHA-256, SHA-3, or better

---

## 5. Birthday Attack (Special Case of Collision Attack)

### Idea
Randomly hash many messages until a collision appears.

Expected number of samples:
$$
2^{n/2}
$$

### Example
For a 128-bit output:
$$
2^{64} \text{ operations}
$$

### Why This Matters
This sets a **fundamental upper bound** on collision security.

No hash with $n$-bit output can be stronger than $2^{n/2}$ against collisions.

---

## 6. Length-Extension Attack

### Target Construction
Naive MAC:
$$
\text{MAC}_k(m) = H(k \parallel m)
$$

### Attacker Given
- $H(k \parallel m)$
- Length $|m|$
- Chosen extension $m'$

### Attacker Can Compute
$$
H(k \parallel m \parallel m')
$$
**without knowing $k$**

### What It Breaks
- Integrity of naive hash-based MACs
- Authentication

### Affects
- Merkle–Damgård hashes:
  - MD5
  - SHA-1
  - SHA-256 / SHA-512

### Defense
- Use HMAC:
$$
\text{HMAC}_k(m)
$$
- Never use $H(k \parallel m)$ as a MAC

---

## 7. Rainbow Table Attack

### Target
Unsalted password hashes:
$$
H(p)
$$

### Idea
- Precompute:
  - Many passwords
  - Their hashes
- Store in large lookup tables
- Instantly invert hashes when database is leaked

### What It Breaks
- Password confidentiality

### Practical Impact
- Massive real-world breaches
- Millions of passwords cracked in seconds

### Defense
Use a **salt**:
$$
H(s \parallel p)
$$
Unique $s$ per user defeats precomputation.

---

## 8. Dictionary Attack

### Target
Human-chosen passwords with low entropy

### Idea
Try common passwords:
$$
p \in \{\text{"123456"}, \text{"password"}, \text{"qwerty"}\}
$$
until:
$$
H(p) \text{ matches stored hash}
$$

### What It Breaks
- Password security (offline)

### Defense
- Long, high-entropy passwords
- Slow password hashing (e.g., bcrypt, scrypt, Argon2)
- Always use salts

---

## 9. Fast Hash Attacks on Passwords

### Problem
Fast hashes like:
- SHA-256
- SHA-1

allow attackers to test:
- Billions of guesses per second with GPUs

### Defense
Use **slow, memory-hard** functions:
- bcrypt
- scrypt
- Argon2

These deliberately increase:
- Computation time
- Memory usage

---

## 10. Broken Hash Function Attacks

### MD5
- Practical collision attacks
- Forged certificates
- Completely broken

### SHA-1
- Practical collision attacks (SHAttered)
- Officially deprecated

### Defense
- Never use MD5 or SHA-1
- Use SHA-256, SHA-3, or better

---

## 11. Which Attack Breaks Which Property

| Property | Attacks That Break It |
|----------|------------------------|
| Preimage resistance | Preimage attack |
| Second preimage resistance | Second preimage attack |
| Collision resistance | Collision, birthday |
| MAC security | Length-extension |
| Password confidentiality | Dictionary, rainbow tables |

---

## 12. Hash Attack vs Use Case

| Use Case | Main Threat | Correct Defense |
|----------|-------------|------------------|
| Digital signatures | Collision | SHA-256+ |
| Certificates | Collision | Strong hash |
| Password storage | Dictionary + rainbow | Salt + slow hash |
| MAC construction | Length extension | HMAC |
| File integrity | Second preimage | Strong hash |

---

## 13. Common Exam Traps

- “Collision resistance prevents preimage attacks.” → False  
- “Salting prevents dictionary attacks.” → False (it only prevents precomputation)  
- “SHA-256 is good for passwords.” → False (too fast)  
- “Length extension breaks encryption.” → False (it breaks naive MACs)  
- “If collisions exist, hash is insecure.” → False (they always exist; must be hard to find)

---

## 14. Final Memory Hooks

- **Preimage** → invert the hash  
- **Second preimage** → replace a specific message  
- **Collision** → find any matching pair  
- **Birthday** → $2^{n/2}$ rule  
- **Length extension** → break $H(k \parallel m)$  
- **Rainbow tables** → precomputation  
- **Dictionary attacks** → human weakness  

---
