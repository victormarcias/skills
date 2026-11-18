# brand-name-check

`/brand-name-check` scores proposed brand, product or tool names against the
brand's purpose: what it is, for whom and with what tone. It returns tables
with color-coded scores: a weighted scorecard (brand fit, recall, audience,
eyeballing, digital assets and SEO) with a detail row per metric, where each
name shines and where it breaks, better variants, and a clear winner when
several names are compared. On request it also proposes alternative names
and exports the report as a PDF to `~/Downloads`.

It checks domain availability with `whois` and, when web search is
available, who already ranks for the exact name. It writes no files except
that PDF and touches no accounts; trademarks and social handles are flagged
as risks, not checked. Instructions: [SKILL.md](SKILL.md).
