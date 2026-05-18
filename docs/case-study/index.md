# Case Study — Unzer Developer Portal

<div class="nn-hero">
  <span class="nn-role">Case Study · Oct 2020 – Oct 2025</span>
  <h1>From inherited portal to AI-ready documentation system</h1>
  <p>How I took over a fragmented developer documentation platform, restructured it from the ground up, achieved EU accessibility compliance, and built content that now powers an AI assistant used by developers across Europe.</p>
</div>

---

## The situation

When I joined Unzer in October 2020, the developer documentation portal was built on **ReadMe.io** — a generic third-party platform with Unzer branding applied on top. An external agency (3di Information Solutions) had been brought in to migrate the platform to a custom Hugo-based site. My role was to serve as the internal content owner during that migration, briefing the agency on requirements, reviewing deliverables, and ensuring the content served real user needs rather than the product's internal structure.

When the agency completed the migration and handed over, I became the sole owner of the portal — responsible for all content, structure, maintenance, and quality.

---

## The problem — what the old documentation was doing

Before looking at what I changed, it helps to understand exactly what was wrong. This is the same Embedded Payment Page documentation as it existed when I inherited the portal:

!!! warning "The old structure"
    The page below represents the documentation as it existed in August 2021 — a single 11-page document mixing concept, task, and reference content with no clear separation.

**What a developer found on one page:**

- A conceptual overview of what the Embedded Payment Page is
- A 4-step implementation procedure
- An optional prerequisite section (customers, baskets, metadata resources) buried mid-task
- A parameter reference table mid-procedure
- Constructor and method summary tables
- Customisation options
- Localisation settings

A developer wanting to know which parameters to include in their API call had to scroll through conceptual explanations and optional prerequisites to find a reference table. A developer who had already read the overview and just needed to check a parameter still had to load the full 11-page document.

**The Supported Payment Methods page had the same problem** — a page titled as a reference list that also contained conceptual explanations of payment type IDs, task instructions for creating and retrieving payment types, and update procedures. Three topic types, one URL.

---

## The decisions — and why I made them

<div class="nn-decision">
  <span class="nn-decision-label">Decision 1</span>
  <h4>Restructure from product-based to audience-based navigation</h4>
  <p>The old navigation reflected how Unzer organised their products internally — not how developers and merchants think about their tasks. A merchant wanting to add a payment method to their shop and a developer writing API integration code have entirely different mental models. I separated them: Plugins for merchants, Server-side integration for developers, Use cases for business readers. Each audience lands where their task starts.</p>
</div>

<div class="nn-decision">
  <span class="nn-decision-label">Decision 2</span>
  <h4>Apply topic-based authoring — one page, one purpose, one audience</h4>
  <p>Every page in the old portal mixed concept, task, and reference content. I restructured the entire portal using DITA-style topic types: concept pages explain what something is, task pages explain how to do something, reference pages contain lookup data. A developer debugging a webhook should not have to read an overview of what webhooks are to find the error code table. Separating these reduced page length, improved scannability, and made individual pages linkable and reusable.</p>
</div>

<div class="nn-decision">
  <span class="nn-decision-label">Decision 3</span>
  <h4>Introduce single-source publishing via metadata-based topic types</h4>
  <p>Shared content — authentication notes, security warnings, code sample patterns — was duplicated across dozens of pages. When something changed, it had to be updated in every location. I introduced reusable content snippets using metadata-based topic types, so shared definitions, warnings, and code patterns lived in one place and propagated everywhere. This eliminated content drift — where different pages gave different instructions for the same thing — which was a genuine source of merchant confusion and support tickets.</p>
</div>

<div class="nn-decision">
  <span class="nn-decision-label">Decision 4</span>
  <h4>Implement WCAG 2.1 AA compliance for EU Accessibility Act</h4>
  <p>The European Accessibility Act created a legal obligation for digital services to meet accessibility standards. I audited all 250+ pages of the portal against WCAG 2.1 AA requirements and reworked navigation structure, colour contrast, alternative text on all images, and link semantics throughout. This was not a cosmetic change — it required reviewing and editing every page in the portal and rebuilding several navigation components. The EAA compliance section now lives as a permanent, dedicated section of the portal.</p>
</div>

<div class="nn-decision">
  <span class="nn-decision-label">Decision 5</span>
  <h4>Add a news and releases section for transparent change communication</h4>
  <p>When an API changed, a plugin was updated, or a payment method was added, there was no structured way to communicate this to developers and merchants. They found out when something broke. I created a news and releases section with structured release notes so developers could track what changed and when — reducing the support queries that spiked every time something changed without notice.</p>
</div>

<div class="nn-decision">
  <span class="nn-decision-label">Decision 6</span>
  <h4>Archive legacy content with clear signposting rather than deleting it</h4>
  <p>When Payment Page v1 was deprecated in favour of v2, simply removing the old documentation would have broken integrations for merchants who had not yet migrated. I moved the legacy content to a clearly labelled Legacy section with migration guides pointing to the new version. Merchants with existing integrations could still find what they needed. New merchants were directed clearly to v2. This is a governance decision, not a writing decision — and it is one that most documentation overhauls get wrong.</p>
</div>

---

## Before and after

<div class="nn-compare">
  <div class="nn-before">
    <span class="nn-before-label">Before — 2021</span>
    <strong>Navigation</strong>
    <p>Flat hierarchy reflecting product structure. Accept payments → Accept payments with API → Accept payments with payment page → Embedded Payment Page. Every audience on the same path.</p>
    <br>
    <strong>Page structure</strong>
    <p>Long single-scrolling pages mixing concept, task, and reference. The Embedded Payment Page page was 11 pages of mixed content on one URL.</p>
    <br>
    <strong>Shared content</strong>
    <p>Authentication warnings, code patterns, and security notes duplicated manually across dozens of pages. No single source of truth.</p>
    <br>
    <strong>Legacy content</strong>
    <p>No strategy. Old versions disappeared or remained without context.</p>
  </div>
  <div class="nn-after">
    <span class="nn-after-label">After — 2024</span>
    <strong>Navigation</strong>
    <p>Audience-based hierarchy. Plugins for merchants. Server-side integration for developers. Use cases for business readers. Each audience arrives at their task directly.</p>
    <br>
    <strong>Page structure</strong>
    <p>Short, focused pages. One topic type per page. Each page serves one audience and one intent. Reference content separated from task content.</p>
    <br>
    <strong>Shared content</strong>
    <p>Single-source publishing via reusable content snippets. Changes made once, reflected everywhere. No content drift.</p>
    <br>
    <strong>Legacy content</strong>
    <p>Dedicated Legacy section with clear migration guides. Old integrations still findable. New integrations directed to current versions.</p>
  </div>
</div>

---

## The outcome

**The support team used the portal as their primary reference** to answer merchant queries — not because they were told to, but because the documentation was accurate, well-structured, and trustworthy enough to rely on in live support conversations. That is a higher bar than most documentation achieves.

**The portal now powers an AI assistant** — UnzerAI, embedded directly in docs.unzer.com, answers developer integration questions 24/7. AI assistants are only as good as the content they index. Clean, single-purpose, well-structured pages return accurate AI answers. Mixed, duplicated, long-scrolling pages produce unreliable ones. The structured content architecture I built directly enables the AI assistant that Unzer launched.

**The portal is publicly accessible** at [docs.unzer.com](https://docs.unzer.com) — the live result of five years of documentation ownership.

---

## What I learned

The hardest part of this project was not the writing. It was the governance — deciding what belonged where, maintaining consistency across 250+ pages over five years, and making structural decisions that served users I never met but could infer from the questions they asked.

Good documentation is a system. A well-written page in a badly structured system fails. A simply-written page in a well-governed system succeeds. The structural decisions I made — audience separation, topic typing, single-source publishing, legacy archiving — are the decisions that determined whether the documentation worked. The writing is what made it readable.

---

*Next: [View writing samples →](../samples/)*
