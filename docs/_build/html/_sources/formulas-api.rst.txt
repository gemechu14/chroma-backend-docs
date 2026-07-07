Formulas API
============

Purpose
-------

Captures color formulas and visit-level usage details while atomically deducting inventory usage from tracked stock.

Endpoint list
-------------

- ``GET /api/v1/formulas``
- ``POST /api/v1/formulas/visit``
- ``GET /api/v1/formulas/visits/{visit_id}``
- ``POST /api/v1/formulas``
- ``GET /api/v1/formulas/{formula_id}``
- ``PATCH /api/v1/formulas/{formula_id}``
- ``DELETE /api/v1/formulas/{formula_id}``

Auth requirements
-----------------

- All routes require Bearer token auth.
- Tenant and location boundaries apply to formula read/write operations.

Key schemas
-----------

- Request: formula/visit create and update payloads with formula items
- Response: ``FormulaRead``, ``FormulaVisitRead``, ``PaginatedResponse[FormulaRead]``

Common errors
-------------

- ``400 Bad Request``: insufficient stock or invalid formula item payload
- ``401 Unauthorized``: missing/invalid token
- ``403 Forbidden``: role or tenant access violation
- ``404 Not Found``: formula/visit record not found
- ``422 Unprocessable Entity``: schema validation failure

Example request/response
------------------------

.. code-block:: bash

   curl -X POST "http://localhost:8000/api/v1/formulas/visit" \
     -H "Authorization: Bearer <access_token>" \
     -H "Content-Type: application/json" \
     -d '{"customer_id":88,"location_id":3,"items":[{"tenant_product_id":301,"qty_used":20.0}]}'

.. code-block:: json

   {
     "id": 77,
     "customer_id": 88,
     "location_id": 3,
     "total_cost": 12.4,
     "items": [
       {
         "tenant_product_id": 301,
         "qty_used": 20.0
       }
     ]
   }
