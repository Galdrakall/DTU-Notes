
---
# Security Through Obscurity

## Definition
Security that depends on keeping the algorithm secret instead of the key.

## Formal Failure Mode
If breaking the system becomes easy after learning the algorithm:

$$
\Pr[\mathcal{A}^{\text{public alg}} \text{ wins}] \approx 1
$$

then the scheme violates Kerckhoffs’ Principle.

## Why It Fails
- Algorithms leak
- Reverse engineering is standard
- Insider disclosure is common

## Correct Requirement
Security must depend only on:

$$
k \in \{0,1\}^{\lambda}
$$

not on hidden design.

## Exam Notes
- Any system relying on hidden design is insecure by cryptographic standards