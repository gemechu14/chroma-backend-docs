# Frontend integration: platform SUPER_ADMIN and salon roles

This document describes how the Chroma backend distinguishes **platform operators** (global catalog and tenant management) from **salon tenants** (day-to-day salon operations), and how to integrate it in a frontend.

## Concepts

| Concept | Description |
|--------|-------------|
| **Salon tenant** | A customer organization (`tenants.is_platform = false`). Has locations, customers, inventory, formulas. |
| **Platform tenant** | A single special tenant (`tenants.is_platform = true`) used only for SaaS operator accounts. |
| **SUPER_ADMIN** | User role that **must** belong to the platform tenant. Manages **global product catalog** (brands, lines, products) and **all salon tenants** (create/update/delete tenants, list users with `tenant_id`). |
| **ADMIN / MANAGER / EMPLOYEE** | Salon roles. **ADMIN** is the salon owner/manager for that tenant only. They **cannot** edit global brands/products anymore; they manage **tenant catalog** (mapping global products to their salon) and operations. |

Base URL for API (default): `/api/v1`.

## Identifying the user after login

### `POST /users/login`

Response body (`Token`):

- `access_token`, `refresh_token`, `token_type`
- **`user`** (object, recommended for routing):
  - `id`, `email`, `role`, `tenant_id`
  - **`is_platform_tenant`**: `true` only for users on the platform tenant (typically `SUPER_ADMIN`)

Example (trimmed):

```json
{
  "access_token": "...",
  "refresh_token": "...",
  "token_type": "bearer",
  "user": {
    "id": "...",
    "email": "superadmin@chroma.local",
    "role": "SUPER_ADMIN",
    "tenant_id": "...",
    "is_platform_tenant": true
  }
}
```

### `GET /users/me`

Returns `UserMeRead`: same fields as `UserRead` plus **`is_platform_tenant`**.

Use this after refresh or when the SPA reloads if you only store the token.

### JWT payload (optional decoding)

Access tokens include claims (for debugging or client-side checks):

- `sub`: user id  
- `tenant_id`  
- `role`  
- `is_platform_tenant`: boolean  

Prefer server-trusted checks: always send the Bearer token; the API enforces permissions.

## Routing UX

- If `role === "SUPER_ADMIN"` **and** `is_platform_tenant === true` → show **Platform admin** (global catalog, tenant list, cross-tenant user management).
- Otherwise → **Salon app** (inventory, customers, formulas, tenant catalog for that salon).

Do **not** send platform users to salon flows that depend on `tenant_id` being a salon (inventory, formulas, etc.) unless you implement impersonation (not in this API).

## Platform endpoints (SUPER_ADMIN)

All require `Authorization: Bearer <access_token>`.

### Global catalog (brands, product lines, products)

| Method | Path | Notes |
|--------|------|--------|
| POST | `/products/brands` | Create brand |
| PATCH | `/products/brands/{brand_id}` | Update |
| DELETE | `/products/brands/{brand_id}` | Delete |
| POST | `/products/lines` | Create product line |
| PATCH | `/products/lines/{line_id}` | Update |
| DELETE | `/products/lines/{line_id}` | Delete |
| POST | `/products/` | Create global product |
| PATCH | `/products/{product_id}` | Update |
| DELETE | `/products/{product_id}` | Delete |

Reads (GET lists) stay available to **any authenticated user** (salon staff can browse the global catalog).

Salon **ADMIN** can no longer create/update/delete global catalog entries; those return **403** unless the user is a valid platform SUPER_ADMIN.

### Tenants

| Method | Path | Notes |
|--------|------|--------|
| GET | `/tenants/` | List tenants. Query: `include_platform` (default `false`), pagination. |
| POST | `/tenants/` | Create tenant (`is_platform` only for first platform tenant; normally create salons with `is_platform: false`) |
| GET | `/tenants/{tenant_id}` | Get tenant (super admin: any id; salon user: own id only) |
| PATCH | `/tenants/{tenant_id}` | Update |
| DELETE | `/tenants/{tenant_id}` | Cannot delete platform tenant |

### Convenience

| Method | Path | Notes |
|--------|------|--------|
| GET | `/tenants/mine` | Current user’s tenant record (any authenticated user) |

### Users (cross-tenant)

| Method | Path | Notes |
|--------|------|--------|
| GET | `/users/?tenant_id=<salon_uuid>` | **Required** `tenant_id` for SUPER_ADMIN to list staff |
| POST | `/users/` | Create user; `SUPER_ADMIN` role only if caller is platform super admin and `tenant_id` is platform tenant |
| PATCH / DELETE | `/users/{user_id}` | Super admin can manage users in any tenant (with rules for SUPER_ADMIN assignment/removal) |

## Salon endpoints (non–platform flows)

### Tenant catalog (per salon)

| Method | Path | Notes |
|--------|------|--------|
| GET | `/products/tenant-catalog?tenant_id=...` | **SUPER_ADMIN must pass `tenant_id`** (salon id) to view that salon’s catalog |
| POST / PATCH / DELETE | `/products/tenant-catalog...` | **Salon ADMIN/MANAGER only**; platform SUPER_ADMIN receives **403** (use a salon account for these operations) |

### Inventory, formulas, customers

Unchanged: scoped by the authenticated user’s `tenant_id`. Platform users **should not** use these for real data (their tenant is the platform tenant).

## Bootstrap (first deployment)

If no SUPER_ADMIN exists yet:

1. Set environment variable **`PLATFORM_BOOTSTRAP_SECRET`** (string) in the server config.
2. `POST /api/v1/platform/bootstrap-super-admin` with JSON body:

```json
{
  "secret": "<same as PLATFORM_BOOTSTRAP_SECRET>",
  "tenant_name": "Chroma Platform",
  "admin_email": "ops@example.com",
  "admin_password": "min 8 chars",
  "admin_full_name": "..."
}
```

- Disabled if `PLATFORM_BOOTSTRAP_SECRET` is empty.
- Returns **409** if a SUPER_ADMIN or platform tenant already exists.

### Local / dev seed

Running `python -m src.seed.seed_data` creates a platform tenant and user:

- Email: `superadmin@chroma.local`  
- Password: `password123`  
- Role: `SUPER_ADMIN`

## Database note

Existing databases receive the **`tenants.is_platform`** column automatically on app startup (additive migration). If you use raw SQL or external tools, ensure that column exists for PostgreSQL/SQLite.

## Error codes to handle

| Code | Typical cause |
|------|----------------|
| 401 | Missing/invalid token |
| 403 | Wrong role (e.g. salon ADMIN editing global brand) |
| 400 | Missing `tenant_id` where required (e.g. super admin listing tenant-catalog or users) |
| 409 | Bootstrap conflict, duplicate platform tenant, etc. |

## Summary checklist for frontend

1. After login, read `user.role` and `user.is_platform_tenant` (or `GET /users/me`).
2. Route SUPER_ADMIN to a **platform** shell (catalog + tenants + cross-tenant users).
3. Send `Authorization: Bearer` on every request.
4. For super admin views of a salon’s catalog or users, always pass **`tenant_id`** (the salon UUID).
5. Do not use platform credentials to call salon-only mutation endpoints (tenant-catalog POST/PATCH/DELETE, locations, inventory mutations) — the API returns **403** by design.
