Platform API
============

Purpose
-------

Exposes platform-owner endpoints for super-admin bootstrap and global billing configuration management.

Endpoint list
-------------

- ``POST /api/v1/platform/bootstrap``
- ``GET /api/v1/platform/billing/plans``
- ``PATCH /api/v1/platform/billing/plans/{interval}``
- ``GET /api/v1/platform/billing/config``
- ``PATCH /api/v1/platform/billing/config``

Auth requirements
-----------------

- Bootstrap endpoint uses platform bootstrap secret logic.
- Billing config endpoints require platform-level privileges.

Key schemas
-----------

- Request: bootstrap payload, plan/config update payloads
- Response: ``SubscriptionPlanAdmin``, ``PlatformBillingConfigRead``

Common errors
-------------

- ``400 Bad Request``: invalid interval/config payload
- ``401 Unauthorized``: missing/invalid platform credential
- ``403 Forbidden``: caller is not platform admin
- ``409 Conflict``: bootstrap already completed (if idempotency checks apply)
- ``422 Unprocessable Entity``: request validation failure

Example request/response
------------------------

.. code-block:: bash

   curl -X GET "http://localhost:8000/api/v1/platform/billing/config" \
     -H "Authorization: Bearer <platform_admin_token>"

.. code-block:: json

   {
     "stripe_enabled": true,
     "trial_days_default": 14
   }
