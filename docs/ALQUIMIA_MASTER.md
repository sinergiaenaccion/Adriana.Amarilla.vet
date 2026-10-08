# ALQUIMIA — MASTER ARCHITECTURE
Version: 1.0 · 2026-10-08

## 1. Brand architecture
- Personal brand: Adriana Amarilla, Médica Veterinaria.
- Institutional universe: Alquimia Medicina Veterinaria Integrativa.
- Audience: tutores de perros y gatos.
- Primary conversion: reservar consulta.
- Secondary conversions: comprar productos, sumarse a Comunidad Alquimia, solicitar Gift Card, participar de Empresas Amigas.
- Tone: profesional, cálido, claro, humano, premium; nunca prometer diagnósticos automáticos.

## 2. Global funnel
Atracción → Orientación → Conversión → Reserva/Compra → Confirmación → Atención/Preparación → Seguimiento → Fidelización.

## 3. Appointment engine
### User flow
1. Choose modality: presencial / virtual / domicilio.
2. Choose service.
3. Choose date.
4. Show only available slots.
5. Capture tutor + pet profile.
6. Create appointment request.
7. Central availability service locks the slot.
8. Send confirmation email.
9. Optional WhatsApp confirmation.
10. Reminder at 48h and 24 business hours.
11. Confirm / reschedule / cancel.
12. Post-consult follow-up.

### Appointment states
DISPONIBLE → SOLICITADO → CONFIRMADO → RECORDATORIO_ENVIADO → ATENDIDO → SEGUIMIENTO
Alternative: CANCELADO / REPROGRAMADO / NO_ASISTIO.

### Required pet profile
- Pet name
- Species (dog/cat)
- Approximate age
- Weight
- Main reason
- Relevant history
- Current medication
- Allergies/intolerances
- Prior studies
- Optional image/document uploads

## 4. Orientation test
The test is triage/orientation only, never a diagnosis.
Outputs:
- PRIORIDAD
- CONVIENE_CONSULTAR
- ORIENTACION_BIENESTAR
- PREVENCION

Any alarm signal must direct the user to professional/urgent veterinary care as appropriate.

## 5. Store engine
Product → Cart → Customer → Delivery/Pickup → Coupon/Gift Card → Payment → Webhook verification → Order confirmation → Preparation → Dispatch/Pickup → Delivered.

### Order states
CARRITO
PENDIENTE_DE_PAGO
PAGO_APROBADO
EN_PREPARACION
LISTO_PARA_RETIRO
DESPACHADO
EN_CAMINO
ENTREGADO
PAGO_RECHAZADO
CANCELADO
REEMBOLSADO
DEVUELTO

### Payment rule
Mercado Pago must be server-side. Never expose access tokens in the browser.
The authoritative order state must come from verified payment notification/webhook, not only from a return URL.

## 6. Delivery
Delivery methods are configurable:
- Retiro
- Envío local
- Envío nacional

The site must calculate/show cost and estimated delivery before payment. Operator/API is intentionally not hard-coded until the real logistics provider is defined.

## 7. Community / CRM
Capture:
- Name
- Email
- Pet name (optional)
- Consent
- Interests/species when available
- Origin/source
- Date joined

Welcome benefit: 10% off eligible products.
COMUNIDAD10 is a current test code only; production must validate eligibility server-side.

## 8. Gift Cards
Gift Card must become a real digital product:
- amount
- recipient
- sender
- recipient email
- unique code
- status
- expiration policy
- redemption history

## 9. Follow-up automation
After purchase:
- confirmation
- preparation/dispatch
- delivery
- post-purchase care message

After consultation:
- confirmation
- reminder
- post-consult follow-up
- suggested next control when clinically appropriate

## 10. Data model
Core entities:
clients, pets, services, availability, appointments, products, inventory, orders, order_items, payments, shipments, coupons, giftcards, communications, consents, companies, testimonials, content.

Every clinical/personal data flow must use minimum necessary data and appropriate consent/privacy controls.

## 11. API contract
Planned endpoints:
- POST /api/appointments
- GET /api/availability
- POST /api/appointment-confirm
- POST /api/appointment-cancel
- POST /api/appointment-reschedule
- POST /api/orders
- POST /api/checkout
- POST /api/payment/webhook
- GET /api/orders/:id
- POST /api/shipping
- GET /api/tracking/:id
- POST /api/community
- POST /api/newsletter
- POST /api/giftcards
- POST /api/contact

## 12. Admin
Minimum dashboard:
- today's appointments
- pending confirmations
- reminders
- clients/pets
- orders/payments
- inventory
- shipments
- community
- inquiries
- basic metrics

## 13. Production variables
Expected secrets/configuration:
- MERCADOPAGO_ACCESS_TOKEN
- MERCADOPAGO_WEBHOOK_SECRET (when applicable to the selected MP integration)
- RESEND_API_KEY
- EMAIL_FROM
- EMAIL_REPLY_TO
- CALENDAR credentials/provider
- SHIPPING provider credentials
- DATABASE_URL / selected persistence layer
- APP_BASE_URL

Never commit secrets.

## 14. Current status
The public site is a high-fidelity front-end prototype. The current booking lock is browser-local and the store checkout is WhatsApp-based test mode.
Production activation requires real persistence, calendar, email, payment and logistics credentials.
Exact catalog products/prices/images must come from Adriana's real catalog; placeholders must not be treated as inventory.

## 15. Non-negotiables
- Canonical Alquimia SVG logo.
- Real Adriana portrait when supplied.
- Real catalog data when supplied.
- No invented prices, stock, clinical claims or integrations.
- No diagnosis by the orientation test.
- No payment marked approved without server-side verification.
- No appointment marked globally unavailable using browser localStorage alone.
