##  Learning Goals
- Authenticate messages
- Prevent forgery and tampering
- [[IND Comparison]]

##  [[W5 Key definitions|Key Definitions]]
- MAC:
- Tag:
- [[W5 Theorems|Mac Unforgeability]]:
- IND-CCA:

##  [[W5 Core Concepts|Core Concepts]]
- MACs from PRFs
- CBC-MAC
- Encrypt-then-MAC

##  [[W5 Attacks and Pitfalls|Attacks & Pitfalls]]
- MACing plaintext instead of ciphertext
- Chosen-ciphertext attacks

## [[W5 Intuition and Examples|Intuition & Examples]]
- Tamper-proof sealing

##  Exam Notes
- Encryption alone does not give integrity.
- A MAC provides integrity and authenticity, not confidentiality.
- MAC security means unforgeability under chosen-message attack.
- PRFs can be used to build secure MACs.
- CBC-MAC is secure only for fixed-length messages.
- The correct composition is Encrypt-then-MAC.
- Encrypt-then-MAC gives IND-CCA security.
- MAC-then-Encrypt is generally insecure.
- AE and AEAD are the modern standard.
- GCM and ChaCha20-Poly1305 are widely used AEAD schemes.

##  Links
- Prev: [[W4 Notes]]
- Next: [[W6 Notes]]