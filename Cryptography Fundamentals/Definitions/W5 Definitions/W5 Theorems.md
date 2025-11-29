
## Theorem: MAC Unforgeability (Formal)
For any PPT adversary $\mathcal{A}$:
$$
\Pr[\mathcal{A} \text{ forges a valid tag}] \le \text{negl}(\lambda)
$$

---

## Theorem: Security of PRF-Based MAC
If $F_k$ is a secure PRF, then the MAC:
$$
t = F_k(m)
$$
is unforgeable under chosen-message attack.

---

## Theorem: CBC-MAC Security (Fixed-Length Messages)
CBC-MAC is secure **only if all messages have the same fixed length**.

Variable-length CBC-MAC is insecure without length encoding.

---

## Theorem: Encrypt-then-MAC Security
If:
- The encryption scheme is IND-CPA secure
- The MAC scheme is unforgeable

Then Encrypt-then-MAC is:
- IND-CCA secure
- Provides ciphertext integrity (INT-CTXT)

---

## Proof Idea: Encrypt-then-MAC
Hybrid argument:
1. Replace real MAC with a random function
2. Show forging implies MAC break
3. Confidentiality follows from CPA security
4. Integrity follows from unforgeability
