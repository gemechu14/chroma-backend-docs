Getting Started
===============

Prerequisites
-------------

- Python 3.11+
- ``pip`` and virtual environment support
- PostgreSQL for production-like testing (SQLite is supported for local development)
- Optional integrations: Google OAuth client, SMTP server, Stripe account

Local setup
-----------

.. code-block:: bash

   git clone <repo-url>
   cd "Chroma backend"
   python -m venv venv

   # Windows
   venv\Scripts\activate

   # macOS/Linux
   # source venv/bin/activate

   pip install -r requirements.txt
   copy env.example .env
   # edit .env values

   alembic upgrade head
   uvicorn src.main:app --reload

Environment variables
---------------------

Minimum required for secure local use:

- ``DATABASE_URL``
- ``SECRET_KEY``
- ``ACCESS_TOKEN_EXPIRE_MINUTES``
- ``REFRESH_TOKEN_EXPIRE_DAYS``

Common optional variables:

- Google OAuth: ``GOOGLE_CLIENT_ID``, ``GOOGLE_CLIENT_SECRET``, ``GOOGLE_REDIRECT_URI``
- SMTP/email: ``SMTP_SERVER``, ``SMTP_PORT``, ``SMTP_USER``, ``SMTP_PASSWORD``, ``MAIL_FROM``
- Stripe: ``STRIPE_SECRET_KEY``, ``STRIPE_WEBHOOK_SECRET``, ``STRIPE_PRICE_*``
- Places provider: ``PLACES_PROVIDER``, ``PHOTON_API_BASE``, ``GOOGLE_PLACES_API_KEY``

Run and test commands
---------------------

Run API (dev):

.. code-block:: bash

   uvicorn src.main:app --reload

Run database migrations:

.. code-block:: bash

   alembic upgrade head

Run seed data (optional):

.. code-block:: bash

   python -m src.seed.seed_data

Run tests (if pytest suite is present):

.. code-block:: bash

   pytest -q

Open API docs UI
----------------

- Swagger UI: ``http://localhost:8000/swagger``
- ReDoc: ``http://localhost:8000/redoc``
- OpenAPI JSON: ``http://localhost:8000/openapi.json``
- Health endpoint: ``http://localhost:8000/health``
