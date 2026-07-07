Users API
=========

Purpose
-------

Provides authentication token issuance and tenant-scoped user management (staff profiles, role-aware access, and account lifecycle operations).

Endpoint list
-------------

- ``POST /api/v1/users/login``
- ``POST /api/v1/users/refresh``
- ``GET /api/v1/users/me``
- ``GET /api/v1/users``
- ``POST /api/v1/users``
- ``GET /api/v1/users/{user_id}``
- ``PATCH /api/v1/users/{user_id}``
- ``DELETE /api/v1/users/{user_id}``

Auth requirements
-----------------

- ``/login`` and ``/refresh`` are token bootstrap endpoints.
- All other endpoints require Bearer token auth and enforce tenant isolation.

Key schemas
-----------

- Request: login credentials, user create/update payloads
- Response: ``Token``, ``UserMeRead``, ``UserRead``, ``PaginatedResponse[UserRead]``

Common errors
-------------

- ``400 Bad Request``: invalid role transition or malformed payload
- ``401 Unauthorized``: missing/invalid token
- ``403 Forbidden``: insufficient role permissions
- ``404 Not Found``: user not found within tenant scope
- ``422 Unprocessable Entity``: schema validation failure

Example request/response
------------------------

.. code-block:: bash

   curl -X POST "http://localhost:8000/api/v1/users/login" \
     -H "Content-Type: application/json" \
     -d '{"email":"owner@example.com","password":"<password>"}'

.. code-block:: json

   {
     "access_token": "<jwt-access-token>",
     "refresh_token": "<jwt-refresh-token>",
     "token_type": "bearer"
   }
