---
name: whip-security
description: "Review code for security vulnerabilities. Use before committing code that handles user input, authentication, data storage, external integrations, or any web-facing feature. Covers OWASP Top 10, input validation, secrets management, and SSRF prevention."
---

# Security Review

## Overview

Security-first review for web applications. Treat every external input as hostile, every secret as sacred, every authorization check as mandatory.

## When to Use

- Before committing code that accepts user input
- Before implementing authentication or authorization
- Before handling sensitive data (PII, payment, credentials)
- Before integrating with external APIs
- Before adding file uploads, webhooks, or callbacks

## Threat Model (5 Minutes)

Controls without a threat model are guesses:

1. **Map trust boundaries.** Where does untrusted data enter? HTTP requests, form fields, file uploads, webhooks, third-party APIs, **LLM output**.
2. **Name the assets.** What's worth stealing? Credentials, PII, payment data, admin actions.
3. **Quick STRIDE per boundary:**

| Threat | Ask | Mitigation |
|---|---|---|
| Spoofing | Can someone impersonate a user? | Authentication, signature verification |
| Tampering | Can data be altered? | Parameterized queries, HTTPS |
| Repudiation | Can an action be denied? | Audit logging |
| Info disclosure | Can data leak? | Encryption, field allowlists, generic errors |
| Denial of service | Can it be overwhelmed? | Rate limiting, input size caps, timeouts |
| Elevation of privilege | Can a user gain rights they shouldn't? | Authorization checks, least privilege |

## The Three-Tier System

### Always Do (No Exceptions)

- **Validate all external input** at the system boundary
- **Parameterize all database queries** — never concatenate user input into SQL
- **Encode output** to prevent XSS (use framework auto-escaping)
- **Use HTTPS** for all external communication
- **Hash passwords** with bcrypt/scrypt/argon2 (salt rounds ≥ 12)
- **Set security headers** (CSP, HSTS, X-Frame-Options)
- **Use httpOnly, secure, sameSite cookies** for sessions
- **Run dependency audit** (`npm audit` / `pip audit`) before release

### Ask First (Requires Human Approval)

- Adding new auth flows or changing auth logic
- Storing new categories of sensitive data
- Adding new external service integrations
- Changing CORS configuration
- Adding file upload handlers
- Modifying rate limiting

### Never Do

- **Never commit secrets** to version control
- **Never log sensitive data** (passwords, tokens, full credit card numbers)
- **Never trust client-side validation** as a security boundary
- **Never disable security headers** for convenience
- **Never use `eval()` or `innerHTML`** with user-provided data
- **Never expose stack traces** to users

## OWASP Quick Checks

**Injection:** All queries parameterized? No string concatenation in SQL/commands?

**XSS:** All output encoded? No `innerHTML` with user data? Framework auto-escaping not bypassed?

**Broken Auth:** Passwords hashed? Session tokens httpOnly+secure+sameSite? Login rate-limited?

**Broken Access Control:** Every endpoint checks user permissions? Users can only access their own resources?

**SSRF:** Server-side URL fetches allowlisted? Private IPs rejected? Redirects limited?

**Secrets:** No secrets in code or git history? `.env` in `.gitignore`?

## Securing LLM Features

If your app calls an LLM (chatbots, RAG, agents):

- **Treat all model output as untrusted input.** Never pass LLM output into `eval`, SQL, shell, or `innerHTML`
- **Assume prompts can be hijacked.** Untrusted text in context can carry instructions. Enforce permissions in code, not in the prompt
- **Keep secrets and other users' data out of prompts.** Anything in context can be echoed back
- **Constrain tool permissions.** Scope to minimum, validate every tool argument
- **Bound consumption.** Cap tokens, rate, loop depth

```python
# BAD: trusting model output
sql = await llm.generate(f"Write SQL for: {user_question}")
await db.execute(sql)  # arbitrary query execution

# GOOD: model output is data, parse and validate
intent = CommandSchema.parse(json.loads(await llm.reply_json(user_message)))
await run_allowlisted_action(intent.action, intent.params)
```

## Pre-Commit Checklist

```
□ All user input validated at boundaries
□ SQL queries parameterized (no string concatenation)
□ No secrets in code or git history
□ Auth checked on every protected endpoint
□ Security headers present
□ Error responses don't expose internals
□ Rate limiting on auth endpoints
□ Server URL fetches validated against allowlist
□ LLM output treated as untrusted (if AI features present)
□ Dependency audit clean (no critical/high vulns)
```
