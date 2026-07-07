Stripe Billing API
==================

Purpose
-------

Handles Stripe checkout session generation, customer portal access, webhook ingestion, and subscription state synchronization.

Endpoint list
-------------

- ``POST /api/v1/stripe/checkout-session``
- ``POST /api/v1/stripe/customer-portal``
- ``POST /api/v1/stripe/webhook``
- ``POST /api/v1/stripe/sync-subscription``

Auth requirements
-----------------

- Checkout, portal, and sync endpoints require Bearer token auth.
- Webhook endpoint is public but must validate ``Stripe-Signature`` with webhook secret.

Key schemas
-----------

- Request: checkout/session and portal payloads, sync trigger payload
- Response: ``StripeCheckoutResponse``, ``StripePortalResponse``, ``TenantSubscriptionRead``

Common errors
-------------

- ``400 Bad Request``: malformed Stripe payload or invalid request body
- ``401 Unauthorized``: missing token for protected endpoints
- ``403 Forbidden``: tenant-level billing access denied
- ``404 Not Found``: tenant subscription/customer linkage missing
- ``422 Unprocessable Entity``: schema validation failure

Example request/response
------------------------

.. code-block:: bash

   curl -X POST "http://localhost:8000/api/v1/stripe/checkout-session" \
     -H "Authorization: Bearer <access_token>" \
     -H "Content-Type: application/json" \
     -d '{"interval":"monthly","success_url":"https://app.example/success","cancel_url":"https://app.example/cancel"}'

.. code-block:: json

   {
     "checkout_url": "https://checkout.stripe.com/c/pay/..."
   }
