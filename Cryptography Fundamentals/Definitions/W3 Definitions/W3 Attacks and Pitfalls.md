## Predictable Randomness
If randomness is predictable:
- PRFs become distinguishable ^da8579
- Encryption becomes breakable

## Bad Key Generation
If keys are:
- Short
- Reused improperly
- Correlated

Security collapses completely.

## Distinguishability Attacks
If an attacker can tell:
- Random output
- From PRF output

Then the construction is broken.

## Using Non-Cryptographic PRGs
Functions like:
- Linear congruential generators
- System timestamps

Do NOT provide cryptographic randomness.