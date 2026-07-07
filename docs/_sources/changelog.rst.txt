Changelog
=========

This document provides a high-level summary of notable changes to the Dosecolor backend.

Versions are illustrative; consult Git history and release tags for exact implementation details.

1.0.0 (Initial Public Release)
------------------------------

Added
~~~~~

- Core authentication and account lifecycle flows.
- Multi-tenant tenant/location/user/customer APIs.
- Product catalog, inventory, and formula workflow endpoints.
- Stripe checkout, portal access, webhook handling, and subscription sync.
- Initial Sphinx documentation set for operations and API usage.

Changed
~~~~~~~

- N/A for initial release.

Fixed
~~~~~

- N/A for initial release.

Security
~~~~~~~~

- Baseline JWT-protected routes with tenant-scoped authorization checks.

1.x (Ongoing Enhancements)
--------------------------

Added
~~~~~

- Expanded endpoint-level documentation by functional API group.
- Improved operations guidance for migrations, monitoring, and incidents.
- Additional examples for common request/response payloads.

Changed
~~~~~~~

- Refined navigation structure and section hierarchy in Sphinx docs.

Fixed
~~~~~

- Minor clarity fixes in setup and configuration documentation.

Security
~~~~~~~~

- Added explicit hardening guidance for CORS, secrets rotation, and abuse controls.

Future Changes
--------------

Planned improvements include:

- Auto-generated schema tables per endpoint from OpenAPI.
- Expanded deployment runbooks for Docker and Kubernetes environments.
- More detailed troubleshooting playbooks for external integrations.
