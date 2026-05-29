# Source Review

Use this reference when reviewing application code, secrets, or pull request source changes.

## Focus Areas

* Authentication and authorization: missing authentication, IDOR, missing ownership checks, privilege escalation, tenant isolation
* Input handling: SQL injection, command injection, path traversal, SSRF, XSS, unsafe deserialization
* Tokens and sessions: JWT validation, token leakage to logs, insecure token storage or handling
* Sensitive data: PII/PHI exposure, sensitive logging, weak cryptography
* File handling: unrestricted upload, unsafe parsing, dangerous path construction, missing file type validation
* Web security: missing security headers, insecure cookies, CORS misconfiguration
* Configuration: debug mode, disabled certificate validation, disabled security controls, missing production hardening

## Review Notes

Confirm a credible source-to-sink path before reporting injection, traversal, XSS, SSRF, or authorization issues. For suspected secrets, inspect surrounding context and distinguish production credentials from fixtures, examples, and test-only values.
