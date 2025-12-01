
## EUF-CMA Security (Formal)
For any PPT adversary $\mathcal{A}$:
$$
\Pr[\mathcal{A} \text{ forges a new valid signature}] \le \text{negl}(\lambda)
$$

---

## Theorem: Security of Hash-and-Sign
If:
- The underlying signature scheme is secure
- The hash function is collision resistant

Then the hash-and-sign construction is EUF-CMA secure.

---

## Security Basis of RSA Signatures
Security relies on:
- The hardness of RSA
- The collision resistance of the hash function

---

## Theorem: Deterministic Signatures Need Hashing
Signing raw messages directly without hashing:
- Leads to algebraic forgeries
- Breaks unforgeability

---

## Proof Idea (RSA Signatures)
A successful forgery can be turned into:
- Either a collision in the hash
- Or a solution to an RSA inversion problem
