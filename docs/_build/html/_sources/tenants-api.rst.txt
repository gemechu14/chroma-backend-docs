Tenants API
===========

Purpose
-------

Manages tenant onboarding, tenant profile updates, subscription lifecycle actions, and location management for multi-location salons.

Endpoint list
-------------

- ``GET /api/v1/tenants``
- ``GET /api/v1/tenants/mine``
- ``GET /api/v1/tenants/mine/subscription``
- ``GET /api/v1/tenants/mine/subscription/events``
- ``POST /api/v1/tenants/mine/subscription/cancel``
- ``POST /api/v1/tenants/mine/subscription/reactivate``
- ``POST /api/v1/tenants/mine/subscription/renew``
- ``POST /api/v1/tenants/register``
- ``POST /api/v1/tenants``
- ``POST /api/v1/tenants/{tenant_id}/subscription/renew``
- ``GET /api/v1/tenants/{tenant_id}``
- ``PATCH /api/v1/tenants/{tenant_id}``
- ``DELETE /api/v1/tenants/{tenant_id}``
- ``GET /api/v1/tenants/{tenant_id}/locations``
- ``POST /api/v1/tenants/{tenant_id}/locations``
- ``PATCH /api/v1/tenants/{tenant_id}/locations/{location_id}``
- ``DELETE /api/v1/tenants/{tenant_id}/locations/{location_id}``

Auth requirements
-----------------

- Registration endpoints are public or bootstrap-gated.
- Administrative and tenant-scoped operations require Bearer token auth.
- Role checks apply for cross-tenant or billing-sensitive actions.

Key schemas
-----------

- Request: tenant registration/create/update, location create/update, renewal requests
- Response: ``TenantRead``, ``LocationRead``, ``TenantSubscriptionRead``, ``SubscriptionEventRead``

Common errors
-------------

- ``400 Bad Request``: invalid subscription action or malformed payload
- ``401 Unauthorized``: missing/invalid token
- ``403 Forbidden``: tenant access denied by role/ownership checks
- ``404 Not Found``: tenant or location not found
- ``409 Conflict``: duplicate tenant slug/domain/email constraints (if configured)

Example request/response
------------------------

.. code-block:: bash

   curl -X GET "http://localhost:8000/api/v1/tenants/mine" \
     -H "Authorization: Bearer <access_token>"

.. code-block:: json

   {
     "id": 12,
     "name": "Downtown Salon",
     "plan": "premium",
     "status": "active"
   }
