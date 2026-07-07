Security
========

Auth model
----------

- JWT-based authentication using access and refresh tokens.
- Google OAuth supported as external identity source.
- Tenant-aware authorization enforced in router/crud layers.

Token and session handling
--------------------------

- Use short-lived access tokens and rotate refresh tokens on use.
- Transmit tokens only over HTTPS in production.
- Validate issuer/audience claims if cross-service token usage is introduced.
- Revoke refresh tokens on password reset or account disable events.

Secrets management
------------------

- Store ``SECRET_KEY``, OAuth client secrets, Stripe keys, and SMTP credentials in secret managers or encrypted environment stores.
- Do not commit ``.env`` files or plaintext credentials.
- Rotate high-impact secrets on schedule and after incidents.

Rate limiting and abuse controls
--------------------------------

Assumptions:

- No explicit application-layer limiter is currently configured in this codebase.

Recommendations:

- Apply gateway-level limits for ``/api/v1/users/login``, password reset endpoints, and ``/api/v1/places/autocomplete``.
- Apply webhook signature validation and request-size limits on ``/api/v1/stripe/webhook``.
- Add brute-force protection and temporary lockouts for repeated failed auth attempts.

Security best practices
-----------------------

- Narrow CORS allowlist in production (do not use ``*``).
- Enforce least-privilege role checks for all mutation routes.
- Log security-relevant events (login failures, privilege changes, billing changes).
- Keep dependencies patched (FastAPI, SQLAlchemy, auth libraries, Stripe SDK).
- Run periodic secret scanning and dependency vulnerability checks in CI.
