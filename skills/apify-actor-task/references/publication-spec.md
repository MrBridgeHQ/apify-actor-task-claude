# Publication Spec - WRITE Template & Shared AUDIT Checklist

This reference is loaded by the `apify-actor-task` skill for both **WRITE** mode (producing a publication spec for one task) and **AUDIT** mode (validating an already-published task). It is the single source of truth for field rules and the validation checklist - do not duplicate it in either mode.

---

## Part A - The Publication Spec (WRITE mode deliverable)

A complete publication spec has six fields. Produce all six. Missing any one field means the spec is incomplete.

---

### Field 1 - Slug

**Rule:** Short, lowercase, hyphen-separated, use-case keyword. 3–6 words. Reflects the goal, not the Actor name.

The public Examples-tab URL takes the form:
`https://apify.com/<owner>/<actor>/examples/<slug>?fpr=YOUR_AFFILIATE_ID`

(The `?fpr=` parameter is your Apify affiliate ID, if you run an affiliate funnel - see R1. Drop it if you do not.)

The slug is **not** the SEO title. It is a URL identifier. Never derive the title mechanically from the slug.

**Valid slugs:**
- `find-bordeaux-wines-under-50`
- `compare-tuscany-wine-ratings`
- `monitor-paris-hotel-prices`

**Invalid slugs:**
- `vivino-wine-data-scraper` (Actor name, not a use-case)
- `Find top-rated Bordeaux wines under €50` (this is a title, not a slug)
- `default-config` / `test-run-3` (meaningless)

---

### Field 2 - SEO Task Title

**Rule:** Goal-framed. Formula: **[Action verb] + [subject/target] + [qualifier]**. The title states what the *user* achieves, not what the Actor does.

Delegate length formulas, character limits, and wording polish to your Store-content authoring skill's SEO-description doctrine. Humanize final prose via your prose-humanization skill.

Quick validity test: can you replace the title with "Run the Actor" and have it still make sense as a button label? If yes, the title is mechanics-framed, not goal-framed - rewrite it.

EN + FR are both valid Publication tab entries (same task, localized title). Slug stays in English either way.

---

### Field 3 - SEO Description

**Rule:** Short meta description - **1–2 sentences, ~120–155 characters**. This is NOT a paragraph; it is the text that appears in search result snippets. A 160-word block is always wrong.

- State the immediate benefit and what data the user gets.
- MAY name your **brand** in plain text - no hyperlink (R2). There is no free prose body on a task landing page, so a hyperlink cannot be placed.
- Every `apify.com` URL in a description carries your affiliate parameter if you use one (R1).
- Do not start with "This task…" or repeat the title verbatim.

Delegate character-count formulas and A/B variant patterns to your Store-content authoring skill's SEO-description doctrine.

---

### Field 4 - Input Fields to Display

**Rule:** Select ONLY the fields that demonstrate this specific use-case. Hide irrelevant config options.

**Critical mechanic:** This is a **display selection** - a UI toggle that controls which fields appear on the public task landing page. It does NOT alter the task's actual input or the Actor's schema. The task still runs with its full saved input. Hiding a field from the display does not remove it from the run and does not alter the schema.

Selection criteria:
1. **Show** fields whose values directly communicate the use-case to a visitor ("Bordeaux", "under 50 EUR").
2. **Hide** fields that are irrelevant to this use-case (e.g., `includeReviews` if this task is about prices, not reviews).
3. **Never display** credential or sensitive fields. These must have `isSecret: true` in the Actor input schema (R6) - the fix is always on the schema, not on the display toggle.
4. Aim for 2–5 displayed fields. A wall of checkboxes defeats the use-case framing.

---

### Field 5 - Dataset View

**Rule:** Pick an **existing view** that matches this use-case. Do not invent a new view; do not rename columns.

A dataset view is a named column subset already defined in the Actor's output schema. Your job is to select the most relevant one for the use-case being showcased - e.g., a "prices" view for a price-tracking task, a "ratings" view for a comparison task.

If the Actor has only one view, select it. If no view matches the use-case well, flag this to the user - they may need to define a new view (via your Store-content authoring skill's dataset-schema doctrine) before publishing.

---

### Field 6 - Prefilled Input JSON

**Rule:** Concrete, realistic values for this exact use-case. These are the values a visitor sees when they land on the task page - they must tell the story of the use-case at a glance.

Requirements:
- Real values, not placeholders (`"bordeaux"`, not `"<your search term>"`).
- Cover every field you have chosen to display (plus any required hidden fields the task needs to run).
- If a field is `isSecret`, the saved input carries the credential internally - do not include it in the prefilled JSON you publish.

---

## Worked Example - a Vivino wine-data scraper

> Fictional Actor used only to illustrate the technique. Swap in your own Actor.

**Actor:** `your-username/vivino-wine-data-scraper`
**Public actor URL:** `https://apify.com/your-username/vivino-wine-data-scraper?fpr=YOUR_AFFILIATE_ID`
**Available input fields:** `wines`, `searchMode`, `includeTasteProfile`, `includeReviews`, `shipTo`, `countryCode`, `currencyCode`
**Use-case:** Find top-rated Bordeaux wines under €50

---

**Field 1 - Slug**
```
find-bordeaux-wines-under-50
```
Public URL: `https://apify.com/your-username/vivino-wine-data-scraper/examples/find-bordeaux-wines-under-50?fpr=YOUR_AFFILIATE_ID`

---

**Field 2 - SEO Task Title**

| Language | Title |
|---|---|
| EN | Find top-rated Bordeaux wines under €50 |
| FR | Trouver des Bordeaux les mieux notés à moins de 50 € |

---

**Field 3 - SEO Description**

EN: `Scrape Vivino for top-rated Bordeaux wines priced under €50 - ratings, prices, and taste profiles in one dataset.`
(~110 chars - within the 120–155 target once you append your brand)

FR: `Récupérez les Bordeaux les mieux notés à moins de 50 € sur Vivino - notes, prix et profils gustatifs en un seul jeu de données.`
(~127 chars - within the 120–155 target)

(Optionally append your brand in plain text at the end, no hyperlink - see R2.)

---

**Field 4 - Input Fields to Display**

Display these 4 fields (out of 7 available):

| Field | Reason to display |
|---|---|
| `wines` | The search term - shows the use-case subject ("bordeaux") |
| `searchMode` | Shows the scope (region-based search) |
| `countryCode` | Shows geographic filter (FR for France) |
| `currencyCode` | Shows price denomination (EUR) |

Hide `includeTasteProfile`, `includeReviews`, `shipTo` - not the focus of this use-case. Their values are still saved in the task and run normally.

---

**Field 5 - Dataset View**

Select the existing **"wines"** view (assumes it is defined; includes `name`, `rating`, `price`, `region` columns). If a "prices" view exists and is a closer match for the under-€50 framing, prefer it.

---

**Field 6 - Prefilled Input JSON**

```json
{
  "wines": "bordeaux",
  "searchMode": "region",
  "countryCode": "FR",
  "currencyCode": "EUR"
}
```

`includeTasteProfile`, `includeReviews`, and `shipTo` are not included here - they are saved in the task but not displayed, so they are not part of the visible prefilled spec.

---

## Part B - Shared Validation Checklist

Used by WRITE mode (self-check before handing off) and AUDIT mode (reviewing a published task). Both modes run the same checklist. Tags: **[HARD]** = blocks publication / must fix; **[WARN]** = degrades quality / should fix; **[INFO]** = nice-to-have.

---

### Checklist

- **[HARD]** If you run an affiliate funnel, every `apify.com` URL in the spec carries your affiliate parameter (R1). Apply it consistently across all URLs or not at all.

- **[HARD]** No credential or sensitive input field is included in the displayed fields list. Every such field has `isSecret: true` in the Actor's input schema (R6).
  - The fix is **always** `isSecret: true` on the existing field in the input schema.
  - Removing the field from the input schema is WRONG - it breaks the Actor.
  - Hiding the field from the display list alone is WRONG - the field remains visible to anyone who inspects the schema via the API.
  - `isSecret: true` causes Apify to auto-mask the field value in the public task view. That is the only correct mechanism.
  - To restructure a field's schema properties, use your Store-content authoring skill's input-schema doctrine.

- **[HARD]** No internal business context appears anywhere in the spec - no internal margins, client names, or org structure (R5). A plain-text brand name is fine (R2).

- **[WARN]** The SEO title is goal-framed (action + target + qualifier). Title is NOT: a default config label, a test run number, the Actor name, or a feature/mechanics description.

- **[WARN]** The SEO description is 1–2 sentences, approximately 120–155 characters. A description longer than ~200 characters is always wrong for this field.

- **[WARN]** Only use-case-relevant input fields are in the display list. Irrelevant fields (output format options, unrelated toggles, advanced config) are hidden. The display list has 2–5 fields; a list of 8+ nearly always contains noise.

- **[WARN]** The dataset view is an **existing** named view - not a proposed rename or a new column selection invented for this task. If no good view exists, flag it to the user.

- **[INFO]** A plain-text brand name is present in the SEO description where it fits naturally, without a hyperlink (R2). Absence is acceptable if it reads awkwardly; forced inclusion is worse.

---

### AUDIT Output Format

When running in AUDIT mode, report findings in this structure:

```
## AUDIT - <Actor slug> / <task slug>

### HARD findings (must fix before / immediately after publication)
- [HARD] <field or file ref>: <what is wrong> → FIX: <correct action>

### WARN findings (should fix to improve quality)
- [WARN] <field or file ref>: <what is wrong> → FIX: <correct action>

### INFO findings (optional improvements)
- [INFO] <field or file ref>: <what is missing> → FIX: <suggestion>

### Prioritized fix order
1. ...
2. ...
```

Report each finding with a field reference (e.g., "Input schema / `proxyApiKey`", "SEO description / character count", "Dataset view / column names"). Do not aggregate findings - one finding per issue.

---

### Worked AUDIT Example - Bad Baseline Task

> Fictional Actor used only to illustrate the technique.

**Task:** "Vivino scraper - task 3" on `your-username/vivino-wine-data-scraper`
**Symptoms observed:** Title is mechanics-framed, all 9 input fields displayed (including `proxyApiKey`), dataset view is a custom column rename, description is ~160 words.

---

```
## AUDIT - your-username/vivino-wine-data-scraper / vivino-scraper-task-3

### HARD findings

- [HARD] Input schema / `proxyApiKey`: A credential field is in the display list and does NOT have `isSecret: true` in the Actor input schema - the raw value is publicly visible.
  → FIX: Add `"isSecret": true` to the `proxyApiKey` field definition in `.actor/input_schema.json` and redeploy the Actor. Do NOT remove the field from the schema (the Actor needs it to run). Do NOT rely on hiding it from the display list alone (the API still exposes the value).

- [HARD] Slug / URL: The affiliate parameter is applied inconsistently across the spec's apify.com URLs.
  → FIX: Apply your affiliate parameter to every apify.com URL referenced in this spec, or omit it everywhere (R1).

### WARN findings

- [WARN] SEO title / "Vivino scraper - task 3": Mechanics-framed, not goal-framed. A task number is not a use-case.
  → FIX: Rewrite as action + target + qualifier (e.g., "Find top-rated Bordeaux wines under €50"). Consult your Store-content authoring skill's SEO-description doctrine for length/wording rules.

- [WARN] SEO description: ~160 words. Far exceeds the 1–2 sentence / ~120–155 character limit for a meta description.
  → FIX: Reduce to 1–2 sentences, ~120–155 chars. State the immediate benefit and dataset content. May include a plain-text brand name, no hyperlink.

- [WARN] Input fields display / all 9 fields shown: Displaying every available field defeats the use-case framing. Fields like `outputFormat`, `maxItems`, `debugMode` are irrelevant to "finding Bordeaux wines under €50".
  → FIX: Reduce display to 2–5 fields that directly communicate the use-case (e.g., `wines`, `searchMode`, `countryCode`, `currencyCode`). The hidden fields remain in the saved task and run normally - this is a display toggle, not a schema edit.

- [WARN] Dataset view / custom column rename: The selected view appears to be a column-rename invented for this task, not an existing named view from the Actor's output schema.
  → FIX: Select an existing dataset view (e.g., "wines" or "prices") from the Actor's defined views. If none matches the use-case, define a proper view (via your Store-content authoring skill's dataset-schema doctrine) before publishing.

### INFO findings

- [INFO] SEO description / brand: No brand mention. Optional, but natural to add at the end of a short description ("Powered by <your brand>.").

### Prioritized fix order
1. [HARD] Add `isSecret: true` to `proxyApiKey` in the input schema and redeploy.
2. [HARD] Apply the affiliate parameter consistently to all apify.com URLs.
3. [WARN] Rewrite the SEO title to be goal-framed.
4. [WARN] Shorten the SEO description to 1–2 sentences / ~120–155 chars.
5. [WARN] Reduce displayed input fields to the 2–5 relevant to the use-case.
6. [WARN] Switch the dataset view to an existing named view.
```

---

## What this reference does not cover

- Step-by-step Console walkthrough (where to find the Publication tab, how to set each field) → `references/console-howto.md`
- PLAN mode - which tasks to publish, how many, naming strategy → `references/task-strategy.md`
- Fine SEO title/description length formulas and wording patterns → your Store-content authoring skill (SEO-description doctrine)
- Defining or restructuring a dataset view → your Store-content authoring skill (dataset-schema doctrine)
- Marking a field `isSecret` (schema authoring) → your Store-content authoring skill (input-schema doctrine)
