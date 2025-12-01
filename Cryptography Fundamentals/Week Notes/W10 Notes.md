##  Learning Goals
- Understand elliptic curve groups
- Learn why ECC is efficient
[[W10 Theorems]]
##  [[W10 Key Definitions|Key Definitions]]
- Elliptic curve:
- Point addition:
- ECDH:

##  [[W10 Core Concepts|Core Concepts]]
- Elliptic curves over finite fields
- Point doubling
- Scalar multiplication

##  [[W10 Attacks and Pitfalls|Attacks & Pitfalls]]
- Weak curves
- Bad parameter choice

##  [[W10 Intuition and Examples|Intuition & Examples]]
- Curve intersections for addition

##  Exam Notes
- PKI binds identities to public keys using certificates.
- Certificates are digitally signed by CAs.
- Trust is rooted in pre-installed root CA public keys.
- Certificate chains link leaf certificates to roots.
- TLS uses certificates for server authentication.
- After authentication, symmetric crypto is used.
- Revocation is done via CRLs and OCSP.
- Compromised CAs break global trust.
- Users ignoring certificate warnings destroys security.

##  Links
- Prev: [[W9 Notes]]
- Next: [[W11 Notes]]