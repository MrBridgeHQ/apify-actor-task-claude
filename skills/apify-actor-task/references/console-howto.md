# Console How-To - Publishing an Actor Task

This reference is loaded by the `apify-actor-task` skill when the user needs **Console mechanics**: where to click, what each Publication tab section does, how to verify the result, and how to track basic success. Field-level rules (slug format, SEO title formulas, secret-field doctrine) live in `references/publication-spec.md` - not repeated here.

---

> **Experimental feature - check first.** Task publishing via the Apify Console Publication tab is **not available to all users**. It is a gated, experimental capability. If the Publication tab is not visible on your task, contact Apify support to request access before doing anything else described below.

---

## Prerequisites

Before opening the Publication tab, confirm all three of these are true:

- [ ] **A published Actor you own or maintain.** The Actor must be live in the Apify Store (or at minimum deployed). You cannot publish a task on a draft or deleted Actor.
- [ ] **A saved task with a complete input configuration.** Open the Actor in the Apify Console, go to **Tasks**, and verify the target task is saved with all required fields filled in. The task must be able to run as-is - the Publication tab does not let you configure the input, only select which fields to display.
- [ ] **An input schema AND at least one dataset schema view defined on the Actor.** The Publication tab requires both: (1) the Actor must have a valid `.actor/input_schema.json`; (2) the Actor must have at least one named dataset view in its output schema. If no dataset view exists, define one before proceeding - see your Store-content authoring skill's dataset-schema doctrine.

If any prerequisite is missing, fix it first. Attempting to publish without them will either block the Publication tab or produce a broken landing page.

---

## Step-by-step walkthrough

### Step 1 - Open the task

In the Apify Console, navigate to **Actors → [your Actor] → Tasks** and click the saved task you want to publish.

### Step 2 - Open the Publication tab

Inside the task view, click the **Publication** tab. If this tab is not visible, the experimental feature is not enabled on your account - contact Apify support.

The Publication tab has three sections: **Display information**, **Input**, and **Dataset schema**. Complete them in order.

---

### Step 3 - Display information

This section controls the public identity of the task landing page.

| Field | What it is | What to do |
|---|---|---|
| **Slug** | The URL identifier for this task's landing page | Enter a short, lowercase, hyphen-separated, use-case keyword (3–6 words). See `publication-spec.md` Field 1 for rules and valid/invalid examples. |
| **SEO task title** | The `<title>` tag and main heading on the landing page | Enter a goal-framed title (action + target + qualifier). See `publication-spec.md` Field 2. |
| **SEO description** | The meta description - appears in search result snippets | Enter 1–2 sentences, ~120–155 characters. See `publication-spec.md` Field 3. |

**Live previews update as you type:**

- **Page Preview** - shows how the landing page heading and description will render on `apify.com`.
- **SERP Preview** - shows how the title and description will appear in a Google search result snippet.

Use both previews to catch truncation before publishing. If the SERP preview cuts the description, shorten it.

---

### Step 4 - Input

This section controls which input fields appear on the public landing page.

You will see a list of all fields defined in the Actor's input schema. Toggle each field **on** (displayed) or **off** (hidden from the public page).

Key mechanics to understand:

- This is a **display selection only.** Toggling a field off does not remove it from the saved task or from the Actor's schema. The task still runs with its full saved input, including all hidden fields.
- Fields with `isSecret: true` in the Actor's input schema are **automatically masked** - their values are never shown to visitors regardless of whether they are toggled on. If you see a credential or API key field that does NOT have `isSecret: true`, stop and fix the schema before publishing (R6 - see `publication-spec.md` for the exact fix).
- Aim for **2–5 displayed fields** that tell the use-case story at a glance. A wall of toggles defeats the purpose of a use-case landing page.

---

### Step 5 - Dataset schema

This section controls which dataset view renders on the landing page as a sample output table.

Select one **existing named view** from the dropdown. These views come from the Actor's output schema - you are selecting among views that already exist, not creating or renaming columns here.

Pick the view whose columns best match the use-case this task demonstrates (e.g., a "prices" view for a price-tracking task). If the dropdown is empty or no view fits the use-case, the Actor's output schema needs a new view - define it via your Store-content authoring skill's dataset-schema doctrine and redeploy the Actor before returning here.

---

### Step 6 - Publish task

Click **Publish task**. Apify creates the public landing page.

---

## What the published landing page looks like

Once published, the task appears at:

```
https://apify.com/<username>/<actor-name>/examples/<task-slug>?fpr=YOUR_AFFILIATE_ID
```

If you run an affiliate funnel, add your affiliate parameter when sharing or linking to this URL (R1 - apply it consistently across every `apify.com` URL, or not at all).

The landing page:

- Is indexed by search engines (`index, follow`). Expect it to appear in Google within days to weeks depending on the Actor's domain authority.
- Exposes a `.md` version at the same URL with a `.md` suffix - AI agents and LLMs can read it directly.
- Appears in the Actor's **Examples** tab in the Apify Store, giving it a second discovery surface for existing Actor visitors.

**What happens when a visitor clicks the call-to-action:** Apify creates a new task under the visitor's own account with the same input configuration as your published task. The visitor gets an independent copy to run and customize. Your original task is untouched.

---

## Verifying the published task

After clicking **Publish task**, do a quick sanity check:

1. Open the landing-page URL in a private/incognito browser tab. Verify the title, description, displayed input fields, and dataset view sample all appear as intended.
2. Open the Actor's **Examples** tab in the Apify Store and confirm the new task appears there.
3. Check that no credential values are visible on the page. If a value is visible that should be masked, stop - add `isSecret: true` to that field in the input schema and redeploy the Actor, then re-publish the task.

---

## Light measurement

Task publication generates two trackable signals. Keep measurement lightweight here - deep analytics belong in generic web tooling (Google Search Console, Plausible, etc.).

**Traffic to the landing page**

Monitor search-engine clicks to `/examples/<slug>` pages via Google Search Console (property: `apify.com` or your Actor's sub-path if available). Look for:
- Impressions and clicks on the task's target keyword over the first 4–8 weeks.
- A rising click-through rate as the page gains ranking - a well-framed goal title outperforms a mechanics title here.

**Affiliate attribution from Apify**

If you run an affiliate funnel, your affiliate parameter routes conversions through the Apify affiliate dashboard. Check the affiliate dashboard for:
- Click-throughs on `apify.com/<username>/<actor-name>/examples/<slug>`.
- Any resulting sign-ups or runs attributed to your affiliate parameter.

**Examples-tab referrals**

The Examples tab is a second discovery surface within the Apify Store. No separate tracking is needed - any conversion via the Examples tab that results in a run on your Actor already appears in your Actor's run metrics.

Keep it simple: check GSC + affiliate dashboard monthly; do not build custom dashboards before you have meaningful traffic.

---

## What this reference does not cover

- Field-level rules (slug format, SEO title/description formulas, goal-framing, character limits, `isSecret` doctrine) → `references/publication-spec.md`
- Which tasks to publish, how many, naming strategy → `references/task-strategy.md`
- Defining a new dataset view on the Actor → your Store-content authoring skill (dataset-schema doctrine)
- Marking a field `isSecret` in the input schema → your Store-content authoring skill (input-schema doctrine)
- Fine SEO copy wording and A/B patterns → your Store-content authoring skill (SEO-description doctrine)
