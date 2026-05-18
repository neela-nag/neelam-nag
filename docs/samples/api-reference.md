# Sample — API reference (Reference topic)

## Context note

**Audience:** Developers integrating the PayCart payment API. They know how REST APIs work. They are here to look something up — not to learn.

**The problem this page solves:** Developers need a fast, scannable reference for a specific endpoint. They are not reading this linearly — they are searching for a parameter name, checking a required field, or verifying an error code. Every second they spend looking is friction.

**Structural decisions:**
This is a pure reference topic — no task steps, no conceptual explanation. I used tables for parameters rather than prose because tables are scannable. Required and optional parameters are clearly marked. Request and response examples are shown together so the developer can see the full round-trip in one view.

I kept the endpoint description to one sentence. The developer already knows what a charge is — they are here for the parameters, not the definition.

**What I would improve:** I would add a code tab section with examples in multiple languages (cURL, PHP, Python, Java) — the most common SDK languages for payments APIs. A single JSON example limits the usefulness for developers using SDK integrations.

---

## POST /v1/payments/charge

Creates a charge transaction against a payment type.

**Base URL:** `https://api.paycart.example/v1`

**Authentication:** Basic authentication using your private key as the username. No password required.

```
Authorization: Basic <base64(private_key:)>
```

---

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `amount` | integer | Yes | The charge amount in the smallest currency unit. For EUR, this is cents — `2500` = €25.00 |
| `currency` | string | Yes | Three-letter ISO 4217 currency code. Example: `EUR` |
| `paymentTypeId` | string | Yes | The ID of the payment type resource to charge. Example: `s-crd-abc123` |
| `orderId` | string | No | Your internal order reference. Returned in the response for reconciliation |
| `customerId` | string | No | ID of an existing customer resource. Required for Invoice and Direct Debit payment types |
| `basketId` | string | No | ID of an existing basket resource. Required for Instalment payment types |
| `returnUrl` | string | Conditional | The URL to redirect the customer to after payment completion. Required for redirect payment types |
| `description` | string | No | A short description of the charge, visible in the dashboard |

---

### Example request

```bash
curl -X POST https://api.paycart.example/v1/payments/charge \
  -u s-priv-xxxxxxxxxx: \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 2500,
    "currency": "EUR",
    "paymentTypeId": "s-crd-abc123xyz",
    "orderId": "order-7712",
    "returnUrl": "https://yourshop.example/payment/return"
  }'
```

---

### Example response

```json
{
  "id": "s-pay-1a2b3c4d5e",
  "orderId": "order-7712",
  "status": "success",
  "amount": 2500,
  "currency": "EUR",
  "paymentTypeId": "s-crd-abc123xyz",
  "transactions": [
    {
      "id": "s-chg-001",
      "type": "charge",
      "status": "success",
      "amount": 2500,
      "currency": "EUR",
      "date": "2026-04-12T09:14:00Z"
    }
  ]
}
```

---

### Response properties

| Property | Type | Description |
|---|---|---|
| `id` | string | Unique payment ID. Use this to fetch or manage the payment later |
| `orderId` | string | Your order reference, echoed from the request |
| `status` | string | Payment status. See [Payment statuses](#payment-statuses) |
| `amount` | integer | Charged amount in the smallest currency unit |
| `currency` | string | Three-letter currency code |
| `transactions` | array | List of transactions associated with this payment |

---

### Payment statuses

| Status | Description |
|---|---|
| `success` | Payment completed successfully |
| `pending` | Payment initiated but not yet settled |
| `failed` | Payment was not completed |
| `cancelled` | Payment was cancelled before completion |

---

### Error codes

| HTTP status | Error code | Description | Resolution |
|---|---|---|---|
| `400` | `amount_invalid` | Amount is zero, negative, or not an integer | Check the amount value — must be a positive integer |
| `400` | `currency_unsupported` | Currency code is not supported | Use a supported ISO 4217 currency code |
| `400` | `payment_type_not_found` | The `paymentTypeId` does not exist | Verify the ID — it may have expired |
| `401` | `unauthorized` | Authentication failed | Check your private key |
| `422` | `customer_required` | `customerId` is required for this payment type | Create a customer resource first |
| `500` | `internal_error` | Unexpected server error | Retry. If persistent, contact support |

---

### Related endpoints

- `GET /v1/payments/{id}` — Fetch payment status
- `POST /v1/payments/{id}/cancel` — Cancel or refund a payment
- `GET /v1/payment-types/{id}` — Retrieve a payment type

---

*[Back to Writing Samples](../)*
