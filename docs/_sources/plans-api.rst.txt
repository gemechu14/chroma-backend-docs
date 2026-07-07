Plans API
=========

Purpose
-------

Publishes available subscription catalog details for client apps during onboarding and billing selection.

Endpoint list
-------------

- ``GET /api/v1/plans``

Auth requirements
-----------------

- Public endpoint; no Bearer token required.

Key schemas
-----------

- Response: ``SubscriptionCatalogPublic``

Common errors
-------------

- ``500 Internal Server Error``: plan seed/config data unavailable
- ``422 Unprocessable Entity``: response serialization mismatch

Example request/response
------------------------

.. code-block:: bash

   curl -X GET "http://localhost:8000/api/v1/plans"

.. code-block:: json

   {
     "plans": [
       {
         "interval": "monthly",
         "price": 29.0,
         "currency": "USD"
       }
     ]
   }
