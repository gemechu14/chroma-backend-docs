Customers API
=============

Purpose
-------

Stores and retrieves tenant customer records used by formula history and service workflows.

Endpoint list
-------------

- ``GET /api/v1/customers``
- ``POST /api/v1/customers``
- ``GET /api/v1/customers/{customer_id}``
- ``PATCH /api/v1/customers/{customer_id}``
- ``DELETE /api/v1/customers/{customer_id}``

Auth requirements
-----------------

- All customer endpoints require Bearer token auth.
- Queries are tenant-scoped.

Key schemas
-----------

- Request: ``CustomerCreate``, ``CustomerUpdate``
- Response: ``CustomerRead``, ``PaginatedResponse[CustomerRead]``

Common errors
-------------

- ``401 Unauthorized``: missing/invalid token
- ``403 Forbidden``: role does not permit operation
- ``404 Not Found``: customer record not in tenant scope
- ``422 Unprocessable Entity``: invalid customer payload

Example request/response
------------------------

.. code-block:: bash

   curl -X POST "http://localhost:8000/api/v1/customers" \
     -H "Authorization: Bearer <access_token>" \
     -H "Content-Type: application/json" \
     -d '{"full_name":"Jane Doe","phone":"+1-202-555-0123"}'

.. code-block:: json

   {
     "id": 88,
     "full_name": "Jane Doe",
     "phone": "+1-202-555-0123"
   }
