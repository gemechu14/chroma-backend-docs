Products API
============

Purpose
-------

Manages global product taxonomy (brands, lines, products) and tenant-specific catalog entries (tenant product settings, preferred stock metadata).

Endpoint list
-------------

- ``GET /api/v1/products/brands``
- ``POST /api/v1/products/brands``
- ``PATCH /api/v1/products/brands/{brand_id}``
- ``DELETE /api/v1/products/brands/{brand_id}``
- ``GET /api/v1/products/lines``
- ``POST /api/v1/products/lines``
- ``PATCH /api/v1/products/lines/{line_id}``
- ``DELETE /api/v1/products/lines/{line_id}``
- ``GET /api/v1/products``
- ``POST /api/v1/products``
- ``PATCH /api/v1/products/{product_id}``
- ``DELETE /api/v1/products/{product_id}``
- ``GET /api/v1/products/tenant-catalog``
- ``POST /api/v1/products/tenant-catalog``
- ``POST /api/v1/products/tenant-catalog/bulk``
- ``PATCH /api/v1/products/tenant-catalog/{tp_id}``
- ``DELETE /api/v1/products/tenant-catalog/{tp_id}``

Auth requirements
-----------------

- All routes require Bearer token auth.
- Some catalog endpoints are role-gated for manager/admin capabilities.

Key schemas
-----------

- Request: brand/line/product create/update, tenant catalog create/update
- Response: ``BrandRead``, ``ProductLineRead``, ``ProductRead``, ``TenantProductRead``
- List responses: ``PaginatedResponse[...]`` wrappers

Common errors
-------------

- ``400 Bad Request``: invalid catalog linkage or unit metadata
- ``401 Unauthorized``: invalid token
- ``403 Forbidden``: insufficient role for mutation
- ``404 Not Found``: missing brand/line/product/tenant-product
- ``422 Unprocessable Entity``: schema validation failure

Example request/response
------------------------

.. code-block:: bash

   curl -X GET "http://localhost:8000/api/v1/products/tenant-catalog" \
     -H "Authorization: Bearer <access_token>"

.. code-block:: json

   {
     "items": [
       {
         "id": 301,
         "tenant_id": 12,
         "product_id": 41,
         "tracking_unit": "g"
       }
     ],
     "total": 1,
     "page": 1,
     "size": 20
   }
