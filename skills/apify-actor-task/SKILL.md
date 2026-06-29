---
name: apify-actor-task
description: Use when planning, authoring, or auditing the publication of an Apify Actor task as a public use-case landing page — deciding which tasks to publish (one keyword-targeted use-case each), producing the publication spec (slug, SEO title, SEO description, which input fields to display, dataset view, prefilled input JSON) for the Console Publication tab, or auditing already-published tasks. Triggers on "publish an Apify task", "Actor task landing page", "Examples tab", "task SEO title / slug / description", "which tasks should I publish", "publish-task", EN or FR. Task publishing is an experimental Apify feature (contact Apify support for access). Distinct from the Actor README/Store content, pricing, and prose humanization.
---

# Apify Actor Task

This skill owns **task publication for Apify Actors**: turning a saved task into a public, keyword-targeted use-case landing page via the Console Publication tab → Examples tab. It covers task selection strategy, the full publication spec (slug, SEO title, SEO description, displayed input fields, dataset view, prefilled input JSON), and post-publish audits. Fine SEO-copy doctrine (length formulas, wording patterns) is delegated to a dedicated Store-content authoring skill, not re-derived here.

---

> **Experimental feature.** Task publishing via the Apify Console Publication tab is **not available to all users** — it is a gated experimental feature. If the Console Publication tab is not visible on your Actor, contact Apify support to request access before proceeding with any step below.

---

## First action when invoked

1. **Detect the mode** (PLAN / WRITE / AUDIT) from the request. If ambiguous, ask once.
2. **Load the matching reference(s)** from the table below — do not produce output from memory alone.
3. **Then produce** the artifact for that mode.

Never skip step 2. The references are the source of truth for validation rules, field specs, and canonical examples.

### Load-on-demand table

| Mode / need | Load this |
|---|---|
| **PLAN** — which tasks to publish, how many, naming strategy | `references/task-strategy.md` |
| **WRITE** — produce a publication spec for one task | `references/publication-spec.md` (+ delegate fine copy to your Store-content authoring skill) |
| **AUDIT** — review an existing published task | `references/publication-spec.md` (the validation checklist section) |
| **How-to / Console mechanics** — where to click, Publication tab fields, dataset views | `references/console-howto.md` |

---

## Delegation table

This skill owns task selection and the publication spec. It does not re-derive copy doctrine or schema authoring — those belong to sister skills/tooling in your own setup.

| Trigger | Redirect |
|---|---|
| Fine SEO title/description wording (length formulas, patterns) | Your Store-content authoring skill (SEO-description doctrine) |
| Keyword research for a task's target use-case | Your Store-content authoring skill (keyword + GEO/AEO doctrine) |
| Humanizing the title/description prose | Your prose-humanization skill |
| Defining or restructuring the dataset view itself | Your Store-content authoring skill (dataset-schema doctrine) |
| Marking an input field `isSecret` in the input schema | Your Store-content authoring skill (input-schema doctrine) |
| Pricing/monetization of the underlying Actor | Your Actor-monetization skill |

---

## Rules

**R1 — Affiliate parameter.** If you run an Apify affiliate funnel, every `apify.com/…` URL produced by this skill should carry your affiliate parameter (`?fpr=YOUR_AFFILIATE_ID`). Apply it consistently or not at all. The examples below use the placeholder `YOUR_AFFILIATE_ID` — substitute your own.

**R2 — Brand name, no hyperlink.** A task landing page has no free prose body, so there is no place to embed a hyperlink. Your **brand name MAY be cited in plain text** (no hyperlink) in the SEO description when the use-case naturally allows it (e.g. "by <your brand>"). Do not force it.

**R3 — Experimental gate.** Before any WRITE or AUDIT step, confirm the user has the Publication tab enabled. If uncertain, instruct them to contact Apify support first.

**R4 — Examples are illustrative.** All worked examples use a generic, fictional Actor (a Vivino wine-data scraper) purely to illustrate the technique. Replace it with your own Actor and use-cases.

**R5 — No internal business context.** Never include any internal business context (margins, client names, org structure, private analytics) in a published task spec. A plain-text brand name in the SEO description is fine (see R2 — no hyperlink); business internals are not.

**R6 — Secret-field gate before publication.** Before recommending that a task be published, confirm every credential or sensitive input field has `isSecret: true` in the Actor's input schema. Apify auto-masks `isSecret` fields in the public task view. The AUDIT mode must check this explicitly.

---

## Anti-patterns (doctrine counters)

| Failure mode | Correct doctrine |
|---|---|
| Conflating the task **slug** with the SEO **title** | They are different fields — never treat one as the other. |
| Inventing platform mechanics (e.g. "Featured task slots", "publish #1-3 as Featured") | No such slots exist. There is one Publication tab per task. Do not invent mechanics not documented in `references/console-howto.md`. |
| Treating the public input-field list as the task's full input | The displayed fields are a **selection for the UI**; the task still runs with its full input configuration. Hiding a field from the display does not remove it from the run. |
| Fixing a leaked secret by removing the field from the input schema | Fix it with `isSecret: true` on the existing field (R6). The public display list is a selection, not a schema edit. |

Field-level specs (slug format, SEO title/description length & formulas, goal-framed title rules) live in `references/publication-spec.md` and are delegated to your Store-content authoring skill's SEO-description doctrine — not restated here.

---

## When to ask / when to defer

**Ask the user when:**
- Mode (PLAN / WRITE / AUDIT) is ambiguous.
- The target Actor and its existing input schema are not provided (needed for WRITE and AUDIT).
- The desired use-case keyword for a new task is unclear.

**Defer to the user when:**
- Choosing which specific keyword to target (competitive positioning decision).
- Approving the final prefilled input JSON values (user knows their Actor's realistic defaults).
- Deciding whether to request the experimental feature from Apify support (user action required).

---

## Available references

| File | Purpose |
|---|---|
| `references/task-strategy.md` | How to select, scope, and name tasks — one use-case per task, goal-framed titles, how many to publish, slug conventions |
| `references/publication-spec.md` | Full spec for the Console Publication tab — all fields, validation checklist, display-field selection, dataset-view selection, prefilled JSON, AUDIT checklist |
| `references/console-howto.md` | Step-by-step Console walkthrough — where the Publication tab lives, how to set each field, how to verify the published Examples tab |

---

## What this skill is not

- **Not the Actor README or Store description.** Publishing artifacts (Store description, README, input/output schemas) belong to a dedicated Store-content authoring skill.
- **Not pricing or monetization.** PPE setup, `Actor.charge()`, revenue audit belong to a dedicated Actor-monetization skill.
- **Not creating or saving the task input itself.** Saving a task configuration in the Apify Console is a basic product operation, not a skill concern.
- **Not a non-Apify distribution channel.** Listing on third-party directories belongs to a dedicated distribution skill.
