---
name: brand-name-check
description: Score and critique one or more proposed brand, company, product or tool names against the brand's purpose (what it is, for whom, with what tone). Weighted scorecard covering brand fit, recall, audience, eyeballing and digital assets (domains checked, SEO confusion risk), then friction analysis, variants and a clear winner. Use when the user runs /brand-name-check, proposes names, or asks whether a name fits their positioning.
argument-hint: <name(s)> [-- brand purpose] [--alternatives] [--pdf]
---

# Brand name check

Judge names the way a skeptical brand strategist with B2B and tech
experience would: honest, specific, no flattery for a weak name.

## 1. Pin down the brand purpose

The arguments carry the candidate names and, optionally, the purpose. If the
purpose is missing, take it from the conversation or from the thing being
named (a repo, a product page). Only when nothing gives it, ask once, in one
question, for: what the brand does, the target customer, the value
proposition and the tone it wants.

State the purpose back in two lines, then list the 3–5 brand attributes it
needs to convey (for example: speed, common sense, low risk, craft). Every
score is read against them.

## 2. Check the digital assets

Before scoring, check each name for real:

- **Domains:** the name squashed and hyphen-free on `.com`, `.dev`, `.io`
  and the audience's country TLD, with `whois <domain>`. A "no match" or
  "not found" line means free; a registrar and dates mean taken; when
  `whois` is inconclusive, `dig +short NS <domain>` with no answer suggests
  free. Mark parked or for-sale domains as taken.
- **Search:** when web search is available, search the exact name and note
  what the first results are (a competitor, a generic phrase, a famous
  product, nothing).
- **Commands:** when the name is for a CLI or a package, check that the
  command does not collide with a common binary (`which`, `brew info`) or a
  package of the same name.

Trademarks, company registries and social handles stay unchecked: flag the
obvious risks and say a real check is still needed.

## 3. Scorecard (1–10 per criterion)

| Criterion | Weight | What it measures |
| --- | --- | --- |
| Brand fit | 25% | Attribute matrix: how many of the brand attributes the name conveys. "So what?" test: does someone outside the niche understand what the brand does, or does it need a descriptor (Labs, Studio, Engineering)? |
| Recall & recognition | 25% | Spontaneous recall: is it remembered after hearing it once? Recognition: does it stand apart from the category's clichés? Idioms and loanwords help recall only for audiences that already know them. |
| Audience fit | 20% | Segmentation: does the ideal customer feel "they think like me"? Exclusion index: who it puts off, and whether losing them is wanted (filters bad clients) or costly (filters the ones who pay). |
| Eyeballing & legibility | 15% | Words, characters and cognitive density. How it looks in a logo, a favicon, a web footer, a GitHub profile, an invoice, an email signature. For a tool: how it types as a command. |
| Digital assets & SEO | 15% | Domain availability from step 2. Search intent and confusion risk: who it competes with for the exact term, and whether ranking needs a suffix. Spelling when heard over the phone. |

Present one table: a row per name, a column per criterion, the weighted
final score, and a one-line verdict per name. With several names, all go in
the same table, sorted by final score.

Then one detail table per name, a row per metric with its finding in one
line: attribute matrix, "so what?", spontaneous recall, recognition,
segmentation, exclusion index, cognitive density, eyeballing, search
confusion. Then a domains table: domain, status (free, taken, unclear).

## 4. Friction analysis

One table, a row per name worth keeping:

| Name | Where it shines | Where it breaks |
| --- | --- | --- |

- **Where it shines:** the best real scenario (cold email, email signature,
  a technical meetup, a pitch deck).
- **Where it breaks:** the worst real scenario (spelled over the phone to a
  non-native speaker, a formal B2B invoice, an organic search, a slide at a
  conservative enterprise).

Skip names that scored too low to matter and say so in one line under the
table.

## 5. Variants

For each name with potential that fails on one criterion, propose 3
variants (a suffix, an acronym, a shorter form, a compound), in one table.
Check each variant's main domain the same way as in step 2.

| Original | Variant | Domain | Problem it fixes |
| --- | --- | --- | --- |

## 6. Verdict

With several names, pick one winner and name the trade-off accepted by
choosing it. With one name, say plainly whether to keep it, keep a variant
or keep searching.

## 7. Alternatives (optional)

Only with `--alternatives` or when the user asks for them after the
report. Propose 5 new names built from the brand attributes, not from the
rejected name, avoiding the category's clichés. Check each one's `.com` and
`.dev` as in step 2, score them with the same criteria in one table sorted
by final score, and say which one would beat the current winner, if any.

| Name | Idea behind it | Domains | Final | Verdict |
| --- | --- | --- | --- | --- |

## 8. PDF (optional)

Only with `--pdf` or when the user asks for it after the report. Build a
self-contained HTML file in the scratchpad with the whole report (tables,
emojis, sources), styled for print: system font, collapsed table borders,
`white-space: pre-wrap` in cells. Convert it with headless Chrome or
Chromium if installed:

```bash
"<chrome binary>" --headless --no-pdf-header-footer \
  --print-to-pdf="$HOME/Downloads/brand-name-check-<name>.pdf" <file>.html
```

Without a Chromium browser, use the PDF skill if one is available. Give
the PDF's full path.

## 9. Offer

After the verdict, offer whatever was not done yet with one
`AskUserQuestion` call: a multi-select question listing the pending extras
(5 alternative names with checked domains, the report as a PDF in
`~/Downloads`), each with a one-line description. When only one extra is
pending, ask a single-select question with "Yes" and "No" instead, since a
question needs two options. Run what is picked; if nothing is, stop. Skip
the question when both were already done.

## Tone

- Every section's result is a table; prose only for the purpose
  statement and the verdict, a few lines each.
- Color only where it helps scanning: 🟢 🟡 🔴 before each score (8–10,
  5–7, 1–4) and each domain status (free, unclear, taken). No other emojis:
  verdicts start with a bold **Keep**, **Variant only** or **Discard**,
  section headings are plain, and the winner's name is bold.
- Direct and analytical. A bad name is called bad, with the reason.
- Use real branding terms (positioning, recall, cognitive density, digital
  real estate) mixed with the language of the user's industry.
- Flag clichés of the category (in tech: Solutions, Innova-, Nexus, -Soft,
  -ify) as a recognition penalty.
- Answer in the language of the conversation.
