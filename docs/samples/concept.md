# Sample — Understanding payment flows (Concept topic)

## Context note

**Audience:** Merchants and business stakeholders deciding how to configure their payment setup. They understand business concepts but not API mechanics. They need to make a decision — charge or authorise — and need to understand the implications before involving their developer.

**The problem this page solves:** The difference between charge and authorise transactions is a common source of merchant confusion and downstream integration errors. Merchants who do not understand the distinction configure the wrong flow, causing either premature charges or uncaptured authorisations that expire and are never settled.

**Structural decisions:**
This is a pure concept topic — no API calls, no code, no task steps. I used a real-world analogy first because the technical distinction only makes sense once the concept is grounded in something familiar. The comparison table gives merchants a quick way to decide which flow suits their business without reading the full explanation.

I deliberately did not link to the API reference from within the body of this page — that link belongs at the end, as a clear next step for readers who are ready to move from understanding to implementation.

**What I would improve:** I would add a short decision tree — "If you ship physical goods, use authorise. If you deliver digital content immediately, use charge" — to help merchants who are still uncertain after reading.

---

## Charge and authorise — understanding the difference

When a customer pays on your website, PayCart does one of two things with their money: it either takes it immediately, or it reserves it first and takes it later. Understanding which approach is right for your business prevents problems before they happen.

---

## The analogy

Think of a hotel check-in.

When you check in, the hotel does not charge your card for the full stay immediately. Instead, they place a **hold** on a certain amount — enough to cover your room and potential extras. The money is reserved from your available balance, but it has not left your account. When you check out, the hotel **captures** the actual amount owed. If you stayed fewer nights than planned, the hold is released and you are only charged for what you used.

That is an **authorise then capture** flow.

At a coffee shop, the opposite happens. You order, you pay, the money leaves your account immediately. No hold, no capture — just a direct **charge**.

PayCart supports both.

---

## Charge

A charge transaction takes the payment immediately and in full.

**Use charge when:**

- You deliver the product or service immediately — digital downloads, streaming access, instant bookings
- You do not need to adjust the amount after the customer pays
- Simplicity matters more than flexibility

**What happens:**

1. Customer completes payment
2. Funds move immediately from the customer's account to your settlement balance
3. The transaction is complete — no further action required

---

## Authorise

An authorise transaction reserves the payment amount on the customer's account without taking the funds. You capture the reserved amount later when you are ready to settle.

**Use authorise when:**

- You ship physical goods and want to confirm availability before charging
- The final amount may differ from the initial order — for example, variable shipping costs
- You want to verify the customer's payment method before fulfilling the order

**What happens:**

1. Customer completes payment — funds are reserved, not taken
2. You fulfil the order
3. You capture the amount — funds move to your settlement balance
4. Authorisations that are not captured within a set period expire automatically

!!! warning "Authorisation expiry"
    Authorisations expire if not captured within the window set by the card network — typically 7 to 30 days depending on the card type. An expired authorisation cannot be captured. If this happens, the customer must pay again.

---

## Comparison

| | Charge | Authorise |
|---|---|---|
| **When funds move** | Immediately | When you capture |
| **Best for** | Digital goods, services | Physical goods, variable amounts |
| **Can be cancelled?** | Partial refund only | Full release before capture |
| **Expiry risk** | None | Yes — if not captured in time |
| **Complexity** | Lower | Higher — requires a capture step |

---

## Which should you use?

If your business delivers something immediately and the amount is fixed — use **charge**.

If you need time between the order and the payment, or if the final amount might change — use **authorise**.

When in doubt, ask your developer. The choice affects how your integration is built, and changing it later requires code changes.

---

**Ready to implement?** See the API reference for [POST /v1/payments/charge](api-reference.md) and POST /v1/payments/authorise.

---

*[Back to Writing Samples](../)*
