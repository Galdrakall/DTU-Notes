## Theorem: Perfect Secrecy of the One-Time Pad

If:
1. $k$ is chosen uniformly at random
2. $|k| = |m|$
3. $k$ is used only once
4. $k$ is kept secret

Then the OTP satisfies:
$$
\Pr[M=m \mid C=c] = \Pr[M=m]
$$

So the One-Time Pad is perfectly secure.

---

## Intuition of the Proof
For any fixed ciphertext $c$ and any plaintext $m$, there exists exactly one key:
$$
k = m \oplus c
$$
Because all keys are equally likely, all plaintexts are equally likely.

---

## Shannon’s Bound on Perfect Secrecy
For any perfectly secure encryption scheme:
$$
|K| \ge |M|
$$
The key space must be at least as large as the message space.

This explains why OTP requires very long keys.