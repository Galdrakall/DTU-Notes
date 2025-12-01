
## Theorem: Certificate Authenticity
If:
- A certificate verifies under a trusted CA public key
- The CA is honest

Then the public key in the certificate is correctly bound to the stated identity.

---

## Theorem: TLS Server Authentication
If:
- Certificate validation is correct
- The private key is not compromised
- The handshake is cryptographically sound

Then the client is authenticated to the **intended server**.

---

## Proof Idea (TLS Authentication)
Security is reduced to:
- Signature unforgeability of the CA
- Correct certificate chain validation
- Soundness of the key-exchange protocol

---

## Revocation Soundness (Informal)
If revocation information is:
- Available
- Fresh
- Authenticated

Then clients can reliably reject compromised certificates.
