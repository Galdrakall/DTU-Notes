
##  Learning Goals
- Understand cryptographic hashes
- Learn formal hash security
- [[Hash Attacks]]

##  [[W6 Key Definitions|Key Definitions]]
- Preimage resistance:
- Second preimage resistance:
- Collision resistance:

##  [[W6 Core Concepts|Core Concepts]]
- Merkle–Damgård construction
- Salting
- Iterated hashing

##  [[W6 Attacks and Pitfalls|Attacks & Pitfalls]]
- Collision attacks
- Length extension attacks

##  [[W6 Intuition and Examples|Intuition & Examples]]
- Fingerprints for data

## Exam Notes
- Hash functions map arbitrary input to fixed-length output.
- Core security properties:
  - Preimage resistance
  - Second preimage resistance
  - Collision resistance
- Collision resistance is limited by the birthday bound.
- Merkle–Damgård uses iterative compression.
- Length-extension attacks affect naive hash-based MACs.
- HMAC is safe against length extension.
- Passwords must be hashed with salts.
- Fast hash functions are bad for password storage.
- MD5 and SHA-1 are broken.

##  Links
- Prev: [[W5 Notes]]
- Next: [[W7 Notes]]