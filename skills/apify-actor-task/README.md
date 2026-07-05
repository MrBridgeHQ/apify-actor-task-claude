# Apify Actor Task - a Claude skill

A Claude Code skill for **publishing Apify Actor tasks as public, keyword-targeted use-case landing pages** via the Console Publication tab → Examples tab.

It operates in three modes:

- **PLAN** - decide which tasks to publish (one distinct use-case each), how many, and how to name them.
- **WRITE** - produce the full publication spec for one task: slug, SEO title, SEO description, which input fields to display, dataset view, and prefilled input JSON.
- **AUDIT** - review an already-published task against a HARD/WARN/INFO checklist and report prioritized fixes.

> **Experimental feature.** Task publishing via the Apify Console Publication tab is gated and **not available to all accounts**. If the Publication tab is not visible on your task, contact Apify support to request access before using this skill.

## Why this skill exists

A saved Apify task is private by default. The Publication tab turns one task into an indexable landing page that ranks for a single high-intent search ("best bordeaux wines under 50 euros") and doubles as an Examples-tab entry inside the Store. The discipline is: one published task = one real use-case a user would actually type into Google or an AI assistant - never a generic "default config" or "test run". This skill encodes that selection strategy, the six-field publication spec, the Console mechanics, and a validation checklist.

## What's inside

```
apify-actor-task/
├── SKILL.md                          # Orchestration hub: modes, rules, anti-patterns, delegation
├── README.md                         # This file
├── references/
│   ├── task-strategy.md              # PLAN: which tasks to publish, goal-framed titles, slug rules
│   ├── publication-spec.md           # WRITE template + shared AUDIT checklist (six fields)
│   └── console-howto.md              # Step-by-step Console walkthrough + light measurement
└── assets/
    └── examples/
        └── vivino-tasks.md           # End-to-end worked example (fictional Actor)
```

## Key doctrine

- **Slug ≠ SEO title.** The slug is a lowercase-dashed URL identifier; the title is a goal-framed headline (`[Action verb] + [subject/target] + [qualifier]`). Never derive one mechanically from the other.
- **Displayed fields are a UI selection, not a schema edit.** Hiding a field from the public page does not remove it from the run.
- **Secrets are fixed on the schema.** Mask any credential field with `isSecret: true` in the Actor's input schema - never by hiding it from the display list (the API still exposes it).
- **One use-case per task.** Thin or duplicate tasks dilute the Examples tab; publish 2–4 distinct use-cases to start.

## Customize before use

The skill ships with neutral placeholders you should replace with your own values:

- `your-username/<actor>` - your Apify username and Actor slug.
- `YOUR_AFFILIATE_ID` - your Apify affiliate parameter (`?fpr=...`) if you run an affiliate funnel; otherwise drop the parameter entirely.
- `<your brand>` - the brand name you optionally cite in plain text in an SEO description.

The worked example uses a fictional Vivino wine-data scraper purely to illustrate the technique.

## Related skills

This skill deliberately delegates adjacent concerns rather than re-deriving them. In your own setup, pair it with:

- A **Store-content authoring** skill - fine SEO title/description wording, keyword research, dataset-schema and input-schema authoring.
- An **Actor-monetization** skill - PPE pricing, `Actor.charge()`, revenue audits.
- A **prose-humanization** skill - polishing SEO title/description prose.

## Part of the mr-bridge.com toolkit

This skill is part of the [mr-bridge.com](https://mr-bridge.com) toolkit for scraping, data, and content automation. Related resources:

- [mr-bridge.com](https://mr-bridge.com) - home
- [Scrapers](https://mr-bridge.com/scrapers) - Apify Actors
- [MCP servers](https://mr-bridge.com/mcp-servers) - Model Context Protocol servers
- [AI workflows](https://mr-bridge.com/ai-workflows) - agents and automation
- [Studies](https://mr-bridge.com/studies) - data studies and one-pagers
- [Articles](https://mr-bridge.com/articles) - write-ups and guides
- [Solutions](https://mr-bridge.com/solutions) - end-to-end solutions

## License

Personal use. Customize freely. No warranty.
