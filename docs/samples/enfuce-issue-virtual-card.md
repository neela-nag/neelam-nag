# Issue a virtual card

Virtual cards are issued instantly via the Enfuce API and are ready to use for online payments or digital wallet enrolment immediately after activation. This guide walks through the complete flow from creating a customer to activating a card that your cardholder can use.

**Time to complete:** approximately 15 minutes in the sandbox.

---

## Requirements

Before you begin, you need:

- Sandbox API credentials — your private key and the sandbox base URL. If you do not have these yet, see [Authentication](#).
- Your institution ID — provided by Enfuce during onboarding.
- An account product type configured for your programme — agreed with Enfuce during the implementation project.

If you already have an existing customer and account, skip to [Step 3 — Issue the card](#step-3--issue-the-card).

---

## How it works

In Enfuce, every card is linked to both a **customer** and an **account**. You must create both before issuing a card.

- The **customer** is the cardholder entity. It holds identity data — name, address, date of birth, and registration number. Enfuce recommends that each person or entity is represented by a single customer record, even if they have multiple cards or accounts.
- The **account** holds the balance. When a cardholder makes a purchase, funds are deducted from the account the card is linked to. Several cards can share the same account — they will all draw from the same balance.
- The **card** is the payment instrument. It links to exactly one account and one customer. A virtual card has a card number (PAN), expiry date, and CVV — all available via API for display in your app.

```
Customer
  └── Account (holds balance)
        └── Card (payment instrument)
```

!!! note "Digital wallet support"
    If you plan to support Apple Pay or Google Pay, each card must have its own customer linked to it — not shared between cards. This is required for SMS OTP authentication during digital wallet enrolment.

---

## Step 1 — Create a customer

Create a customer record for the cardholder. The customer holds the identity data used for card delivery, statements, and digital wallet enrolment.

=== "Request"

    ```bash
    POST https://api-sandbox.enfuce.com/v1/customer/private

    Authorization: Basic <base64(private_key:)>
    Content-Type: application/json

    {
      "firstName": "Jane",
      "lastName": "Smith",
      "dateOfBirth": "1985-06-15",
      "locale": "en",
      "customerNumber": "CRM-00012345",
      "regNo": "150685-1234",
      "address": {
        "streetAddress": "Mannerheimintie 12",
        "postalCode": "00100",
        "city": "Helsinki",
        "countryCode": "FI"
      },
      "email": "jane.smith@example.com",
      "mobileNumber": "+358401234567"
    }
    ```

=== "Response"

    ```json
    {
      "customerId": "c-abc123def456",
      "customerNumber": "CRM-00012345",
      "firstName": "Jane",
      "lastName": "Smith",
      "status": "active",
      "createdAt": "2026-05-18T10:14:00Z"
    }
    ```

**Save the `customerId`** — you need it in Step 2.

### Required fields

| Field | Type | Description |
|---|---|---|
| `firstName` | string | Cardholder first name. Max 26 characters. |
| `lastName` | string | Cardholder last name. Max 26 characters. |
| `locale` | string | Language code, e.g. `en`, `fi`, `de`. Used for communications. |
| `customerNumber` | string | Your internal identifier for this customer, e.g. a CRM ID. |
| `regNo` | string | Registration number — for example, a social security number or VAT number. |
| `address` | object | Primary address. Used for card delivery and statements. |

---

## Step 2 — Create an account

Create an account linked to the customer. The account holds the balance and determines the product type — prepaid, credit, or charge — based on your configured account product.

=== "Request"

    ```bash
    POST https://api-sandbox.enfuce.com/v1/account

    Authorization: Basic <base64(private_key:)>
    Content-Type: application/json

    {
      "customerId": "c-abc123def456",
      "currency": "EUR"
    }
    ```

=== "Response"

    ```json
    {
      "accountId": "a-xyz789ghi012",
      "customerId": "c-abc123def456",
      "currency": "EUR",
      "status": "00",
      "statusDescription": "Account OK",
      "balance": 0.00,
      "available": 0.00,
      "createdAt": "2026-05-18T10:15:00Z"
    }
    ```

**Save the `accountId`** — you need it in Step 3.

!!! note "Account product type"
    The account product type — which determines whether this is a prepaid, credit, or charge account — is configured at the institution level during your implementation project with Enfuce. You do not specify it in the API call. Contact your Enfuce implementation manager if you are unsure which product type applies.

### Account statuses

The account status has a higher priority than the card status. If an account is blocked or closed, all linked cards are also affected.

| Code | Status | Description |
|---|---|---|
| `00` | Account OK | Open and in normal status. Transactions are processed. |
| `05` | Account Blocked | Temporary block. All linked cards are blocked for new authorisations. |
| `54` | Account To Close | Closing process started. Linked cards with status `00` are set to `05`. |
| `114` | Auto-Closed | Final termination after 32-day closing period. All linked cards are closed. |

---

## Step 3 — Issue the card

Issue a virtual card linked to the account and customer you just created. The card is created in `inactive` status and must be activated before use.

Enfuce provides separate endpoints for Visa and Mastercard branded cards. Use the endpoint that matches your programme's card scheme.

=== "Visa virtual card"

    ```bash
    POST https://api-sandbox.enfuce.com/v1/card/visa/virtual

    Authorization: Basic <base64(private_key:)>
    Content-Type: application/json

    {
      "accountId": "a-xyz789ghi012",
      "customerId": "c-abc123def456",
      "cardRole": "MAIN",
      "firstName": "Jane",
      "lastName": "Smith"
    }
    ```

=== "Mastercard virtual card"

    ```bash
    POST https://api-sandbox.enfuce.com/v1/card/mastercard/virtual

    Authorization: Basic <base64(private_key:)>
    Content-Type: application/json

    {
      "accountId": "a-xyz789ghi012",
      "customerId": "c-abc123def456",
      "cardRole": "MAIN",
      "firstName": "Jane",
      "lastName": "Smith"
    }
    ```

=== "Response"

    ```json
    {
      "cardId": "k-pqr345stu678",
      "accountId": "a-xyz789ghi012",
      "customerId": "c-abc123def456",
      "maskedCardNumber": "47231234****5678",
      "expiration": "05/2030",
      "status": "00",
      "statusDescription": "Card OK",
      "cardRole": "MAIN",
      "plastics": [
        {
          "sequenceNumber": 1,
          "status": "ACTIVE"
        }
      ],
      "createdAt": "2026-05-18T10:16:00Z"
    }
    ```

**Save the `cardId`** — you need it in Step 4.

### Card statuses

| Code | Status | Description |
|---|---|---|
| `00` | Card OK | Card is open. Authorisations are processed normally. |
| `05` | Card Blocked | Temporary block. Authorisations are declined. Reversible. |
| `41` | Card Lost | Card is permanently closed. A replacement card can be issued. |
| `43` | Card Stolen | Card is permanently closed. A replacement card can be issued. |
| `105` | Card Closed | Closed by request. Tokens are deactivated automatically. |

---

## Step 4 — Activate the card

A newly issued virtual card is ready to use immediately — the card status is already `00` (Card OK) and the plastic status is `ACTIVE`. No separate activation call is needed for virtual cards.

!!! note "Physical cards require activation"
    Physical cards may be produced in a `LOCKED` plastic status, meaning they require explicit activation via the [Update Plastic API](#) before the cardholder can use them. Virtual cards do not have this restriction.

To verify the card is ready, retrieve it and check the `status` and `plastics[0].status`:

```bash
GET https://api-sandbox.enfuce.com/v1/card/{cardId}

Authorization: Basic <base64(private_key:)>
```

A card is ready to use when:

- `status` is `00` (Card OK)
- `plastics[0].status` is `ACTIVE`

---

## Step 5 — Display the card to your cardholder

Virtual card credentials — the full PAN, expiry date, and CVV — are PCI-protected data and are only available through the secure View Card Data API. Never store or transmit the full PAN through your own servers.

To display card details in your app:

1. Call the View Card Data API with the `cardId`.
2. Render the credentials inside a secure, Enfuce-hosted iframe — or use Enfuce's SDK for mobile apps.

See [View card data](#) for implementation details.

---

## What you have now

At the end of this guide you have:

- A customer record (`customerId`) representing your cardholder
- An account (`accountId`) holding their balance
- An active virtual card (`cardId`) ready for online purchases and digital wallet enrolment

---

## Next steps

<div class="nn-sample-grid" markdown>

<div class="nn-sample-card" markdown>
### Top up the account
Add balance so the cardholder can make purchases. [Add funds via Transfer API →](#)
</div>

<div class="nn-sample-card" markdown>
### Set up spend controls
Apply usage limits, geo blocks, or MCC blocks to control where and how the card can be used. [Spend controls →](#)
</div>

<div class="nn-sample-card" markdown>
### Enrol in a digital wallet
Allow your cardholder to add the card to Apple Pay or Google Pay. [Digital wallets →](#)
</div>

<div class="nn-sample-card" markdown>
### Receive transaction notifications
Set up webhooks to receive real-time events when the card is used. [Webhook notifications →](#)
</div>

</div>
