
## Public Key Infrastructure (PKI)
A system for:
- Managing public keys
- Binding identities to public keys
- Distributing trust using certificates

---

## Digital Certificate
A signed data structure that binds:
$$
\text{Identity} \leftrightarrow \text{Public Key}
$$

---

## Certificate Authority (CA)
A trusted entity that:
- Verifies identities
- Signs certificates

---

## Root CA
A CA whose public key is **pre-installed as trusted** in operating systems and browsers.

---

## Intermediate CA
A CA whose certificate is signed by a root CA and which can issue certificates to others.

---

## Certificate Chain
A sequence of certificates:
$$
\text{Leaf} \rightarrow \text{Intermediate} \rightarrow \text{Root}
$$
used to establish trust.

---

## TLS (Transport Layer Security)
A cryptographic protocol that provides:
- Confidentiality
- Integrity
- Authentication

for network communication.

---

## Server Authentication
Process by which a client verifies that it is communicating with the **real server** using certificates.

---

## Revocation
The process of invalidating a certificate before its expiration date.

---

## Certificate Revocation List (CRL)
A periodically published list of revoked certificates.

---

## Online Certificate Status Protocol (OCSP)
A protocol for checking in real time whether a certificate is revoked.
