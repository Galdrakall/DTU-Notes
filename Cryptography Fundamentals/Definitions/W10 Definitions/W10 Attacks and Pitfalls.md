
## Man-in-the-Middle Without PKI
Without certificate validation, attackers can:
- Replace public keys
- Impersonate servers
- Decrypt and modify traffic

---

## Compromised Certificate Authorities
If a CA is hacked:
- Attackers can issue valid certificates
- For any domain

This breaks global trust.

---

## Fake Certificates
If users:
- Ignore browser warnings
- Manually trust attacker certificates

MITM attacks become trivial.

---

## Weak Domain Validation
If the client does not properly verify:
- Domain name
- SAN fields

Attackers can use valid certificates for the wrong domain.

---

## Revocation Failures
If revocation is not checked:
- Stolen certificates remain valid
- Long-term impersonation becomes possible

---

## Certificate Expiration Misuse
Expired certificates:
- Break authentication
- Can enable downgrade-style impersonation attacks

---

## TLS Downgrade Attacks
Attackers attempt to:
- Force older versions of TLS
- Remove forward secrecy
- Enable broken cryptography
