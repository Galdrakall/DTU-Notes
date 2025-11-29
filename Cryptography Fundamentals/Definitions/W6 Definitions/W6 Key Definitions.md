
## Cryptographic Hash Function
A deterministic function:
$$
H : \{0,1\}^* \to \{0,1\}^n
$$
that maps arbitrary-length input to a fixed-length output.

---

## Preimage Resistance
Given $y$, it is computationally infeasible to find $m$ such that:
$$
H(m) = y
$$

---

## Second Preimage Resistance
Given a message $m$, it is infeasible to find a different $m' \neq m$ such that:
$$
H(m') = H(m)
$$

---

## Collision Resistance
It is infeasible to find **any** pair $m \neq m'$ such that:
$$
H(m) = H(m')
$$

---

## Random Oracle Model (ROM)
A theoretical model where a hash function is treated as a truly random function.

---

## Merkle–Damgård Construction
A method for building a hash function from a compression function by iterating it over message blocks.

---

## Compression Function
A fixed-length function:
$$
f : \{0,1\}^{n+b} \to \{0,1\}^n
$$
used as the core building block of many hash functions.

---

## Salt
A random value added to a message before hashing to prevent precomputation attacks.

---

## Length-Extension Attack
An attack exploiting the iterative structure of Merkle–Damgård hashes to compute:
$$
H(m \parallel m')
$$
given only $H(m)$ and $|m|$.
