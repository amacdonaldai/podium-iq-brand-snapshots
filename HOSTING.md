# Hosting

- Repo: https://github.com/amacdonaldai/podium-iq-brand-snapshots
- Pages root: https://amacdonaldai.github.io/podium-iq-brand-snapshots/
- Qualstar: https://amacdonaldai.github.io/podium-iq-brand-snapshots/qualstar/
- Ideal: https://amacdonaldai.github.io/podium-iq-brand-snapshots/ideal/
- Intero: https://amacdonaldai.github.io/podium-iq-brand-snapshots/intero/
- Location3: https://amacdonaldai.github.io/podium-iq-brand-snapshots/location3/
- Disruptive: https://amacdonaldai.github.io/podium-iq-brand-snapshots/disruptive/
- Level Agency: https://amacdonaldai.github.io/podium-iq-brand-snapshots/level-agency/
- Allen & Gerritsen: https://amacdonaldai.github.io/podium-iq-brand-snapshots/allen-gerritsen/
- Highdive: https://amacdonaldai.github.io/podium-iq-brand-snapshots/highdive/
- BarkleyOKRP: https://amacdonaldai.github.io/podium-iq-brand-snapshots/barkleyokrp/
- Perhaps: https://amacdonaldai.github.io/podium-iq-brand-snapshots/perhaps/
- eSEOspace: https://amacdonaldai.github.io/podium-iq-brand-snapshots/eseospace/
- Winger Marketing: https://amacdonaldai.github.io/podium-iq-brand-snapshots/winger/
- BastionBUNTIN: https://amacdonaldai.github.io/podium-iq-brand-snapshots/bastionbuntin/
- Curiosity: https://amacdonaldai.github.io/podium-iq-brand-snapshots/curiosity/
- USIM: https://amacdonaldai.github.io/podium-iq-brand-snapshots/usim/
- Veza: https://amacdonaldai.github.io/podium-iq-brand-snapshots/veza/
- Phaedon: https://amacdonaldai.github.io/podium-iq-brand-snapshots/phaedon/
- Rain: https://amacdonaldai.github.io/podium-iq-brand-snapshots/rain/
- Paradise: https://amacdonaldai.github.io/podium-iq-brand-snapshots/paradise/
- Signal Theory: https://amacdonaldai.github.io/podium-iq-brand-snapshots/signal-theory/
- The Shipyard: https://amacdonaldai.github.io/podium-iq-brand-snapshots/the-shipyard/
- Venables Bell: https://amacdonaldai.github.io/podium-iq-brand-snapshots/venables-bell/
- Build: `python3 build_snapshot.py --json data/<brand>.json --slug <slug>` then copy `sites/<slug>/` into publish repo and push.

## Ecosystem sources (hard — Leading independent sources cards)

Alan policy (2026-09-30): every brand-snapshot linked from outbound email **must** show the "Leading independent sources" cards under "Across the third-party ecosystem".

- Ship **`ecosystem_action`** (prose) **and** **≥4 `ecosystem_sources`** with schema `{name, citations, domain}` (`icon_url` recommended).
- Baseline (Ideal / Highdive / Qualstar): each source is `{name, domain, citations, icon_url?}`.
- **`label` / `share` alone is invalid** and not draft-ready. Page JS historically filtered to `name` (string) + `citations` (number) and hid `#ecosystem-evidence` when none remained.
- Convert legacy share percents with `citations = round(share/100 * citations_total)` when `citations_total` is present; never leave sources empty.
- Prefer Google s2 favicon URLs when `icon_url` is missing: `https://www.google.com/s2/favicons?domain=<domain>&sz=64`.
- After rebuild, verify live `report.json` has `name`+`citations` and the live page would show source cards (citations non-null) before linking a draft.

## Evidence rule (hard — Q&A always required)

Alan policy (2026-09-30): question–answer pairs are **always required and mandatory** on every brand-snapshot linked from outbound email.

- Ship at least 3 real Trace Q&A rows; never invent transcripts; never use summary-only fake evidence.
- Every `evidence[].excerpt` MUST be a literal contiguous substring of `evidence[].full_answer`.
- The page JS filters out any row that fails this check and hides the Q&A panel when none remain.
- A live snapshot that hides "See the answers" is not publishable and not draft-ready.
- Merge with `python3 merge_evidence_and_build.py`, then verify the live Pages URL shows Q&As before linking a draft.
- **No competitor-named questions.** Category/unbranded (or prospect-branded) only. See section below.


## No competitor-named questions (hard — Alan 2026-09-30)

Evidence queries must be **category/unbranded** commercial queries (or prospect-branded). Never lead with a rival agency name. Prefer queries where absence of the prospect is a fair finding.

- Good: "best creative agencies in the US…", "top independent agencies for…", "Which agencies help build long-term brand advocacy?"
- Bad: "is [Competitor] the best…", "[Competitor] alternatives", "Comparing [Competitor] with…", "[Competitor] vs…" — unless the **prospect** brand is also named in the query.
- Do not ship competitor-branded Q&A evidence that makes the prospect look foolishly absent from a question about a named rival.

## How to pull Q&A (Alan 2026-09-30)

Do **not** use Trace Layer. On the APP readout: tap the **topic bar** → reveals queries → **expand all** for the full Q&A stack. Copy real answers from there.

## Excerpt length (Alan 2026-09-30)

The visible Answer excerpt box must show a **substantial useful stretch of the real answer** (roughly 280–520 characters of body text), not a title line. Prospect should not need to expand “Read the full captured answer” to get value. Excerpt remains a literal substring of `full_answer`.

## Answer text formatting (mandatory, Alan 2026-09-30)

Prospect must **not** see large empty gaps in the Answer excerpt box (`.answer-quote` uses `white-space:pre-line`, so blank lines render as big vertical space).

For every `evidence[].full_answer` and `evidence[].excerpt`:
1. Normalize newlines (`\r\n` / `\r` → `\n`).
2. Strip trailing spaces on lines.
3. Collapse **2+ consecutive newlines into a single newline** (no blank lines between paragraphs/list items — single line breaks only).
4. Collapse 2+ horizontal spaces/tabs into one space.
5. Trim ends.
6. Excerpt must remain a literal contiguous substring of that row's `full_answer` after formatting.
7. Excerpt length target stays ~280–520 chars of useful body text.

Use `format_answer_text()` in `merge_evidence_and_build.py` (also mirrored in page JS `formatAnswerText`). Never leave double-newline runs in shipped JSON.
