# AI and Documentation

## How I use AI in a real documentation workflow

AI tools have changed how I work — but not in the way most people assume. I do not use AI to write documentation. I use it to work faster on the parts of documentation that are not writing.

---

## What AI is good at in a documentation workflow

**First drafts from rough notes** — after an SME interview, I have rough notes, diagrams, and half-formed sentences. AI turns those into a readable first draft in minutes. I then edit that draft heavily — checking accuracy, restructuring for the reader, removing anything that does not serve the user's task. The draft is a starting point, not a deliverable.

**Identifying gaps** — I prompt AI to ask "what questions would a developer have after reading this page that this page does not answer?" That surfaces gaps I would otherwise miss.

**Consistency checking** — AI can scan a long document for inconsistent terminology faster than I can. When I restructured the Unzer portal, I used it to flag places where the same concept was described differently on different pages.

**Prompt engineering for documentation** — I wrote a structured prompt bank for a law firm knowledge hub, covering document summarisation, meeting notes, and client communication. Each prompt is designed to be reusable, role-appropriate, and responsible — including clear guidance on when not to use AI output without review.

---

## What AI is not good at

**Accuracy** — AI will confidently generate plausible-sounding API parameters that do not exist. Every technical claim in AI-generated content needs to be verified against the source. This is non-negotiable in payment documentation where an incorrect parameter description could break a merchant's integration.

**User intent** — AI can write a page about a topic. It cannot decide whether that topic deserves its own page, who the reader is, or what the reader already knows. Those are information architecture decisions that require judgment.

**Structure** — AI produces prose. Good documentation is a system. The decisions about how to organise content, what belongs on a reference page versus a task page, how to handle legacy content — those are not writing decisions. AI cannot make them.

---

## Cursor at Unzer

At Unzer, I used **Cursor** — an AI-native code editor — to assist with documentation writing in a docs-as-code workflow. Markdown files lived in a Git repository, and Cursor helped me draft, refine, and maintain content at scale. This is a more sophisticated use case than using a chat interface — it means AI is integrated into the authoring environment rather than used as a separate tool.

The result: the documentation portal I built became the content foundation for **UnzerAI**, an AI assistant now embedded in docs.unzer.com that answers developer integration questions 24/7. AI assistants are only as good as the content they index. Clean, single-purpose, well-structured pages produce accurate AI answers. Mixed, duplicated, long-scrolling pages produce unreliable ones. This is what AI-ready documentation actually means — not documentation about AI, but documentation structured so AI can use it effectively.

---

## The prompt bank

As part of a portfolio project for a knowledge management role, I built a structured prompt bank for a law firm AI knowledge hub — covering three core use cases: document summarisation and research, meeting notes and action items, and client communication drafting.

Each prompt includes:

- A clear use case and audience
- Variable placeholders in `[square brackets]` for reusability
- A "why this matters" note for each question
- Responsible use guidance — including when not to use AI output without human review

**Example prompt — plain-language document summary:**

```
Summarise the following document in plain English. Write for a reader with 
no legal background. Use short paragraphs. Highlight the three most important 
points. Flag anything that requires a lawyer's review before the reader acts on it.

Document: [paste document text here]
```

The square bracket convention makes prompts reusable without editing the core instruction. The responsible use framing — flag what needs review — is built into the prompt itself rather than left to the user's judgment.

---

## My view on AI and technical writing

AI will automate the parts of technical writing that were already mechanical — templated content, first drafts, basic formatting. It will not automate the parts that matter: deciding what to write, who to write it for, how to structure it, and whether it is accurate.

The writers who thrive are the ones who use AI to go faster on the mechanical parts and spend the saved time on the judgment parts. That is how I use it.

---

*[Back to Home](../)*
