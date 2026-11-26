# Investigation Setup

Read this file at the start of every investigation. It holds the dependency check, the disambiguation rules, the invocation menus, and the folder structure.

## Dependency Check

Before doing anything else, check whether the following skills appear in the available skill list for this session:

- `fact-check-workflow`
- `literature-review`
- `document-skills:pdf`
- `data-visualization`
- `expert-panel`

If ANY are missing, say:

> "research-intel works best with its full skill set. The following skills are missing from this session:
> [list the missing ones]
>
> Install them using either method below, then restart your session:
>
> **`fact-check-workflow`**
> - `npx skills add jamditis/claude-skills-journalism@fact-check-workflow` (third-party repo)
>
> **`literature-review`**
> - `npx skills add eyadsibai/ltk@literature-review` (third-party repo)
>
> **`document-skills:pdf`**
> - `npx skills add anthropics/skills@pdf`
> - In Claude Code: `/plugin marketplace add anthropics/skills`, then `/plugin install document-skills@anthropic-agent-skills`
>
> **`data-visualization`**
> - `npx skills add anthropics/knowledge-work-plugins@data-visualization`
>
> **`expert-panel`**
> - `npx skills add thedanielmay/expert-panel-skill`
> - `/plugin install expert-panel@thedanielmay`
>
> Or type **'continue anyway'** to proceed — steps that need missing skills will be skipped."

If the user types 'continue anyway', note which skills are unavailable and proceed. When a missing skill would normally be invoked, skip that step and note it inline: "(skipped — `literature-review` not available)".

If all skills are present, proceed silently without mentioning the check.

---

## Disambiguation: topic mode vs company mode

Before starting, confirm the subject is a topic, not a company.

| Subject type | Mode to use |
|---|---|
| Named legal entity (company, charity, trust, government agency) | company mode |
| Policy, framework, standard, regulation | topic mode |
| Industry, market, or sector | topic mode |
| Technology category or product class (not one specific company) | topic mode |
| Geographic area or community | topic mode |
| Trend, movement, or time-bounded phenomenon | topic mode |
| Public figure (not a company they run) | topic mode |
| Scientific or academic field | topic mode |
| Ambiguous — could be company or topic | Show disambiguation prompt |

**Disambiguation prompt** (show when ambiguous):
```
Is this research about the organisation itself, or the broader topic?

  C: The organisation [Name], company mode (dossier, people, financials)
  T: The broader topic or landscape, topic mode (market, policy, ecosystem)
  B: Both: company mode first, then topic mode for sector context

(C / T / B)
```

**If user selects C:** Leave topic mode. Switch to company mode instead — it is purpose-built for named legal entities.
**If user selects T:** Continue with topic mode.
**If user selects B:** Run company mode for the entity first and finish it, then return to topic mode for the sector context.

Full routing logic and natural hand-off points between modes are documented in `references/shared/INTELLIGENCE-STANDARDS.md` Section 14.

---

## Invocation

### Menu-driven (no topic specified)

```
TOPIC INTELLIGENCE RESEARCH
───────────────────────────
1. New Investigation     : Research a topic from scratch
2. Resume Investigation  : Continue an existing topic dossier
3. Refresh Investigation : Update a dossier with new data
4. Quick Scan            : Fast overview (~30 min equivalent)

Which would you like? (1 to 4)
```

### Refresh Investigation protocol (Menu option 3)

When a user selects Refresh Investigation:

1. Load `00-summary/topic-scoping.md` and `research-log.md` from the existing investigation folder
2. Identify what has changed since the last run:
   - Time elapsed (has the monitoring plan's refresh cadence been reached?)
   - Monitoring plan trigger events (have any triggers from `00-summary/monitoring-plan.md` fired?)
   - New KITs added by the user in this session
3. Determine refresh scope: which phases need re-running vs. which findings can be carried forward unchanged
4. Run only the affected phases — do not re-run the full investigation
5. Update these files: `kit-answers.md`, `critical-findings.md`, `rag-dashboard.md`, `monitoring-plan.md`
6. Append a refresh block to `research-log.md`:
   ```
   ## Refresh: [YYYY-MM-DD]
   What was refreshed: [phases re-run]
   What was carried forward: [phases not re-run and why]
   Material changes: [what changed vs. prior run]
   Next refresh: [date or trigger]
   ```
7. Do not regenerate the PDF unless findings have materially changed

If New Investigation selected and no topic given:
```
What topic would you like to research?

Examples:
  · A market or industry     ("Australian textile recycling market")
  · A policy or regulation   ("Victorian Social Procurement Framework")
  · A technology area        ("circular economy ERP software")
  · A geographic community   ("social enterprise ecosystem, inner Melbourne")
  · A trend or movement      ("impact investing in Australia, 2023 to 2025")
  · A public figure          ("name, role/context")
  · An academic/science area ("microplastics in marine environments")

Topic:
```

### Direct invocation (topic specified)

When a topic is named — whether explicit (`research-intel: [topic]`) or implicit (`Research the Victorian Social Procurement Framework`) — skip the menu and go directly to Phase 0 in `references/topic/skill.md`.

**Skipping the menu does not skip Phase 0.** If topic is given but investigation depth and KITs are not specified, still perform Steps 2 and 3 of Phase 0 before any research begins.

---

## Folder Structure

**Naming:** Always use `_topic-{slug}/` format, where `{slug}` is the topic lowercased, spaces replaced by hyphens (e.g., `_topic-victorian-social-procurement-framework`, `_topic-australian-textile-recycling-market`).

```
_topic-{slug}/
├── full-synthesis-report-YYYY-MM-DD-HH:MM.pdf   ← Final deliverable (Tier 3 only)
├── 00-summary/
│   ├── topic-scoping.md                          ← Phase 0 output, confirmed before research
│   ├── kit-answers.md                            ← KIT Answer Register
│   ├── critical-findings.md
│   ├── executive-summary.md
│   ├── triangulation-matrix.md
│   ├── contested-claims.md                       ← if applicable
│   ├── information-gaps.md
│   ├── monitoring-plan.md
│   ├── assumptions-limitations.md
│   └── synthesis-source-register.md          ← claim-to-source linkage map (required before synthesis prose)
├── 01-primary-sources/                           ← Official documents, legislation, data
├── 02-media-coverage/                            ← News, interviews, podcasts, video
├── 03-stakeholders-actors/                       ← People, organisations, networks
├── 04-data-statistics/                           ← Quantitative data, datasets
├── 05-analysis/                                  ← Frameworks, models, synthesis drafts
│   ├── sci-citation-manifest.md              ← SCI module only: citation identifier verification
│   ├── synthesis-[MODULE-CODE]-primary.md    ← module synthesis before Phase 11 integration
│   └── synthesis-[MODULE-CODE]-secondary.md  ← secondary module synthesis files (if chaining)
├── inaccessible-sources.md                       ← Sources that failed all four layers
└── research-log.md                               ← Running log of all research activity
```

Every subfolder gets an `INDEX.md` cataloguing its contents with source, date, confidence rating, and summary.

---
