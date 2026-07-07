Places API
==========

Purpose
-------

Provides address autocomplete for location entry using pluggable providers (Photon by default, optional Google Places).

Endpoint list
-------------

- ``GET /api/v1/places/autocomplete``

Auth requirements
-----------------

- Current implementation is public and intended for low-risk UX enhancement.
- If exposed publicly, enforce edge-level abuse controls.

Key schemas
-----------

- Request query: search text, optional geographic hints
- Response: ``PlaceAutocompleteResponse``

Common errors
-------------

- ``400 Bad Request``: missing/invalid query parameters
- ``429 Too Many Requests``: provider or gateway throttle
- ``502 Bad Gateway``: upstream places provider unavailable

Example request/response
------------------------

.. code-block:: bash

   curl -G "http://localhost:8000/api/v1/places/autocomplete" \
     --data-urlencode "q=Main Street 42"

.. code-block:: json

   {
     "results": [
       {
         "formatted": "42 Main Street, Springfield",
         "lat": 40.12,
         "lng": -74.01
       }
     ]
   }
