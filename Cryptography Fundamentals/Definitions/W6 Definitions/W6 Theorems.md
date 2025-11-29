
## Birthday Bound
For an $n$-bit hash output:
- Expected work to find a collision is:
$$
O(2^{n/2})
$$

---

## Theorem: Collision Resistance Upper Bound
No $n$-bit hash function can have collision resistance better than:
$$
2^{n/2}
$$
due to the birthday paradox.

---

## Theorem: Merkle–Damgård Collision Resistance
If the compression function $f$ is collision resistant, then the Merkle–Damgård construction is also collision resistant.

---

## Theorem: Length-Extension Vulnerability
For any Merkle–Damgård hash:
Given $H(m)$ and $|m|$, it is possible to compute:
$$
H(m \parallel m')
$$
without knowing $m$.

---

## Proof Idea (Birthday Attack)
Randomly sample messages:
$$
m_1, m_2, \dots
$$
Until two produce the same hash.
Expected samples: $2^{n/2}$.

---

## Proof Idea (Length Extension)
Because the final internal state $h_\ell = H(m)$ is reused as the next chaining value, the attacker can continue the iteration without knowing $m$.
