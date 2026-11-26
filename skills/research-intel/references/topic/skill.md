# Topic Intelligence Research

## Overview

A structured intelligence-gathering skill that performs deep, multi-source research on any non-company topic. Produces a comprehensive dossier with provenance-tracked findings, organised into a navigable folder structure.

**Core principles:**
- Define scope before searching — never research blind
- Every claim needs a source and a confidence rating
- Triangulate across independent sources — one source is not enough
- Negative space is intelligence — what doesn't exist is as important as what does
- KITs (Key Intelligence Topics) drive everything — orient every phase toward answering them

**Announce at start:** "Using the research-intel skill (topic mode) to conduct a structured intelligence investigation."


**Reference files. Read each one at the time named below.**

- `references/shared/INTELLIGENCE-STANDARDS.md`: read first, at the start of every investigation. It holds the STOP gate, scoping document, KIT register, confidence ratings, saturation rule, wave rules, progress reporting, synthesis sequence, deliverables, disambiguation routing, tool check and ethical boundaries.
- `references/shared/web-access-layers.md`: read at the start of every investigation. It describes the four web access layers.
- `references/shared/layer-4-playwright-protocol.md`: read when Layer 4 is triggered.
- `references/topic/topic-sources.md`: read when identifying sources. It holds the source register by topic type, with API endpoints and access notes.
- `references/topic/topic-outputs.md`: read when producing outputs. It holds the output templates for each topic type.
- `references/topic/topic-analytical-frameworks.md`: read during analysis. It holds the analytical frameworks for topic research.
- `references/topic/topic-modules.md`: read after classifying the topic type. It holds the KIT defaults, phase sets, sources, output formats and chaining protocol for all seven modules.
- `references/topic/topic-synthesis.md`: read at Phase 11. It holds the full synthesis, expert QA and deliverable procedure.
- `references/topic/topic-expert-panels.md`: read at Phase 11 QA. It holds seven module-specific expert sets and three integration modes.
- `references/topic/topic-osint.md`: read when using OSINT. It holds search operators, Wayback techniques, social media signals and document metadata methods.
- `references/shared/agent-dispatch.md`: read when dispatching parallel agents. It holds briefing templates, handoff format, per-wave notes and timeout fallback.
- `references/topic/investigation-setup.md`: read at the start of every investigation. It holds the dependency check, disambiguation rules, invocation menus and folder structure.

---


## Phase 0: Intelligence Requirements Definition

**This is the most important phase. Do not skip it.**

### Step 1: Confirm topic and classify type

State the topic and its type:
```
TOPIC INTELLIGENCE RESEARCH
───────────────────────────
Topic:   [topic as stated]
Type:    [topic type: see table below]
```

**Topic type codes:**

| Code | Type | Examples |
|---|---|---|
| MKT | Market / industry | "Australian textile recycling market", "B2B stationery sector" |
| POL | Policy / regulatory | "Victorian Social Procurement Framework", "ISSB climate standards" |
| SCI | Academic / scientific | "microplastics in marine environments", "social impact measurement" |
| TEC | Technology / product | "circular economy ERP platforms", "AI tools for NFP fundraising" |
| GEO | Geographic / community | "social enterprise ecosystem in inner Melbourne" |
| TRD | Trend / event | "impact investing trends 2023–2025", "post-COVID supply chain shifts" |
| PRS | Person (public figure) | Research on a named public figure's career, views, record |

If type is ambiguous between two codes, ask the user to confirm before proceeding.

### Step 2: Offer investigation depth

If the user named a depth in the request, use that depth and skip this step.

```
INVESTIGATION DEPTH
───────────────────
1. Quick Scan:          Core sources, key facts, recent developments. ~30 min.
                        Produces: 1-page briefing note.
2. Standard:            All of Quick Scan + stakeholder map, data sources,
                        analysis, synthesis. ~2 hrs.
                        Produces: Multi-section dossier + executive summary.
3. Full Investigation:  Everything. All sources, cross-reference, expert QA
                        review, PDF synthesis report. ~4 to 6 hrs.
                        Produces: Complete dossier + PDF.

Which depth? (1 to 3; suggested: 1 Quick Scan)
```

### Step 3: KIT elicitation

KITs are the 2–5 questions you most need answered. Everything in the research is oriented toward answering them.

```
KEY INTELLIGENCE TOPICS
───────────────────────
What are the 2 to 5 questions you most need this research to answer?
These drive the entire investigation, the more specific, the better.

[Show examples relevant to the topic type, see Topic Type Modules below]

Your KITs (list them, one per line):
```

If user gives vague or no KITs, propose a set based on the topic type defaults (see each module) and ask for confirmation.

### Step 4: Topic Scoping Document

Before any research begins, propose and confirm this document:

```
TOPIC SCOPING DOCUMENT
──────────────────────
Topic:        [topic as refined]
Topic type:   [code and name]
Depth tier:   [1/2/3: name]
Output:       [what will be produced]

Key Intelligence Topics:
  1. [KIT 1]
  2. [KIT 2]
  3. [KIT 3]
  [additional KITs if provided]

Scope inclusions:
  [Geographic boundaries, time range, sub-topics, source types to prioritise]

Scope exclusions:
  [What is explicitly out of scope, prevents drift]

Primary sources to target:
  [3 to 6 named source types most relevant to this topic type]

Output location:
  _topic-{slug}/

──────────────────────
Does this capture what you need? (yes / adjust scope / change KITs)
```

**Research does not begin until the user confirms this document.**

**STOP — do not run any searches, WebFetch calls, or tool checks until the user has confirmed this document.** The temptation to begin research while awaiting confirmation is a common failure mode. Resist it.

Save to: `_topic-{slug}/00-summary/topic-scoping.md`

---

## Web Access — Four-Layer Architecture

Read `references/shared/web-access-layers.md` for the full protocol.

**Summary:**
- **Layer 1 — API-first:** Use public APIs where available (ABS, legislation.gov.au, Semantic Scholar, CrossRef, ACNC, etc.)
- **Layer 2 — Search as router:** Use WebSearch to find the exact URL, then fetch only that URL. Never crawl blind.
- **Layer 3 — Scraping service:** If WebFetch fails, route through ScrapingBee/ZenRows/BrightData/ScraperAPI only after the user says yes for it. A configured key is not a yes. Otherwise go to the free browser read (Layer 4).
- **Layer 4 - Playwright:** Automatic fallback when a search or fetch fails. Tier 1 and Tier 2 never use it.

**Layer 4 fallback rule:** Layer 4 is only the free browser read of a public page after a failed search or fetch. If a search or fetch has failed and Layers 1 to 3 give no other way, run Layer 4 without asking and record the exact source and why Layer 4 is needed (except a sensitive integrity investigation: confirm with the user first). If Playwright is not installed, or the browser also fails, log the source as a gap with that reason in `inaccessible-sources.md` and move on. Layer 4 runs as a normal browser that identifies as automation. The browser never signs in. Do not solve or bypass a captcha. Log a captcha as a gap unless a person can reach the page and paste its content. A 429 rate limit response never switches to the browser. Wait and retry later, or log the source as a gap. LinkedIn is not a Layer 4 target. Use only LinkedIn content that is publicly visible without login through Layers 1 to 3, or content the user pastes. Read `references/shared/layer-4-playwright-protocol.md` for the execution protocol and source-logging format.

**Pre-investigation tool check** (run once at the start of every investigation):
```bash
python3 -c "import playwright" 2>/dev/null && echo "playwright ok" || echo "playwright NOT installed; skip Layer 4"
[ -n "${SCRAPINGBEE_API_KEY:-}" ] && echo "SCRAPINGBEE_API_KEY: set" || echo "SCRAPINGBEE_API_KEY: not set"
[ -n "${BRIGHTDATA_API_KEY:-}" ] && echo "BRIGHTDATA_API_KEY: set" || echo "BRIGHTDATA_API_KEY: not set"
[ -n "${ZENROWS_API_KEY:-}" ] && echo "ZENROWS_API_KEY: set" || echo "ZENROWS_API_KEY: not set"
[ -n "${SCRAPERAPI_KEY:-}" ] && echo "SCRAPERAPI_KEY: set" || echo "SCRAPERAPI_KEY: not set"
```

Run the check and record the actual results in `research-log.md`. Note any tools that are unavailable before proceeding.

If the Playwright check fails, say Playwright is not installed, skip Layer 4 for this investigation, and log each affected source as a gap with that reason.

See `references/shared/INTELLIGENCE-STANDARDS.md` Section 15 for the standard tool check format.

---

## Confidence Rating System

See `references/shared/INTELLIGENCE-STANDARDS.md` Section 4 for the full grading system and source independence rule. Apply two grades to every piece of information recorded: source reliability A to F and information credibility 1 to 6, written together (for example B2). Keep the ⚠ flag for claims contradicted by another source.

**Source independence matters.** Two sources citing the same original report = one source.

---

## Parallel Execution Strategy

Run phases in waves to maximise efficiency:

**Wave 1** (must complete first): Phase 0 — scope definition and tool check
**Wave 2** (parallel): Primary source identification + stakeholder/actor mapping + data and statistics search
**Wave 3** (after Wave 2): Media and community sources + policy/regulatory/technical depth + cross-reference
**Wave 4** (after all data): Framework analysis and synthesis
**Wave 5** (final): Expert QA review + deliverable generation

**Wave-level parallelisation:** Each wave can be executed using parallel agents rather than sequentially. When doing so:
- Include the confirmed scoping document, current KIT status, and research log in every agent briefing
- Use Explore agents for Waves 2-3 (source discovery), general-purpose agents for Waves 4-5 (analysis and synthesis)
- Apply tier effort caps: Tier 1 = Layer 2 max; Tier 2 = Layer 3 max; Tier 3 = Layer 3 by default, with Layer 4 as the automatic fallback
- After each agent completes, it returns a structured research-log block in its final message. The orchestrator appends the block to `research-log.md` (see `references/shared/agent-dispatch.md`)
- Sequential execution remains valid — dispatch is a performance optimisation, not a requirement

See `references/shared/agent-dispatch.md` for full guidance: agent briefing templates, per-wave notes, handoff format, and timeout fallback. See `superpowers:dispatching-parallel-agents` skill for Claude Code implementation.

**Wave 2 minimum threshold:** If primary source identification returns fewer than 3 substantive sources for any KIT, pause and report to the user before proceeding to Wave 3. Do not proceed to synthesis on a thin evidence base.

See `references/shared/INTELLIGENCE-STANDARDS.md` Section 6 for the full wave execution rules.

---

## Source Saturation Rule

See `references/shared/INTELLIGENCE-STANDARDS.md` Section 5 for the source saturation rule. Module SCI's additional threshold (≥50% overlap) applies to academic searches and takes precedence over the global rule.

---

## Topic Type Modules

Select the module matching the classified topic type and follow its guidance throughout the investigation. Read `references/topic/topic-modules.md` for all seven modules, their KIT defaults, phase sets, sources, output formats and the multi-module chaining protocol.

---

## Phase 11: Synthesis and Expert Review

Every KIT must be answered explicitly. Read `references/topic/topic-synthesis.md` for the full synthesis procedure, expert QA, deliverables and PDF generation rules.

---

## Progress Reporting

After completing each wave, post a wave summary before beginning the next wave — this is required, not optional. See `references/shared/INTELLIGENCE-STANDARDS.md` Section 6 for the standard wave summary format and Wave 2 minimum threshold rules.

---


## Research Log Format

Append to `research-log.md` throughout the investigation. See `references/shared/INTELLIGENCE-STANDARDS.md` Section 13 for the standard research log format.

---

## Key Principles

1. **Scope before search.** The Topic Scoping Document is confirmed before any research begins.
2. **KITs drive everything.** Every phase asks: what does this tell us about each KIT?
3. **Provenance on everything.** Every fact cites its source with a confidence rating.
4. **Source independence.** Two organisations citing the same government report = one source.
5. **Gaps are intelligence.** Interpret every gap — don't just list it.
6. **Contested knowledge.** Where genuine expert disagreement exists, surface it — don't resolve it artificially.
7. **Layer 4 is an automatic fallback for the free browser read of a public page.** If a search or fetch has failed and Layers 1-3 give no other way, record the source and why, then run Playwright without asking (except a sensitive integrity investigation: confirm with the user first). Tier 1 and Tier 2 never use it. Log a gap when Playwright is not installed or the browser also fails. A 429 rate limit never switches to the browser. Use a paid scraping service only after the user says yes for it.
8. **Ethical boundaries.** All intelligence comes from publicly accessible sources. Respect each site's terms and robots rules. Privacy Act applies. Person research covers public role only.
9. **Actionable outputs.** The reader should know what to DO, not just what to THINK.
10. **Single source of truth.** Standards shared with company mode (STOP gate, KIT register, saturation rule, wave thresholds, progress reporting, synthesis sequence, deliverables, disambiguation) are defined in `references/shared/INTELLIGENCE-STANDARDS.md`. Do not duplicate them here — reference them.
