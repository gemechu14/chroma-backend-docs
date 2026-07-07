Auth API
========

Purpose
-------

Handles external authentication and account recovery flows, including Google OAuth handshakes, password setup, and password reset.

Endpoint list
-------------

- ``GET /api/v1/auth/google/config``
- ``GET /api/v1/auth/google/start``
- ``GET /api/v1/auth/google/callback``
- ``POST /api/v1/auth/google/callback``
- ``POST /api/v1/auth/set-password``
- ``POST /api/v1/auth/forgot-password``
- ``POST /api/v1/auth/reset-password``

Auth requirements
-----------------

- OAuth endpoints are public and use provider callbacks.
- Password management endpoints may require one-time tokens delivered by email/invite workflows.

Key schemas
-----------

- Request: password reset and set-password payloads
- Response: ``Token``, ``GoogleOAuthConfigResponse``, ``GoogleStartResponse``

Common errors
-------------

- ``400 Bad Request``: invalid callback payload, token mismatch, malformed request
- ``401 Unauthorized``: invalid or expired auth/reset token
- ``404 Not Found``: user or invite context not found
- ``422 Unprocessable Entity``: validation errors

Example request/response
------------------------

.. code-block:: bash

   curl -X POST "http://localhost:8000/api/v1/auth/forgot-password" \
     -H "Content-Type: application/json" \
     -d '{"email":"stylist@example.com"}'

.. code-block:: json

   {
     "message": "If the account exists, password reset instructions were sent."
   }
