## OTP Key Reuse Attack
If the same key $k$ is reused:

$$
c_1 = m_1 \oplus k
$$
$$
c_2 = m_2 \oplus k
$$

Then:
$$
c_1 \oplus c_2 = m_1 \oplus m_2
$$

This completely breaks security and leaks information about both messages.

---

## Small Key Space Attacks
If $|K|$ is small:
- Brute-force searching all keys is feasible
- Perfect secrecy is impossible

This is why classical ciphers are insecure.

---

## Predictable Key Generation
If the OTP key is not truly random:
- Ciphertexts become statistically biased
- Security collapses completely