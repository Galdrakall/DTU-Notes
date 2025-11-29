
---
# Cryptography

## Definition
Cryptography is the science of securing communication against adversaries.

Its main goals are to protect information even when an attacker:
- Can observe all communication
- Knows how the system works
- Has significant computational power

## What Cryptography Tries to Achieve
- Confidentiality: Only intended recipients can read the message
- Integrity: Messages cannot be altered unnoticed
- Authentication: Prove who sent a message
- Non-repudiation: A sender cannot deny having sent a message

## What Cryptography Does NOT Do
- It does not prevent messages from being intercepted
- It does not stop communication, only protects its contents
- It does not replace good system security

## Exam Notes
- Cryptography assumes attackers see everything except secret keys
- Security is based on mathematics, not secrecy of algorithms
---
# Goals of Cryptography

## Confidentiality
Prevents unauthorized reading of messages.

Formally:
$$
\Pr[\mathcal{A} \text{ guesses } m] \le \frac{1}{2} + \text{negl}(\lambda)
$$

## Integrity
Prevents undetected message modification.

Formally:
$$
\Pr[\mathcal{A} \text{ forges valid message}] \le \text{negl}(\lambda)
$$

## Authentication
Ensures the sender’s identity is genuine.

## Non-Repudiation
Prevents a sender from denying authorship later.

## Exam Notes
- Encryption only gives confidentiality
- MACs give integrity + authenticity
- Signatures give authentication + non-repudiation
---

# Adversary

## Definition
An adversary is an algorithm that attempts to break a cryptographic scheme.

It is typically modeled as a **probabilistic polynomial-time (PPT)** algorithm.


## Formal Success Probability
The adversary’s success is measured as:

$$
\Pr[\mathcal{A} \text{ wins the security game}]
$$

The **advantage** is often defined as:

$$
\text{Adv}_{\mathcal{A}} = 
\left| \Pr[\mathcal{A} \text{ outputs } 1] - \frac{1}{2} \right|
$$

Security requires this to be negligible.


## Capabilities
- Observe all traffic
- Choose plaintexts (CPA)
- Request decryptions (CCA)
- Replay or modify messages

## Exam Notes
- Adversaries are always modeled as algorithms
- Their power is bounded by polynomial time

---
# Threat Model

## Definition
A threat model formally specifies:
- The attacker
- The attacker’s capabilities
- The attacker’s limitations

Security guarantees only hold **inside this model**.

## Formal View (Security Games)
Security is defined via an experiment:

$$
\text{Exp}_{\mathcal{A}}^{\text{game}}(\lambda)
$$

The adversary wins with probability:

$$
\Pr[\text{Exp}_{\mathcal{A}}^{\text{game}}(\lambda) = 1]
$$

A scheme is secure if this probability is negligible.

## Typical Assumptions
- Full control of the network
- Can observe, modify, replay messages
- Can query encryption/decryption oracles
- Knows all algorithms

## Example
If the model does not allow message modification, integrity is never tested.

## Exam Notes
- No threat model → no meaningful security claim
- Every theorem explicitly quantifies over attackers
---

# Kerckhoffs’ Principle

## Definition
A cryptosystem must remain secure even if everything about the system is public, except the secret key.

## Formal Interpretation
Security must depend only on:

$$
k \leftarrow \{0,1\}^{\lambda}
$$

and *not* on secret algorithms.

All advantage bounds must hold:

$$
\Pr[\mathcal{A} \text{ breaks system}] \le \text{negl}(\lambda)
$$

even when $\mathcal{A}$ knows:
- Encryption algorithm
- Decryption algorithm
- Protocol details

## Violation: Security Through Obscurity
If security relies on hiding the algorithm instead of hiding:

$$
k \in K
$$

the system is not cryptographically secure.

## Exam Notes
- Modern crypto always assumes public algorithms
- Only keys are secret