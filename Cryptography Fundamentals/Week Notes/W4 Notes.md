##  Learning Goals
- Understand AES
- Learn why modes are needed
##  [[W4 Key definitions|Key Definitions]]
- Block cipher:
- [[W4 Modes of Operations|Mode of operation:]]
- Nonce:
- Initialization Vector (IV):

##  [[W4 Core Concepts|Core Concepts]]
- AES round structure
- ECB mode (insecure)
- CBC mode
- CTR mode

##  [[W4 Attacks and Pitfalls|Attacks & Pitfalls]]
- ECB pattern leakage
- CTR nonce reuse

## [[W4 Intuition and Examples|Intuition & Examples]]
- Encrypting images with ECB
- Stream cipher behavior in CTR

##  Exam Notes
- [[W4 Theorems|AES is a block cipher and is modeled as a PRP]].
- A block cipher alone only encrypts one fixed-size block.
- Modes of operation allow secure encryption of long messages.
- ECB mode is deterministic and leaks patterns.
- ECB is never secure and must never be used.
- CBC mode randomizes encryption using an IV.
- CTR mode turns a block cipher into a stream cipher.
- CBC and CTR are CPA-secure if used correctly.
- Reusing an IV or nonce breaks security.
- Nonce reuse in CTR is identical to OTP key reuse.

##  Links
- Prev: [[W3 Notes]]
- Next: [[W5 Notes]]