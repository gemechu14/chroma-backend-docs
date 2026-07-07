Operations
==========

Health checks
-------------

- Liveness endpoint: ``GET /health``
- Validate database connectivity during readiness checks (recommended extension).

Logging and monitoring
----------------------

- Use structured application logs (JSON preferred in production).
- Track API latency, error rates, and authentication failures.
- Monitor critical paths:

  - auth endpoints
  - inventory transactions
  - formula creation/visit write paths
  - Stripe webhook processing

Backups and restore
-------------------

Assumptions:

- Primary state is in PostgreSQL in production.

Guidelines:

- Schedule daily full backups and point-in-time recovery.
- Encrypt backups at rest and in transit.
- Perform restore drills in non-production at least monthly.

Migration process
-----------------

- Schema changes are managed with Alembic.
- Deployment order:

  1. Build artifact/container.
  2. Run ``alembic upgrade head``.
  3. Start or roll application instances.
  4. Verify ``/health`` and smoke test critical routes.

Incident checklist
------------------

1. Confirm symptom scope (single tenant vs platform-wide).
2. Check recent deploy, migration, and configuration changes.
3. Verify DB and external provider status (OAuth, SMTP, Stripe, Places).
4. Mitigate user impact (disable failing integration paths if needed).
5. Document timeline, root cause, and corrective actions.
