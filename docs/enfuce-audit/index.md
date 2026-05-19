# Enfuce — Documentation Audit and IA Proposal

<div class="nn-hero">
  <span class="nn-role">Unsolicited Documentation Audit · May 2026</span>
  <h1>From fragmented guides to a developer-first documentation system</h1>
  <p>I read through Enfuce's developer documentation. This page is what came out of that read: where I think the docs already work, where they could be stronger, a proposed information architecture borrowing from issuer docs that are already doing this well, and a sample integration guide to make the rewrite concrete.</p>
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

## Where I'd focus first

Reading the docs end to end, a few themes kept coming back. Each one is something I'd want to work through with the team, in roughly the order below.

| Area | What's there today | Where it could go |
| --- | --- | --- |
| **Getting started** | The homepage offers three equal-weight cards (API Docs, Guides, Release Notes). There's no quickstart or first API call path, and the Guides section opens with a marketing welcome rather than a developer workflow. | A clear "start here" route for developers arriving ready to build: quickstart, first API call, sandbox setup, and a single entry point that doesn't ask them to pick between three equal options. |
| **API overview** | The Payment API overview lists 18 section names with one sentence of description and no relationships between them. A developer can't make a choice without clicking into every section individually. | A short orientation that explains what each section is for and which one fits which use case, before the developer commits to clicking. |
| **The data model** | Card, Customer, and Account live as separate concept pages. The order in which a developer needs them (customer first, then account, then card) isn't documented in one place. | A single "How Enfuce works" page with the entity relationships and a platform diagram. Every other page links back to it rather than re-explaining it. |
| **Page anatomy** | The Card page covers 11+ sections — what cards are, how to create them, statuses, PIN creation, embossing data, renewal — on one scrolling page. Card status codes sit below embossing data. | A consistent page structure across the docs that separates "what it is" from "how to do it" from "look this up later," so each page is scannable on its own. |
| **Voice consistency** | The Guides section is casual ("nice to meet you!"), the Card page is dense and technical, and Authorisation control is professional and measured. The shift between them is noticeable enough that a reader registers it. | A shared editorial baseline so the docs sound like one team across every page, regardless of who's writing in any given week. |
| **Release notes linking** | The release notes are detailed, well-structured, and go back to 2022. They're also unlinked — no API endpoint page references them, so a developer doesn't see what changed recently without leaving the page they're on. | Inline links from each endpoint to the relevant release note entries, so recent changes meet the developer where they already are. |

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

**Top navigation:** Guides · API reference · Release notes

<div class="nn-ia-grid" markdown>

<div class="nn-ia-section" markdown>
##### Overview
- **How Enfuce works** <span class="nn-ia-tag new">NEW</span> — data model + platform diagram
- **Authentication** <span class="nn-ia-tag new">NEW</span> — API keys, sandbox URL, auth pattern
- **Sandbox + test data** <span class="nn-ia-tag new">NEW</span> — test cards, simulate events
- **Start developing** <span class="nn-ia-tag rewrite">REWRITE</span> — true developer quickstart
- **Integration checklist** <span class="nn-ia-tag new">NEW</span> — master go-live checklist
</div>

<div class="nn-ia-section" markdown>
##### Set up your programme
- **BIN sponsorship** <span class="nn-ia-tag rewrite">REWRITE</span> — task flow, concept embedded
- **Onboard a customer** <span class="nn-ia-tag new">NEW</span> — merges Customer + Account pages
    - Create a customer
    - Create an account
    - Account types
    - Account hierarchy
- **Credit solution + ledger** <span class="nn-ia-tag keep">KEEP</span>
</div>

<div class="nn-ia-section" markdown>
##### Issue any card
- **Issue a virtual card** <span class="nn-ia-tag new">NEW</span> — full task flow (see sample below)
- **Issue a physical card** <span class="nn-ia-tag rewrite">REWRITE</span> — Card page restructured
- **Issue a disposable card** <span class="nn-ia-tag rewrite">REWRITE</span>
- **Issue an All-in-One card** <span class="nn-ia-tag rewrite">REWRITE</span>
- **Manage card lifecycle** <span class="nn-ia-tag new">NEW</span> — merges card statuses, renewal, replacement
- **PIN management** <span class="nn-ia-tag rewrite">REWRITE</span> — extract from Card page
- **Digital-first experience** <span class="nn-ia-tag keep">KEEP</span>
    - View card data
    - View + change PIN
    - Digital wallets
    - Flexible card artwork
- **Card Management APIs** <span class="nn-ia-tag keep">KEEP</span>
</div>

<div class="nn-ia-section" markdown>
##### Process payments
- **How authorisation works** <span class="nn-ia-tag rewrite">REWRITE</span> — Introduction to transactions
- **Authorisation control** <span class="nn-ia-tag rewrite">REWRITE</span> — task flow
- **Financial transactions** <span class="nn-ia-tag rewrite">REWRITE</span> — restructured
- **Webhook notifications** <span class="nn-ia-tag new">NEW</span> — dedicated page for notifications
- **3DS authentication** <span class="nn-ia-tag move">MOVE</span> — from Peace of mind
- **Fraud + dispute management** <span class="nn-ia-tag move">MOVE</span> — from Peace of mind
- **Discounts** <span class="nn-ia-tag keep">KEEP</span> — move from Global processing
- **Purchase details** <span class="nn-ia-tag keep">KEEP</span> — move from Global processing
- **Transfer API** <span class="nn-ia-tag keep">KEEP</span> — move from Global processing
</div>

<div class="nn-ia-section" markdown>
##### Control spending
- **Spend controls** <span class="nn-ia-tag rewrite">REWRITE</span>
    - Geo blocks
    - MCC blocks
    - Usage limiters
</div>

<div class="nn-ia-section" markdown>
##### Programme management
- **Customer portal (MyEnfuce)** <span class="nn-ia-tag keep">KEEP</span>
- **Card programme insights** <span class="nn-ia-tag move">MOVE</span> — from The control you want
- **MyApp — cardholder app** <span class="nn-ia-tag keep">KEEP</span>
- **MyCard — card bureau** <span class="nn-ia-tag keep">KEEP</span>
- **Partnerships** <span class="nn-ia-tag keep">KEEP</span>
- **Customer success** <span class="nn-ia-tag keep">KEEP</span>
</div>

<div class="nn-ia-section" markdown>
##### Compliance
- **Global compliance** <span class="nn-ia-tag keep">KEEP</span>
- **PCI DSS** <span class="nn-ia-tag new">NEW</span>
- **PSD2** <span class="nn-ia-tag new">NEW</span>
</div>

<div class="nn-ia-section" markdown>
##### References <span class="nn-ia-tag new">NEW</span>
- **Transaction codes** — extracted from Financial transaction page
- **Response codes** — extracted from existing content
- **Verification codes**
- **Glossary**
</div>

<div class="nn-ia-section" markdown>
##### API reference <span class="nn-ia-tag keep">KEEP</span>
Regrouped by domain:
Cards · Accounts · Customers · Transactions · Authorisation · Notifications · PIN · Transfers · Spend controls · Test API
</div>

<div class="nn-ia-section" markdown>
##### Release notes <span class="nn-ia-tag keep">KEEP</span>
Standalone section, linked from API pages.
</div>

</div>

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

This audit is a demonstration of how I'd approach the first month in the Enfuce Technical Writer role. Read everything. Map what exists. Note where the structure could be stronger, look at how comparable companies have solved similar challenges, and propose a grounded IA before writing a single new page.

The most important part of a documentation restructure is not the writing. It is the governance decisions — what belongs where, how content is organised, how shared content is maintained, how different audiences are served by the same navigation. Those decisions, made correctly at the start, determine whether the documentation works. The writing is what makes it readable.

---

*This audit was conducted in May 2026 as a portfolio piece. All observations are based on publicly accessible pages at docs.enfuce.com.*

*[← Back to portfolio home](../index.md)*
