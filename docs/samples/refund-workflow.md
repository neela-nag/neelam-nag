# Sample — Refund workflow (Task topic)

## Context note

**Audience:** Operations staff at a fictional payments company (PayCart). Non-technical. Responsible for confirming refunds and communicating outcomes to customers.

**The problem this page solves:** When a developer issues a refund via API, the operations team needs to confirm it succeeded and notify the customer — but they have no technical background and are working under time pressure in a support context.

**Structural decisions:**
The page uses a strict task topic structure — prerequisites, numbered steps, one action per step. I separated the confirmation task (verify in dashboard) from the communication task (reply to customer ticket) so each step has exactly one output. The reader always knows what they are looking for and what success looks like.

I deliberately avoided explaining *how* the refund works technically — that is a concept topic for a different audience. This page only tells the operations user what to do, in what order, and how to know they have done it correctly.

**What I would improve:** I would add a troubleshooting section for common edge cases — for example, what to do when the refund shows Pending rather than Succeeded. That content belongs on a separate troubleshooting topic linked from Step 3.

---

## Confirm and communicate a refund

Use this task when a developer notifies you that a refund has been issued and you need to confirm it succeeded before contacting the customer.

**Before you begin**

- You have the refund ID from the developer — for example, `rfd_01HXYZ7890ABCDEF`
- You have access to the PayCart dashboard
- You have the original support ticket open

---

### Step 1 — Open the Refunds list

1. Log in to the PayCart dashboard.
2. In the left navigation, select **Payments**, then **Refunds**.

---

### Step 2 — Find the refund

1. In the search bar, enter the refund ID provided by the developer.
2. Locate the matching row in the results list.
3. Check the **Status** column.

| Status | What it means | What to do |
|---|---|---|
| **Succeeded** | Refund settled | Continue to Step 3 |
| **Pending** | Not yet settled | Wait up to 2 minutes, then refresh |
| **Failed** | Refund did not process | Do not contact the customer — escalate to the developer |

!!! note
    A **Pending** status is normal immediately after a refund is issued. Only escalate if the status has not changed to **Succeeded** after 5 minutes.

---

### Step 3 — Verify the linked payment

1. In the refund row, click the linked **Payment ID**.
2. On the payment detail panel, check:
    - **Status** shows **Partially refunded** or **Refunded**
    - **Total refunded** matches the expected amount
    - The refund ID appears in the **Refunds** section at the bottom

!!! warning
    If the amounts do not match, do not contact the customer. Return the ticket to the developer with a note explaining the discrepancy.

---

### Step 4 — Reply to the customer ticket

Once you have confirmed the refund succeeded, reply to the customer's support ticket.

Include all of the following in your reply:

- The refund amount in euros
- The last four digits of the card the refund was issued to
- The refund ID
- The standard settlement timeframe (3–10 business days)

**Example reply:**

> Hi [Customer name],
>
> We have processed a refund of **€[amount]** to your [card type] card ending in **[last four digits]**.
>
> Your refund reference is: `rfd_01HXYZ7890ABCDEF`
>
> Please allow 3–10 business days for the funds to appear on your statement, depending on your bank.
>
> Kind regards,
> [Your name]

---

### Step 5 — Close the ticket

Set the ticket status to **Refund issued** and close.

---

*[Back to Writing Samples](../)*
