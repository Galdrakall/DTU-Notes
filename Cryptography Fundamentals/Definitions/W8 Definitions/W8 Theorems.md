
## Theorem: Correctness of Diffie–Hellman
Alice computes:
$$
K = (g^b)^a = g^{ba}
$$
Bob computes:
$$
K = (g^a)^b = g^{ab}
$$
Since $ab = ba$, both obtain the same shared key.

---

## Theorem: CDH Assumption
There is no PPT algorithm that, given $(g^a, g^b)$, can compute:
$$
g^{ab}
$$
with non-negligible probability.

---

## Theorem: DDH Assumption
No PPT adversary can distinguish:
$$
(g^a, g^b, g^{ab}) \quad \text{from} \quad (g^a, g^b, g^c)
$$
where $c$ is random.

---

## Theorem: IND-CPA Security of ElGamal
If the DDH assumption holds in $G$, then ElGamal encryption is IND-CPA secure.

---

## Proof Idea (ElGamal)
If an adversary could distinguish ElGamal encryptions of two messages, it could also distinguish:
$$
g^{ab} \text{ from } g^c
$$
thereby breaking DDH.
