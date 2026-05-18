# Enfuce — Documentation Audit and IA Proposal

<div class="nn-hero">
  <span class="nn-role">Unsolicited Documentation Audit · May 2026</span>
  <h1>From fragmented guides to a developer-first documentation system</h1>
  <p>A complete audit of Enfuce's developer documentation — identifying structural problems, proposing a new information architecture based on leading issuer documentation patterns, and demonstrating the rewrite with a full sample integration guide.</p>
</div>

---

## Context

Enfuce is a Finnish card issuer and payment processor — one of Europe's leading card programme platforms, processing payments for fintechs, banks, and embedded finance companies across the EEA and UK.

I conducted this audit independently as a portfolio piece, applying the same methodology I used at Unzer: reading every accessible page, mapping what exists, identifying gaps, and proposing a restructured information architecture grounded in actual content — not invented.

This is not a theoretical exercise. Every problem identified is based on pages I actually read. Every proposed page in the new IA corresponds to content that exists in the current docs and needs restructuring, or content that is genuinely missing.

---

## What I read

Before proposing any changes, I systematically read the following pages:

- Homepage and API Overview
- Guides: Quick guide, Start developing with Enfuce
- The essentials: BIN sponsorship, Introduction to transactions, Financial transaction, Authorisation and authentication
- Issue any card: Card (full — very detailed), Disposable Cards, Customer, Account
- The control you want: Authorisation control
- Peace of mind: 3DS authentication
- Release Notes (full — going back to 2022)

I also reviewed established issuer documentation structures and partner style guides — which define required page structure conventions and informed the proposed IA.

---

## Six problems found

<div class="nn-decision">
  <span class="nn-decision-label">Problem 1</span>
  <h4>No getting started path</h4>
  <p>The homepage offers three equal-weight cards: API Docs, Guides, Release Notes. There is no "new here?" path, no quickstart, and no first API call guide. A developer who arrives ready to build has nowhere clear to go. The Guides section opens with a marketing introduction — "nice to meet you!" — not a developer workflow.</p>
</div>

<div class="nn-decision">
  <span class="nn-decision-label">Problem 2</span>
  <h4>API overview is an unlabelled list of 18 names</h4>
  <p>The Payment API overview page contains one sentence of description and a list of 18 API section names with no descriptions, no relationships, and no guidance on which ones to use for which use case. A developer cannot make a single decision without clicking into every section individually.</p>
</div>

<div class="nn-decision">
  <span class="nn-decision-label">Problem 3</span>
  <h4>No data model — the Customer → Account → Card relationship is invisible</h4>
  <p>The Card page, Customer page, and Account page are all separate concept documents with no task flow connecting them. A developer who wants to issue a card must discover independently that they need a customer first, then an account, then a card — in that order. No single page explains this sequence or shows how the entities relate.</p>
</div>

<div class="nn-decision">
  <span class="nn-decision-label">Problem 4</span>
  <h4>Pages mix concept, task, and reference content without separation</h4>
  <p>The Card page is 11+ sections mixing what cards are (concept), how to create them (task), card statuses (reference), plastic statuses (reference), PIN creation (task), embossing data (reference), and card renewal (task) — all on one scrolling page with no clear separation. A developer looking up a card status code has to scroll through embossing data to find it.</p>
</div>

<div class="nn-decision">
  <span class="nn-decision-label">Problem 5</span>
  <h4>Voice inconsistency signals ungoverned content growth</h4>
  <p>The guides section uses casual marketing language ("card issuer's guide to happiness", "nice to meet you!"). The Card page is dense and technical. The Authorisation control page is professional and detailed. These read like three different teams with no shared style guide — which is exactly what developers notice and use to judge documentation trustworthiness.</p>
</div>

<div class="nn-decision">
  <span class="nn-decision-label">Problem 6</span>
  <h4>Excellent release notes — completely unlinked from API pages</h4>
  <p>The release notes are detailed, well-structured, and go back to 2022. They are also completely isolated. There is no link from any API endpoint page to relevant release note entries. A developer looking at the Card API has no way to know what changed in the last three months without navigating to a separate page.</p>
</div>

---

## The approach — patterns from leading issuer documentation

Before proposing changes, I studied how leading issuer platforms structure their card issuing documentation. Two patterns stand out.

**The flat pattern** uses a task flow with no section groupings. Concepts are embedded at the top of task pages — two or three paragraphs explaining what you need to know before the steps begin. There is no separate Concepts section. Disputes, spend controls, and fraud management all sit at the same level as "Issue a card" — they are all task flows, just different ones.

**The grouped pattern** uses task flows with section labels, which is a better fit for Enfuce's complexity. Every page follows a fixed structure: Intro → Requirements → Concept (if needed) → Task → Reference → Exit. The Intro and Exit blocks are mandatory on every page. This pattern also includes a master "Integration and go-live checklist" — a sequential page that covers the full journey from test setup to going live.

**The key principle from both:** concepts belong inside task pages at the point where the developer needs them — not in a separate section they have to navigate away from the task to read. A developer following "Issue a virtual card" needs to understand the Customer → Account → Card relationship — but they need it on that page, not on a separate Concepts page.

---

## Proposed information architecture

The proposed IA keeps the existing content groupings — which are not wrong — but renames them to be task-oriented and restructures what is inside each group.

### Navigation structure

```
Top navigation: Guides | API reference | Release notes

OVERVIEW
├── How Enfuce works          ← NEW: data model + platform diagram
├── Authentication            ← NEW: API keys, sandbox URL, auth pattern  
├── Sandbox + test data       ← NEW: test cards, simulate events
├── Start developing          ← REWRITE: true developer quickstart
└── Integration checklist     ← NEW: master go-live checklist

SET UP YOUR PROGRAMME
├── BIN sponsorship           ← REWRITE: task flow, concept embedded
├── Onboard a customer        ← NEW: merges Customer + Account pages
│   ├── Create a customer
│   ├── Create an account
│   ├── Account types
│   └── Account hierarchy
└── Credit solution + ledger  ← KEEP: existing section

ISSUE ANY CARD
├── Issue a virtual card      ← NEW: full task flow (see sample below)
├── Issue a physical card     ← REWRITE: Card page restructured
├── Issue a disposable card   ← REWRITE: existing page, rewritten
├── Issue an All-in-One card  ← REWRITE: existing page, rewritten
├── Manage card lifecycle     ← NEW: merges card statuses, renewal, replacement
├── PIN management            ← REWRITE: extract from Card page
├── Digital-first experience  ← KEEP: existing section
│   ├── View card data
│   ├── View + change PIN
│   ├── Digital wallets
│   └── Flexible card artwork
└── Card Management APIs      ← KEEP: existing section

PROCESS PAYMENTS
├── How authorisation works   ← REWRITE: Introduction to transactions
├── Authorisation control     ← REWRITE: existing page, task flow
├── Financial transactions    ← REWRITE: existing page, restructured
├── Webhook notifications     ← NEW: dedicated page for notifications
├── 3DS authentication        ← MOVE: from Peace of mind
├── Fraud + dispute management← MOVE: from Peace of mind
├── Discounts                 ← KEEP: move from Global processing
├── Purchase details          ← KEEP: move from Global processing
└── Transfer API              ← KEEP: move from Global processing

CONTROL SPENDING
├── Spend controls            ← REWRITE: existing section
│   ├── Geo blocks
│   ├── MCC blocks
│   └── Usage limiters

PROGRAMME MANAGEMENT
├── Customer portal (MyEnfuce)← KEEP: existing section
├── Card programme insights   ← MOVE: from The control you want
├── MyApp — cardholder app    ← KEEP
├── MyCard — card bureau      ← KEEP
├── Partnerships              ← KEEP
└── Customer success          ← KEEP

COMPLIANCE
├── Global compliance         ← KEEP: existing
├── PCI DSS                   ← NEW
└── PSD2                      ← NEW

REFERENCES                    ← NEW section
├── Transaction codes         ← NEW: extracted from Financial transaction page
├── Response codes            ← NEW: extracted from existing content
├── Verification codes        ← NEW
└── Glossary                  ← NEW

API REFERENCE                 ← KEEP: regroup by domain
Cards · Accounts · Customers · Transactions · Authorisation
Notifications · PIN · Transfers · Spend controls · Test API

RELEASE NOTES                 ← KEEP: standalone, link from API pages
```

---

## What changes and why — five structural decisions

### Decision 1 — No separate Concepts section

The previous version of this IA had a Concepts column. I removed it after studying multiple issuer documentation platforms. The strongest examples do not have a separate Concepts section. Concepts live inside task pages at the point where the developer needs them — typically two to three paragraphs before Step 1 begins. The one exception is "How Enfuce works" — a single platform overview page with the data model diagram, because every other page references it.

### Decision 2 — Grouped sections, not a flat list

A flat navigation with no group headings works for issuer products that are relatively self-contained. Enfuce has more complexity — multiple card types, a credit solution, a customer portal, compliance requirements, and a partner ecosystem. A grouped structure ("Set up your programme", "Issue any card", "Process payments") makes the navigation scannable at Enfuce's scale.

### Decision 3 — A consistent page structure on every page

Every page follows a required structure: Intro (required) → Requirements (required) → Concept (optional, embedded) → Task steps (optional) → Reference tables (optional, inline) → Exit (required). The Intro is one sentence. The Exit tells you what you achieved and links to the next sequential page. This structure makes every page self-contained — a developer never has to leave a page to find what they need.

### Decision 4 — References as a standalone section

Transaction codes and response codes exist in the current documentation — buried inside the Financial Transaction page. They are too long to be comfortably inline in every task page that references them. Mature issuer documentation typically separates these into a References section for exactly this reason. A standalone References section with transaction codes, response codes, verification codes, and a glossary gives developers a fast lookup without breaking the narrative of the task pages.

### Decision 5 — Integration checklist as the go-live guide

A master integration checklist — a sequential page covering every step from sandbox credentials to live deployment — is among the most-used pages in mature issuer documentation. Enfuce has no equivalent. This single page would reduce implementation support queries significantly — developers know exactly what they have done and what they still need to do.

---

## Sample page — Issue a virtual card

The page below demonstrates how one integration guide would look in the new structure, following the proposed page anatomy and grounded in the actual Enfuce API.

[View sample: Issue a virtual card →](../samples/enfuce-issue-virtual-card.md)

The sample demonstrates:

- The six-block page structure in practice
- Concepts embedded at the point of need — not in a separate section
- Accurate field names and API endpoints from the real Enfuce documentation
- Separate tabs for Visa and Mastercard endpoints — reflecting a real Enfuce pattern
- Reference tables inline below the relevant step
- An Exit block with sequential next steps

---

## What this audit demonstrates

This audit is a demonstration of how I would approach the first month in the Enfuce Technical Writer role — read everything, map what exists, identify structural problems, research how comparable companies solve the same problems, and propose a grounded IA before writing a single new page.

The most important part of a documentation restructure is not the writing. It is the governance decisions — what belongs where, how content is organised, how shared content is maintained, how different audiences are served by the same navigation. Those decisions, made correctly at the start, determine whether the documentation works. The writing is what makes it readable.

---

*This audit was conducted in May 2026 as a portfolio piece. All observations are based on publicly accessible pages at docs.enfuce.com.*

*[← Back to portfolio home](../index.md)*
