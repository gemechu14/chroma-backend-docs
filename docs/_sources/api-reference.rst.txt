API Reference
=============

Overview
--------

Base URL:

- ``/api/v1`` for business endpoints
- ``/health`` for service liveness

Authentication model:

- Most endpoints require ``Authorization: Bearer <access_token>``.
- Public endpoints exist for onboarding, auth bootstrap, plans catalog, and selected webhook flows.

API groups
----------

.. toctree::
   :maxdepth: 1

   auth-api
   users-api
   tenants-api
   customers-api
   products-api
   inventory-api
   formulas-api
   platform-api
   plans-api
   stripe-api
   places-api
