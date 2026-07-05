# Worked Example - a Vivino wine-data scraper

> Fictional Actor used only to illustrate the technique end to end. Swap in your own Actor, use-cases, and (optionally) affiliate parameter.

**Actor:** `your-username/vivino-wine-data-scraper`
**Public Actor URL:** `https://apify.com/your-username/vivino-wine-data-scraper?fpr=YOUR_AFFILIATE_ID`
**Available input fields:** `wines`, `searchMode`, `includeTasteProfile`, `includeReviews`, `shipTo`, `countryCode`, `currencyCode`

---

## Part 1 - PLAN: Candidate tasks to publish

Output of the PLAN mode for this Actor. Three distinct goal-framed use-cases, prioritized by expected organic value (highest first).

| # | Task name (slug) | SEO title | Use-case label | Target keyword | Why it wins |
|---|---|---|---|---|---|
| 1 | `find-bordeaux-wines-under-50` | Find top-rated Bordeaux wines under €50 | Location + Segment | "best bordeaux wines under 50 euros" | Low competition, high commercial intent, flagship use-case matching `wines` + `countryCode` + `currencyCode` |
| 2 | `compare-wine-ratings-tuscany` | Compare wine ratings across Tuscany regions | Location + Workflow | "compare vivino ratings tuscany wines" | Informational intent, no direct task-page competitors, showcases `searchMode` regional scope |
| 3 | `export-wine-ratings-prices-spreadsheet` | Export wine ratings & prices to a spreadsheet | Workflow / Deliverable | "vivino wine data export scraper" | Developer/analyst segment, pipeline use-case differentiator, surfaces `includeTasteProfile` as bonus field |

These three tasks cover distinct axes (bargain-hunting by region, competitive comparison by region, data-pipeline export) and together demonstrate the Actor's full range without overlapping.

---

## Part 2 - WRITE spec: flagship task

### Use-case: Find top-rated Bordeaux wines under €50

---

#### Field 1 - Slug

```
find-bordeaux-wines-under-50
```

Landing-page URL:
`https://apify.com/your-username/vivino-wine-data-scraper/examples/find-bordeaux-wines-under-50?fpr=YOUR_AFFILIATE_ID`

---

#### Field 2 - SEO Task Title

| Language | Title |
|---|---|
| EN | Find top-rated Bordeaux wines under €50 |
| FR | Trouver des Bordeaux les mieux notés à moins de 50 € |

---

#### Field 3 - SEO Description

**EN (~110 chars, append your brand to reach the 120–155 band):**
`Scrape Vivino for top-rated Bordeaux wines priced under €50 - ratings, prices, and taste profiles in one dataset.`

**FR (~127 chars):**
`Récupérez les Bordeaux les mieux notés à moins de 50 € sur Vivino - notes, prix et profils gustatifs en un seul jeu de données.`

Both descriptions: 1–2 sentences, within (or near) the 120–155 character target, optional plain-text brand name (no hyperlink), no apify.com URL embedded.

---

#### Field 4 - Input Fields to Display

Display these 4 fields (out of 7 available):

| Field | Reason to display |
|---|---|
| `wines` | The search term - shows the use-case subject ("bordeaux") |
| `searchMode` | Shows the scope (region-based search) |
| `countryCode` | Shows geographic filter (FR for France) |
| `currencyCode` | Shows price denomination (EUR) |

**Hidden (not displayed, still run):** `includeTasteProfile`, `includeReviews`, `shipTo` - not the focus of the under-€50 bargain-hunting use-case. Their values are saved in the task and execute normally on every run.

---

#### Field 5 - Dataset View

Select the existing **"wines"** view (includes `name`, `rating`, `price`, `region` columns). If a more specific **"prices"** view is defined on this Actor, prefer it - the under-€50 qualifier makes price the primary column of interest.

Do not invent a new view or rename columns for this task.

---

#### Field 6 - Prefilled Input JSON

```json
{
  "wines": "bordeaux",
  "searchMode": "region",
  "countryCode": "FR",
  "currencyCode": "EUR"
}
```

`includeTasteProfile`, `includeReviews`, and `shipTo` are saved in the task but are not displayed on the landing page, so they are omitted from the visible prefilled spec.

---

## Part 3 - FR variant (flagship only)

The FR variant uses the same slug (`find-bordeaux-wines-under-50`), same prefilled JSON, and same displayed fields. Only the Publication tab's SEO title and SEO description fields change:

| Field | FR value |
|---|---|
| SEO title | Trouver des Bordeaux les mieux notés à moins de 50 € |
| SEO description | `Récupérez les Bordeaux les mieux notés à moins de 50 € sur Vivino - notes, prix et profils gustatifs en un seul jeu de données.` |

---

## Self-check - publication-spec.md checklist

| Check | Result |
|---|---|
| [HARD] Affiliate parameter applied consistently (or omitted everywhere) | PASS - placeholder applied to every apify.com URL; no bare/mixed apify.com URLs |
| [HARD] No credential or sensitive field in the display list | PASS - no `isSecret` fields involved; none of the 7 input fields are credentials |
| [HARD] No internal business context (margins/clients/org) | PASS - none present |
| [WARN] SEO title is goal-framed (action + target + qualifier) | PASS - "Find top-rated Bordeaux wines under €50" |
| [WARN] SEO description is 1–2 sentences, ~120–155 chars | PASS - EN ~110 chars (append brand), FR ~127 chars |
| [WARN] Display list is 2–5 relevant fields, irrelevant fields hidden | PASS - 4 fields displayed, 3 hidden |
| [WARN] Dataset view is an existing named view, not invented | PASS - selects existing "wines" (or "prices") view; flagged if neither exists |
| [INFO] Plain-text brand name, no hyperlink | OPTIONAL - append "Powered by <your brand>." at end of description if it fits |

No HARD or WARN failures. Spec is ready for Console entry.

---

> **Note:** This file is an illustrative example. The values above must be entered manually in the Apify Console Publication tab for your own Actor. Nothing in this file is auto-published. The Console Publication tab is an experimental Apify feature - contact Apify support to enable it for your Actor before proceeding.
