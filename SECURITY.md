# Security policy

## Reporting a vulnerability

Do not file public issues containing exploit details or confidential evidence. Contact the repository owner privately through the GitHub profile contact route with impact, reproduction steps, and affected version. Never include real credentials or customer records.

## Security boundaries

- Passwords use salted PBKDF2-HMAC-SHA256; raw session tokens are not stored in the database.
- Session cookies are HTTP-only and SameSite=Lax; production configuration requires Secure cookies over HTTPS.
- Uploads are size-limited, extension-checked and stored under random names outside the public web root. Malware scanning and OCR are not included.
- Secrets remain on the backend and must never be placed in frontend variables.
- Demo data must be disabled before production exposure.
- This release is single-workspace and has not received an independent security audit.

## Before confidential use

Use HTTPS, strong secrets, PostgreSQL, durable private storage, rate limiting, least-privilege credentials and tested backups. Review provider data handling before enabling external AI processing. Perform security and isolation testing before multi-customer hosting.
