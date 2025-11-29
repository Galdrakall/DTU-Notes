## Intuition: Why We Fake Randomness
True randomness is slow and expensive.
We simulate it using deterministic algorithms that:
- Look random
- Behave randomly to attackers

This is pseudorandomness.

## Example: Random vs Pseudorandom Coin Flips
True coin flips come from physics.
PRF-based coin flips come from:
$$
F_k(1), F_k(2), F_k(3), \dots
$$
To any attacker, both sequences look the same.

## Intuition: What a PRF Really Is
A PRF acts like:
- A huge random lookup table
- Chosen once by the key
- Fixed for all time

## Example: Stream Cipher as PRF
A stream cipher generates:
$$
k_1, k_2, k_3, \dots
$$
and encrypts via:
$$
c_i = m_i \oplus k_i
$$

Security depends entirely on pseudorandomness of $k_i$.

## Intuition: PRP as a Scrambled Dictionary
A PRP is like:
- A perfect shuffle of all inputs
- Reversible with the right key
- Completely unpredictable without it