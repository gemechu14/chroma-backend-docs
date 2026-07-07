Inventory API
=============

Purpose
-------

Tracks on-hand stock and inventory transactions in grams/mL, including low-stock views, reorder workflow, and manual adjustments.

Endpoint list
-------------

- ``GET /api/v1/inventory/items``
- ``GET /api/v1/inventory/items/low-stock``
- ``GET /api/v1/inventory/items/reorder-list``
- ``POST /api/v1/inventory/items``
- ``GET /api/v1/inventory/items/{item_id}``
- ``PATCH /api/v1/inventory/items/{item_id}``
- ``POST /api/v1/inventory/items/{item_id}/mark-ordered``
- ``DELETE /api/v1/inventory/items/{item_id}/mark-ordered``
- ``GET /api/v1/inventory/transactions``
- ``GET /api/v1/inventory/transactions/{item_id}``
- ``POST /api/v1/inventory/transactions``

Auth requirements
-----------------

- All routes require Bearer token auth.
- Tenant scoping is enforced for both stock records and transactions.

Key schemas
-----------

- Request: ``InventoryItemCreate``, ``InventoryItemUpdate``, ``InventoryTxnCreate``
- Response: ``InventoryItemRead``, ``InventoryTxnRead``, ``PaginatedResponse[InventoryTxnRead]``

Common errors
-------------

- ``400 Bad Request``: invalid transaction direction, quantity, or unit mismatch
- ``401 Unauthorized``: token missing/invalid
- ``403 Forbidden``: insufficient role for stock mutation
- ``404 Not Found``: inventory item not found in tenant/location scope
- ``422 Unprocessable Entity``: invalid schema fields

Example request/response
------------------------

.. code-block:: bash

   curl -X POST "http://localhost:8000/api/v1/inventory/transactions" \
     -H "Authorization: Bearer <access_token>" \
     -H "Content-Type: application/json" \
     -d '{"inventory_item_id":42,"txn_type":"adjustment","qty_delta":15.0,"note":"Cycle count correction"}'

.. code-block:: json

   {
     "id": 991,
     "inventory_item_id": 42,
     "txn_type": "adjustment",
     "qty_delta": 15.0,
     "note": "Cycle count correction"
   }
