
## Intuition: Digital Passports
Certificates are like:
- Passports issued by a trusted government (CA)
- That prove identity to anyone in the world

---

## Example: Visiting a Secure Website
1. You visit a website.
2. The server sends its certificate.
3. Your browser checks:
   - Is it signed by a trusted CA?
   - Is the domain correct?
   - Is it expired?
4. If all checks pass, the lock icon appears.

---

## Intuition: Chain of Trust
You trust:
- The root CA
- Which vouches for the intermediate CA
- Which vouches for the website

Breaking any link breaks trust.

---

## Example: Stolen Private Key
If a server’s private key is stolen:
- Attackers can impersonate the server
- Until the certificate is revoked

---

## Intuition: Why Revocation Is Hard
Clients may be:
- Offline
- Behind firewalls
- Blocked from reaching OCSP servers

This makes real-time revocation unreliable.
