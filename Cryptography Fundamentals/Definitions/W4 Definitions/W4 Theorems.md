## Theorem: Block Cipher as a PRP
A secure block cipher is modeled as a pseudorandom permutation:
$$
E_k \approx \pi
$$
where $\pi$ is a uniform random permutation.

---

## Theorem: ECB is Not CPA-Secure
ECB produces identical ciphertext blocks for identical plaintext blocks:
$$
m_i = m_j \Rightarrow c_i = c_j
$$

Thus, ECB is distinguishable from random encryption and is not IND-CPA secure.

---

## Theorem: CBC Mode is CPA-Secure
If:
- The block cipher is a secure PRP
- The IV is uniformly random and never reused

Then CBC mode is IND-CPA secure.

---

## Theorem: CTR Mode is CPA-Secure
If:
- The block cipher is a secure PRF
- The nonce/counter is never reused

Then CTR mode is IND-CPA secure.

---

## Proof Idea (CBC Security)
Use a hybrid argument:
- Replace PRP with a random permutation (PRP–PRF switching)
- Show XOR with random values hides plaintext
- Conclude indistinguishability

---

## Proof Idea (CTR Security)
Show that:
- The keystream $E_k(\text{ctr}+i)$ is pseudorandom
- XOR with a pseudorandom keystream gives a secure stream cipher