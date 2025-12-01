
## Why PKI Is Needed
Public-key cryptography alone does NOT prove identity.
Anyone can generate a key pair and claim to be anyone.

PKI solves the **binding problem**:
$$
\text{Who owns this public key?}
$$

---

## Structure of a Digital Certificate
A certificate contains:
- Subject identity (domain name, organization, etc.)
- Public key
- Issuer (the CA)
- Validity period
- Digital signature of the CA

---

## Certificate Signing
A CA signs a certificate by computing:
$$
\sigma = \text{Sign}_{sk_{CA}}(\text{CertData})
$$

Anyone can verify the certificate using:
$$
pk_{CA}
$$

---

## Certificate Chains and Trust Anchors
Trust is bootstrapped from **root CAs**.
If a certificate chains up to a trusted root, it is accepted.

---

## TLS High-Level Handshake (Modern View)
1. Client connects to server.
2. Server sends:
   - Certificate chain
3. Client verifies:
   - Signatures
   - Domain name
   - Expiry
   - Revocation status
4. Key exchange (e.g., ECDHE).
5. Secure channel established using symmetric crypto.

---

## Authentication via Certificates
The server proves possession of the private key corresponding to the certified public key by signing handshake data.

---

## Why Symmetric Crypto Comes After PKI
PKI is slow and expensive.
After authentication and key exchange:
- AES/GCM and MACs are used for bulk data.

---

## Trust Store
A database of trusted root CA public keys stored in:
- Browsers
- Operating systems
- Mobile devices
