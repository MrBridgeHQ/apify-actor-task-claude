# Task Strategy — PLAN Mode Doctrine

This reference is loaded by the `apify-actor-task` skill whenever the mode is **PLAN**: deciding which tasks to publish, how many, and how to name them. Read this before producing any PLAN output.

---

## Core principle

**One published task = one real use-case a user would search.**

A task earns a public landing page only when it represents a *distinct, self-contained goal* a real user types into Google or an AI assistant. Publish one task per use-case to show the Actor's range across different audiences and intents. Not every saved task configuration deserves a page.

Three questions that must all answer "yes" before a task qualifies for publication:

1. Can you state the user's goal in ≤ 7 words with an action verb? ("Find top-rated Bordeaux wines under €50")
2. Is there a real keyword or AI query behind it? ("best bordeaux wines under 50 euros", "compare vivino ratings tuscany")
3. Does it show a genuinely different angle of the Actor's capabilities vs. the other tasks you are publishing?

If a task fails any of these, it stays private.

---

## Title frame: action + target + qualifier

The SEO title states the **user's goal**, not the Actor's mechanics, data type, or config state.

Formula: **[Action verb] + [subject/target] + [qualifier]**

- Action verb: Find, Track, Compare, Monitor, Export, Identify, Discover
- Subject/target: what the user cares about (wines, hotels, prices, ratings)
- Qualifier: the narrowing that makes it low-competition and high-intent (location, price band, time horizon, workflow)

### Weak vs. strong titles

| Weak | Strong |
|---|---|
| Vivino scraper - default config | Find top-rated Bordeaux wines under €50 |
| Test run 2 | Track Burgundy grand cru prices over time |
| All fields enabled | Compare wine ratings across Tuscany regions |
| Hotel scraper default | Monitor Paris hotel prices for travel planning |

The slug and the SEO title are **different fields**. The slug is a lowercase-dashed identifier (`find-bordeaux-wines-under-50`). The SEO title is the human-readable, goal-framed headline. Never treat a slug as a title or derive the title mechanically from the slug.

---

## Use-case × keyword matrix

Derive tasks along three axes. One Actor typically yields 3–6 distinct tasks this way.

| Axis | Key question | Example qualifier |
|---|---|---|
| **Location / geography** | Where does the user's target exist? | "Bordeaux", "Tuscany", "Paris", "Burgundy" |
| **Industry / segment** | What niche or vertical does the user care about? | "grand cru", "budget wines under €50", "boutique hotels" |
| **Workflow / deliverable** | What does the user *do* with the data? | "export to spreadsheet", "track over time", "compare across regions" |

Each intersection of axes that maps to a real search intent is a candidate task. Discard combinations where the use-case is implausible or duplicates another task.

### Worked example — a Vivino wine-data scraper (`your-username/vivino-wine-data-scraper`)

> Fictional Actor used only to illustrate the technique. Swap in your own Actor.

Key input fields: `wines`, `searchMode`, `includeTasteProfile`, `includeReviews`, `shipTo`, `countryCode`, `currencyCode`. Extracts: ratings, prices, taste profiles, reviews.

| Task | Axis combination | Target keyword |
|---|---|---|
| Find top-rated Bordeaux wines under €50 | Location (Bordeaux) + Segment (budget) | "best bordeaux wines under 50 euros" |
| Compare wine ratings across Tuscany regions | Location (Tuscany) + Workflow (compare) | "compare vivino ratings tuscany wines" |
| Export wine ratings & prices to a spreadsheet | Workflow (export deliverable) | "vivino wine data export scraper" |

These three tasks show three distinct angles: bargain-hunting by region, competitive comparison by region, and data-pipeline use. Together they demonstrate the Actor's range without overlapping.

### Short illustrations — other example Actors

**A wine-price scraper** (`your-username/wine-price-scraper`):
- Use-case: price tracking over time for premium wines
- Title: **Track Burgundy grand cru prices over time**
- Target keyword: "wine price history burgundy grand cru"

**A hotel-price scraper** (`your-username/hotel-price-scraper`):
- Use-case: travel price monitoring for a specific destination
- Title: **Monitor Paris hotel prices for travel planning**
- Target keyword: "paris hotel prices scraper"

---

## EN + FR bilingual examples

If you target multiple language markets, translate the goal frame, not the mechanics. Keep the same formula.

| Language | Title |
|---|---|
| EN | Find top-rated Bordeaux wines under €50 |
| FR | Trouver des Bordeaux les mieux notés à moins de 50 € |

Both titles resolve to the same task configuration. The slug stays in English (URL convention); the Publication tab SEO title field carries the localized version.

---

## PLAN output format

Produce a prioritized list — highest expected organic value first. One row per candidate task.

| # | Task name (slug) | SEO title | Use-case label | Target keyword | Why it wins |
|---|---|---|---|---|---|
| 1 | `find-bordeaux-wines-under-50` | Find top-rated Bordeaux wines under €50 | Location + Segment | "best bordeaux wines under 50 euros" | Low competition, high commercial intent, matches actor capability |
| 2 | `compare-wine-ratings-tuscany` | Compare wine ratings across Tuscany regions | Location + Workflow | "compare vivino ratings tuscany wines" | Informational intent, no direct competitors on task landing pages |
| 3 | `export-wine-ratings-prices-spreadsheet` | Export wine ratings & prices to a spreadsheet | Workflow / Deliverable | "vivino wine data export scraper" | Developer/analyst segment, pipeline use-case differentiator |

**Slug rules:**
- All lowercase, hyphens only, no special characters
- 3–6 words; reflects the use-case, not the Actor name
- Never the Actor's display name or a copy of the SEO title verbatim

**Prioritization heuristics** (rough, in order):
1. Highest search volume with lowest existing competition (use-case not already served by a task or rich SERP result)
2. Strongest match to the Actor's most distinctive input fields
3. Most shareable as an AI-assistant answer (a clear, answerable goal users also ask in ChatGPT / Perplexity)

---

## How many tasks to publish

There is no fixed minimum or maximum. Practical guidance:

- **Start with 2–4** tasks that clearly cover distinct use-cases. Saturating the Examples tab with 8+ tasks of the same flavor dilutes each one.
- **Each task must pass the three-question gate** above independently.
- **Add more only when** a new location/segment/workflow combination maps to a genuinely different keyword cluster with real search demand.
- **Do not publish** a task solely to increase the Examples tab count. Thin use-cases hurt rather than help discoverability.

---

## Pre-publication gate (PLAN checklist)

Before recommending any task for publication, confirm:

- [ ] Every credential or sensitive input field has `isSecret: true` in the Actor's input schema (R6). Apify auto-masks `isSecret` fields in the public task view — the fix is always on the schema, never by hiding the field from the display list.
- [ ] The Console Publication tab is enabled for this Actor (experimental gate — R3). If not, instruct the user to contact Apify support before proceeding.
- [ ] No internal business context appears in any title, description, or slug (R5). A plain-text brand name is allowed (R2); hyperlinks are not (no free prose body on a task landing page).

---

## What this reference does not cover

- Full field-by-field spec for the Console Publication tab (slug character limits, SEO description length formulas, input-field display selection, dataset-view selection, prefilled input JSON) → `references/publication-spec.md`
- Step-by-step Console walkthrough → `references/console-howto.md`
- Fine SEO title/description wording, length formulas → your Store-content authoring skill (SEO-description doctrine)
- Keyword research for a chosen use-case → your Store-content authoring skill (keyword doctrine)
