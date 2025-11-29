## Intuition: What “Security” Means in Cryptography
Cryptography does not aim to make attacks impossible.
Instead, it aims to make attacks either:
- Information-theoretically impossible (perfect security), or
- Computationally infeasible (computational security)

Security always assumes a powerful attacker.

---

## Intuition: Why We Assume the Worst-Case Attacker
If a system is only secure against weak attackers, it is not really secure.

Worst-case assumptions protect against:
- Future improvements in computing power
- Smarter attacks
- Unknown attack strategies

This gives long-term confidence in security.

---

## Example: Security Through Obscurity
Suppose an encryption algorithm is kept secret and no secret key is used.

- As long as the algorithm is unknown, the system appears secure.
- Once the algorithm leaks, the attacker can immediately decrypt everything.

This shows why **hiding algorithms is not real security**.

---

## Intuition: Why Only the Key Should Be Secret
Algorithms:
- Must be public for peer review
- Are eventually reverse-engineered
- Can be copied perfectly by attackers

Keys:
- Can be rotated
- Can be revoked
- Are small enough to protect securely

Therefore, security must depend only on secret keys.

---

## Example: Lock and Key Analogy
Think of cryptography like a lock:

- The design of the lock is public
- Anyone can buy and examine it
- Security comes only from the physical key

If the lock were only secure because its design was secret, it would fail instantly once exposed.

---

## Intuition: Perfect Security vs Computational Security
Perfect security means:
- Even with unlimited time and computers, the attacker learns nothing.

Computational security means:
- The attacker could break the system in theory
- But not within any realistic amount of time

Perfect security is stronger but usually impractical.
Computational security is weaker but usable in practice.

---

## Example: Safe vs Time-Locked Safe
Perfect security is like a safe that **cannot be opened by any method**.
Computational security is like a safe that **could be opened with enough time**, but that time would exceed the age of the universe.

---

## Intuition: Why Formal Definitions Are Necessary
Without formal definitions:
- “Secure” becomes a vague marketing claim
- Different people assume different attackers
- Systems appear secure but fail in practice

Formal models force us to state exactly:
- What the attacker can do
- What success means
- How security is measured

---

## Example: Weak Threat Model Failure
If a designer assumes:
- The attacker only listens
- The attacker never modifies traffic

Then they may ignore:
- Message tampering
- Replay attacks
- Impersonation

A real attacker will exploit these gaps immediately.

---

## Intuition: Why Cryptography Is Not the Same as Security
Cryptography protects:
- Messages
- Keys
- Proofs of identity

It does NOT automatically protect:
- Password storage
- Software bugs
- Social engineering
- Physical device theft

Crypto is only one layer in a full security system.

---

## Exam Intuition Summary
- Always assume a powerful attacker.
- Hiding algorithms is never enough.
- Only keys should be secret.
- Perfect security is absolute but impractical.
- Computational security is practical but assumption-based.
- Formal definitions prevent false confidence.