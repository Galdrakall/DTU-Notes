
## Theorem: Euler’s Theorem
If $\gcd(m, N) = 1$, then:
$$
m^{\varphi(N)} \equiv 1 \pmod N
$$

---

## Correctness of RSA
Because:
$$
ed \equiv 1 \pmod{\varphi(N)}
$$
we can write:
$$
ed = 1 + k\varphi(N)
$$

Then:
$$
m^{ed} = m^{1+k\varphi(N)} \equiv m \pmod N
$$

---

## Theorem: Deterministic Encryption Is Not IND-CPA Secure
If:
$$
\text{Enc}_{pk}(m)
$$
is deterministic, then an attacker can:
- Encrypt guessed messages
- Compare to a challenge ciphertext
- Break IND-CPA security

Therefore, **raw RSA is not IND-CPA secure**.

---

## Security Basis of RSA
RSA security relies on the assumed hardness of:
- Integer factorization
- Computing $d$ from $(N,e)$ without factoring $N$
