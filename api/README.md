# Alquimia API — production contract

This folder is reserved for Vercel serverless endpoints.

The front-end currently calls:
- /api/appointments
- /api/community

Those calls are intentionally non-authoritative in test mode.

Before production, endpoints must be backed by persistent storage and validated server-side.

## Security
- Validate request bodies.
- Rate-limit public endpoints.
- Never trust client-side prices, discounts or appointment availability.
- Never expose payment/email/calendar secrets.
- Verify Mercado Pago webhook authenticity and fetch/verify the payment server-side.
- Store only the minimum personal/clinical data needed for the requested operation.

## Appointment requirement
A successful request must perform an atomic availability check and reservation/lock in the database. localStorage is not a production availability mechanism.

## Order requirement
The browser creates an order intent. The server calculates totals from the catalog, applies validated coupons, creates the payment order/preference and persists the pending order. Only a verified payment notification can transition the order to PAGO_APROBADO.

## Email requirement
Transactional emails should be sent from the server after a persisted state transition, with idempotency to avoid duplicates.

## Calendar requirement
The calendar provider is the source of truth for availability once connected. A confirmed appointment must be synchronized back to the site's appointment record.

## Current implementation boundary
No production credentials or provider identifiers are committed to this repository.
