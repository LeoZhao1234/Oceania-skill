---
name: amazon-listing-workflow
description: End-to-end Amazon listing creation workflow that composes a listing and then audits it for intent coverage. Use this skill whenever a user wants to write, create, draft, or build an Amazon listing from scratch, especially when they mention a product spec file, competitor ASINs, or keywords. Also triggers when the user says things like "create a listing", "write a listing", "build a product page", "make an Amazon listing", or "compose and audit a listing". This skill must be used instead of running amazon-listing-composer or amazon-listing-intent-auditor separately — it handles the full compose → audit → revise → save workflow in one shot.
---

# Amazon Listing Workflow

This skill orchestrates two sub-skills in sequence:
1. **amazon-listing-composer** — writes the listing draft
2. **amazon-listing-intent-auditor** — audits it for intent coverage gaps
3. **Revision pass** — applies auditor priority fixes to the draft
4. **Save** — writes the final listing to the working folder

## Step 0 — Ask for marketplace first (before anything else)

Before collecting any other input, ask:

> "Which marketplace are we targeting? (e.g. US, UK, DE, CA)"

Wait for the answer. Do not proceed until you have it.

## Step 1 — Collect remaining inputs

After confirming the marketplace, ask for (or confirm) the following:

| Input | Required | Notes |
|---|---|---|
| Product spec file | Yes | File path — read in full before writing any copy |
| Competitor ASINs | Yes | 2–5 ASINs |
| Keyword brief | Yes | Provided file or derived from competitor traffic data |
| Selling points file | Optional | Ranked list of features/benefits |

If the user already provided some of these in the conversation, use them — do not ask again.

## Step 2 — Run the amazon-listing-composer workflow

Follow the full `amazon-listing-composer` skill from Step 1 through Step 5:

- Collect competitor data in parallel (product detail, traffic terms, reviews)
- Build parity vs gap analysis
- If selling points provided: confirm and rank them with the user before writing
- Collect Rufus signals
- Write title, 5 bullets, description (narrative + specifications + FAQ), and backend search terms
- Run the full compliance sweep (all 12 checks)
- Output the listing draft in the standard `━━━` block format

After outputting the draft, ask:
> "Would you like the Competitor Insight Summary? Y to include, N to skip."

Wait for a reply before proceeding to Step 3.

## Step 3 — Run the amazon-listing-intent-auditor workflow

Pass the completed listing draft (title, bullets, description, backend terms) directly into the `amazon-listing-intent-auditor` workflow. Use the product category and marketplace already confirmed.

Follow the full auditor workflow:
- Identify the product mission
- Build the search path map
- Score listing coverage across all 7 dimensions
- Inspect each page element (title, bullets, description, backend keywords)
- Detect intent dynamics (saturation cliff, complement opportunity, session reset risk, substitution window)
- Output the full audit report in the standard auditor format

## Step 4 — Revision pass

After the audit report, do the following automatically (no additional prompt needed):

1. Read the **Priority fixes** section from the audit report — these are the highest-impact changes.
2. Cross-check each fix against the product spec file to ensure any new claims are verifiable.
3. Apply fixes to the listing draft. Typical changes include:
   - Inserting missing specification or complement terms into bullets or backend terms
   - Rewriting a bullet that scores poorly on a key intent dimension
   - Adjusting title to add a missing generalization or specification term
   - Adding complement language to Bullet 5 or the FAQ
4. Re-run the compliance sweep (Step 5 of the composer skill) on the revised copy.
5. Output the revised listing in the standard `━━━` block format, clearly labeled:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
REVISED LISTING (post-audit)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Immediately after the revised block, list what changed:

```
CHANGES APPLIED:
- [change 1 and which audit fix it addresses]
- [change 2 ...]
```

## Step 5 — Save to working folder

Save the final output to a Markdown file in the current working directory (the folder the user is working in, not the skill directory).

**File name format:** `[brand-slug]-listing-[YYYYMMDD].md`
- `brand-slug`: brand name lowercased, spaces replaced with hyphens
- `YYYYMMDD`: today's date

**File contents** (in order):
1. The revised listing block (title, bullets, description, specifications, FAQ, backend terms)
2. The Competitor Insight Summary block (if the user requested it in Step 2)
3. The full audit report from Step 3
4. The changes applied list from Step 4

Preserve all `━━━` section dividers exactly.

Confirm to the user:
> "Final listing saved to `[full file path]`."

## Hard constraints

- Marketplace must be confirmed before any other action.
- Never invent features not in the product spec file.
- Apply only fixes from the audit's Priority fixes section — do not rewrite sections the auditor scored 4 or 5.
- All compliance rules from the composer skill apply to the revised copy as well.
- If a priority fix cannot be applied without inventing an unverified claim, note it as skipped and explain why.
