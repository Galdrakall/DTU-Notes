## Intuition: Why a Block Cipher Alone Is Not Enough
A block cipher only encrypts fixed-size blocks.
Real messages are long.
Without a mode, encrypting each block independently leaks patterns.

---

## Example: ECB Image Leak
If encrypted in ECB mode:
- Identical pixel blocks encrypt to identical ciphertext blocks
- The outline of the original image remains visible

This visually demonstrates why ECB is insecure.

---

## Intuition: What CBC Really Does
CBC mixes each plaintext block with the previous ciphertext block before encryption.
This ensures:
- Same plaintext blocks encrypt differently
- Each ciphertext depends on all previous blocks

---

## Example: CBC First Block
The first block uses:
$$
c_1 = E_k(m_1 \oplus \text{IV})
$$
The random IV ensures different encryptions of the same message look different.

---

## Intuition: CTR as a Stream Cipher
CTR generates a pseudorandom keystream:
$$
E_k(\text{ctr}), E_k(\text{ctr}+1), E_k(\text{ctr}+2), \dots
$$

The message is hidden by XORing with this keystream.

---

## Example: CTR Parallelism
Each block is independent:
$$
c_i = m_i \oplus E_k(\text{ctr}+i)
$$
This allows fast parallel encryption and decryption.

---

## Intuition: Why Nonce Reuse Is Catastrophic
Reusing a nonce repeats the keystream.
This turns secure encryption into a two-time pad, which is completely insecure.