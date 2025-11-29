# IND-CPA vs IND-CCA vs INT-CTXT – Global Comparison Sheet

This sheet gives a **side-by-side comparison of the three most important symmetric encryption security notions**:
- IND-CPA (confidentiality)
- IND-CCA (confidentiality under active attack)
- INT-CTXT (ciphertext integrity)

Use this for:
- Fast exam revision
- Proof questions
- Construction correctness checks

---

## 1. One-Line Meaning

| Notion | What It Guarantees |
|--------|---------------------|
| IND-CPA | Attacker cannot learn anything about plaintexts, even with encryption queries |
| IND-CCA | Attacker cannot learn anything about plaintexts, even with encryption *and decryption* queries |
| INT-CTXT | Attacker cannot create a new valid ciphertext |

---

## 2. What the Attacker Is Allowed to Do

| Capability | IND-CPA | IND-CCA | INT-CTXT |
|------------|---------|----------|-----------|
| Encryption oracle | Yes | Yes | Optional |
| Decryption oracle | No | Yes (except challenge) | Yes |
| Modify ciphertexts | Yes | Yes | Yes |
| Goal | Distinguish messages | Distinguish messages | Forge valid ciphertext |

---

## 3. IND-CPA – Formal Definition

### Game
1. Challenger generates key $k$.
2. Adversary $\mathcal{A}$ gets oracle access to $\text{Enc}_k(\cdot)$.
3. $\mathcal{A}$ submits $(m_0, m_1)$ with $|m_0| = |m_1|$.
4. Challenger picks $b \in \{0,1\}$ and sends:
   $$
   c^* = \text{Enc}_k(m_b)
   $$
5. $\mathcal{A}$ outputs $b'$.

### Advantage
$$
\text{Adv}_{\mathcal{A}}^{\text{IND-CPA}}
= \left|\Pr[b' = b] - \tfrac12\right|
$$

### Security Condition
$$
\forall \mathcal{A} \in \text{PPT}, \quad 
\text{Adv}_{\mathcal{A}}^{\text{IND-CPA}} \le \text{negl}(\lambda)
$$

### Intuition
Even after choosing plaintexts and seeing their encryptions, the attacker learns nothing about which challenge message was encrypted.

---

## 4. IND-CCA – Formal Definition

### Game
Same as IND-CPA but with **extra power**:

- $\mathcal{A}$ also gets access to $\text{Dec}_k(\cdot)$
- After receiving $c^*$, $\mathcal{A}$ may query:
  $$
  \text{Dec}_k(c) \quad \text{for all } c \neq c^*
  $$

Then $\mathcal{A}$ outputs $b'$.

### Advantage
$$
\text{Adv}_{\mathcal{A}}^{\text{IND-CCA}}
= \left|\Pr[b' = b] - \tfrac12\right|
$$

### Security Condition
$$
\forall \mathcal{A} \in \text{PPT}, \quad 
\text{Adv}_{\mathcal{A}}^{\text{IND-CCA}} \le \text{negl}(\lambda)
$$

### Intuition
Even if the attacker can decrypt **almost any ciphertext they want**, they still cannot learn anything about the challenge message.

---

## 5. INT-CTXT – Formal Definition (Ciphertext Integrity)

### Game
1. Challenger generates key $k$.
2. $\mathcal{A}$ gets access to $\text{Enc}_k(\cdot)$ and $\text{Dec}_k(\cdot)$.
3. $\mathcal{A}$ outputs a ciphertext $c^*$.
4. $\mathcal{A}$ **wins** if:
   $$
   \text{Dec}_k(c^*) \neq \perp
   $$
   and $c^*$ was never previously returned by $\text{Enc}_k$.

### Advantage
$$
\text{Adv}_{\mathcal{A}}^{\text{INT-CTXT}}
= \Pr[\mathcal{A} \text{ forges a valid ciphertext}]
$$

### Security Condition
$$
\forall \mathcal{A} \in \text{PPT}, \quad 
\text{Adv}_{\mathcal{A}}^{\text{INT-CTXT}} \le \text{negl}(\lambda)
$$

### Intuition
The attacker cannot create a *new valid ciphertext*, even after seeing existing ones.

---

## 6. Relationship Between the Notions

### Strength Ordering

$$
\text{IND-CCA} \;\Rightarrow\; \text{IND-CPA}
$$

- Every IND-CCA secure scheme is automatically IND-CPA secure.
- The converse is false.

IND-CPA and INT-CTXT are **independent**:
- You can be IND-CPA but not INT-CTXT
- You can be INT-CTXT but not IND-CPA

To get full security, you need **both**.

---

## 7. Full Authenticated Encryption (AE)

A scheme is **Authenticated Encryption (AE)** if it satisfies:

- IND-CCA (confidentiality under active attack)
- INT-CTXT (ciphertext integrity)

Formally:

$$
\text{AE} = \text{IND-CCA} + \text{INT-CTXT}
$$

---

## 8. Which Constructions Satisfy What?

| Scheme | IND-CPA | IND-CCA | INT-CTXT |
|--------|----------|-----------|-----------|
| ECB | No | No | No |
| CBC (no MAC) | Yes | No | No |
| CTR (no MAC) | Yes | No | No |
| Encrypt-then-MAC | Yes | Yes | Yes |
| AES-GCM | Yes | Yes | Yes |
| ChaCha20-Poly1305 | Yes | Yes | Yes |

---

## 9. Typical Attacks per Notion

| If this fails… | Then attacker can… |
|----------------|---------------------|
| IND-CPA | Learn info about plaintext |
| IND-CCA | Learn plaintext using decryption oracle |
| INT-CTXT | Inject forged messages |

---

## 10. Canonical Example Attacks

### Breaking IND-CPA
- ECB mode
- Deterministic encryption
- Pattern leakage in images

---

### Breaking IND-CCA
- Padding oracle attacks on CBC
- Error-message based decryption leaks

---

### Breaking INT-CTXT
- Bit-flipping in CBC
- Forging CTR ciphertext blocks
- Replay of unauthenticated ciphertexts

---

## 11. Composition Theorem (Core Exam Result)

If:
- Encryption is IND-CPA secure
- MAC is unforgeable

Then:
$$
\text{Encrypt-then-MAC} \Rightarrow \text{IND-CCA + INT-CTXT}
$$

This is why:
- Encrypt-then-MAC is the **correct** composition
- MAC-then-Encrypt is generally insecure

---

## 12. Fast Exam Cheat Grid

| Question | Correct Security Notion |
|----------|--------------------------|
| “Does the attacker learn plaintext?” | IND-CPA |
| “Does this resist padding-oracle attacks?” | IND-CCA |
| “Can the attacker inject messages?” | INT-CTXT |
| “Is this fully authenticated encryption?” | IND-CCA + INT-CTXT |

---

## 13. Common Exam Traps

- “IND-CPA implies integrity.” → False  
- “IND-CCA only matters for public-key crypto.” → False  
- “MAC prevents eavesdropping.” → False  
- “Encryption prevents forgery.” → False  
- “INT-CTXT implies confidentiality.” → False  

---

## 14. Final Memory Hook

- **IND-CPA** → secrecy against *passive* attackers  
- **IND-CCA** → secrecy against *active* attackers  
- **INT-CTXT** → protection against *forgery*  
- **AE** → all three properties together

---