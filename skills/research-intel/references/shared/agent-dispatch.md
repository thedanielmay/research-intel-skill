# Agent Dispatch — Wave Parallelisation Guide

## Purpose

Guidance for parallelising company mode and topic mode investigation waves using agents. Agent dispatch converts the wave structure from aspirational parallelism to actual parallelism.

Agent dispatch is a performance optimisation, not a requirement. Sequential execution remains valid — all waves can be run in order without agents. Use this guide when investigation scope and available resources make parallel execution worthwhile.

**Reference:** See `superpowers:dispatching-parallel-agents` skill for Claude Code implementation details.

---

## Agent Briefing — Minimum Required Contents

### Company mode

Every agent dispatched from a company mode investigation must receive all of the following in its briefing:

1. **Confirmed Company Scoping Document** — full text of `00-summary/company-scoping.md`
2. **Entity type module** — which entity module (STA/SME/ASX/PEQ/NFP/GOV/FOR) was classified in Phase 0
3. **KIT list with current status** — each KIT labelled: answered / partial / unanswered
4. **Research log excerpt** — sources accessed so far (prevents duplication)
5. **Specific task for this agent** — named source targets, what to look for, return format
6. **Tier constraint** — "This is a Tier [N] investigation. Highest web layer: [2 | 3 | 4]. Tier 1 and Tier 2 never use Layer 4. Yes from the user for a paid scraping service: [yes, name of service | no]. Sensitive integrity investigation: [no | yes, browser confirmed by the user | yes, browser not confirmed]. Use Layer 4 only when the highest layer is 4 and a search or fetch has failed, and record the source and why. If the browser is not confirmed, do not use Layer 4. Without a yes, use no paid service. You cannot ask the user: report each blocked source in your final message. Layer 4 limits: no source behind a login, no LinkedIn, never solve a captcha, a captcha or bot check page is a gap, no stealth tools or session cookies, respect site terms, robots rules and rate limits; a 429 rate limit never switches to the browser." Fill every bracket before dispatch.
7. **Output instruction** — what to write back (see Handoff Format below)

Do not dispatch agents without the scoping document and research log.

### Topic mode

Every agent dispatched from a topic mode investigation must receive all of the following in its briefing prompt:

1. **Confirmed Topic Scoping Document** — full text of `00-summary/topic-scoping.md`
2. **KIT list with current status** — each KIT labelled: answered / partial / unanswered
3. **Research log excerpt** — sources accessed so far (prevents duplication of effort)
4. **Specific task for this agent** — named source targets, what to look for, what format to return
5. **Output instruction** — what to return in the final message and in what format (see Handoff Format below)
6. **Tier constraint**: "This is a Tier [N] investigation. Highest web layer: [2 | 3 | 4]. Tier 1 and Tier 2 never use Layer 4. Yes from the user for a paid scraping service: [yes, name of service | no]. Sensitive integrity investigation: [no | yes, browser confirmed by the user | yes, browser not confirmed]. Use Layer 4 only when the highest layer is 4 and a search or fetch has failed, and record the source and why. If the browser is not confirmed, do not use Layer 4. Without a yes, use no paid service. You cannot ask the user: report each blocked source in your final message. Layer 4 limits: no source behind a login, no LinkedIn, never solve a captcha, a captcha or bot check page is a gap, no stealth tools or session cookies, respect site terms, robots rules and rate limits; a 429 rate limit never switches to the browser." Fill every bracket before dispatch.

Do not dispatch agents without the scoping document and research log — agents without context will duplicate work and miss KIT-specific angles.


---

## Handoff Format

### Company mode

Each agent returns its research-log block in its final message. The orchestrator alone appends the returned blocks to `research-log.md`. Agents do not write that file:

```markdown
## Agent task: [brief description], [YYYY-MM-DD HH:MM]
Sources accessed: [N]
Layers used: [1/2/3/4]
KIT progress: KIT 1 [answered/partial/unanswered] | KIT 2 [...] | KIT 3 [...]
Key findings: [2-3 sentences: most significant findings]
Data-currency flags: [any ASIC, ACNC, or registry data where staleness may affect confidence]
Gaps logged: [N: reference inaccessible-sources.md for details]
New sources to follow up: [URLs or source names surfaced but not yet accessed]
```

### Topic mode

Each agent returns its research-log block in its final message. The orchestrator alone appends the returned blocks to `research-log.md`. Agents do not write that file, and Explore agents cannot write files:

```markdown
## Agent task: [brief description], [YYYY-MM-DD HH:MM]
Sources accessed: [N]
Layers used: [1/2/3/4]
KIT progress: KIT 1 [answered/partial/unanswered] | KIT 2 [...] | KIT 3 [...]
Key findings: [2-3 sentences summarising the most significant findings]
Gaps logged: [N: reference inaccessible-sources.md for details]
New sources to follow up: [any URLs or source names surfaced but not yet accessed]
```


---

## Wave-Level Parallelisation Notes

### Wave 1 — Foundation (do NOT parallelise)

#### Company mode

Wave 1 confirms the entity, classifies entity type, establishes KITs, and produces the Company Scoping Document. All later agents need this document in their briefing. Do not dispatch agents before Wave 1 is complete.

#### Topic mode

Wave 1 must complete before any agents are dispatched. It confirms the topic, establishes KITs, and produces the scoping document that all agents need in their briefing. Running agents before the scoping document is confirmed is the most common failure mode in parallelised investigations.

### Wave 2 — Primary Research (parallelise across KITs and source types)

**Parallelisable tasks:**

#### Company mode

- Website analysis and digital footprint core (Phase 2 + Phase 8 initial)
- News and media search (Phase 3)
- LinkedIn company page and founder profiles (Phase 4)
- Corporate registries core — ASIC + ABR + entity-type registries (Phase 5 core)

**Recommended agent type:** Explore (read-only source discovery)

**Per-agent brief structure:**

- **Agent A (Website + Digital):** "Analyse [company]'s website at [URL]. Map all pages, extract team, products, clients, messaging, job openings. Also perform DNS reconnaissance (A/MX/TXT records) and check crt.sh for subdomains. Record all findings with confidence ratings."
- **Agent B (News + Media):** "Search for all news, media coverage, podcasts, and video content mentioning [company name] and [founder names]. Date range: all available. Include: mainstream media, industry press, Trove (historical), SmartCompany, AFR. Log each source with date, publication, and summary."
- **Agent C (LinkedIn):** "Research [company]'s LinkedIn page and key people profiles. Extract: employee count and growth trend, team composition by function, leadership profiles (career history, education, tenure), job postings, recent posts and engagement. Note: LinkedIn notifications apply — see references/shared/INTELLIGENCE-STANDARDS.md §16."
- **Agent D (Registries Core):** "Search ASIC, ABR, and [entity-type-specific registry] for [company]. Extract: full company extract, director history, document history (check for late lodgements), share structure, registered charges (PPSR). Flag any data with known staleness (see references/company/company-entity-modules.md data-currency note for this entity type)."

**Wave 2 minimum threshold gate:** If entity verification (Phase 1) returns fewer than 2 independent sources confirming the entity, or any KIT has zero substantive findings after Wave 2, pause and report to the user before Wave 3. Do not build synthesis on a thin evidence base.

#### Topic mode

- Primary source identification — one agent per KIT
- Stakeholder/actor mapping — one agent
- Data and statistics search — one agent

**Recommended agent type:** Explore (read-only source discovery). Each agent returns the research-log block in its final message. The orchestrator appends the block to `research-log.md`.

**Per-agent brief structure:**

- Agent A (KIT 1 primary sources): "Search the following sources for evidence relevant to KIT 1: [source list]. Record every source accessed, confidence rating, and key finding."
- Agent B (KIT 2 primary sources): same structure for KIT 2
- Agent C (stakeholder mapping): "Identify all significant organisations, individuals, and networks relevant to [topic]. Sources: [source list]. Map relationships where evident."
- Agent D (data/statistics): "Find quantitative data relevant to [topic]. Priority: official government statistics, then industry data, then secondary estimates. Record source, date, methodology where stated."

**Wave 2 minimum threshold gate:** If any agent returns fewer than 3 substantive sources for its assigned KIT, pause and report to the user before proceeding to Wave 3. Do not build synthesis on a thin evidence base.

### Wave 3 — Secondary Research (parallelise by source type)

**Parallelisable tasks:**

#### Company mode

- People deep profiling — Tier 1 people (Phase 6)
- Reputation research — reviews, Glassdoor (Phase 7)
- Financial and legal deep dive — AustLII, grants, procurement (Phase 5 deep)
- Social media beyond LinkedIn (Phase 4b)
- Digital footprint depth — WHOIS, ad library, website archaeology (Phase 8 deep)

**Recommended agent type:** Explore

**Per-agent brief structure:**

- **Agent E (People Deep):** "Build Tier 1 profiles for [names]. For each: full career reconstruction across independent sources, board and advisory roles (ASIC cross-search), ASIC disqualified director check, AEC political donations, AustLII court records. Apply Thread-Chasing: career thread (all prior employers), board thread (all boards), education thread."
- **Agent F (Reputation):** "Research [company]'s reputation from: Google Reviews, Glassdoor (company and interview reviews), ProductReview, Whirlpool, Reddit. Also search for: '[company name] complaint', '[company name] lawsuit', '[company name] scam'. Record sentiment, recurring themes, and any specific incidents."
- **Agent G (Financial Deep):** "Search AustLII for all court and tribunal records involving [company name] and [director names]. Search GrantConnect and AusTender for full grant and contract history. Search Fair Work Commission. Search ACCC enforcement register. Extract all findings with source, date, and outcome."

#### Topic mode

- Media and community sources — one agent
- Policy/regulatory/technical depth — one agent (module-specific)
- Cross-reference — one agent

**Recommended agent type:** Explore. Each agent returns the research-log block in its final message. The orchestrator appends the block to `research-log.md`.

**Per-agent brief structure:**

- Agent E (media/community): "Search news archives, trade press, and community sources for coverage of [topic] from [date range]. Focus on: [specific angles from KIT list]."
- Agent F (policy/technical depth): "Conduct depth research on [specific policy/technical aspect identified in Wave 2]. Primary sources first, then secondary. Access via [layer guidance based on tier]."
- Agent G (cross-reference): "Cross-reference the following Wave 2 findings against independent sources. Identify any contradictions or where a single original source is being cited multiple times."

### Wave 4 — Analysis (parallelise by framework)

**Parallelisable tasks:**

#### Company mode

- Analytical frameworks (SWOT, Porter's Five Forces per `references/company/analytical-frameworks.md`)
- People profiles synthesis — Tier 2 and 3 (Phase 6)
- Customers and stakeholders mapping (Phase 9)
- Industry and market context (Phase 10)

**Recommended agent type:** General-purpose (analysis and writing)

**Per-agent brief structure:**

- **Agent H (Framework analysis):** "Apply [framework] to the following evidence set from the research log. Template: [paste from references/company/analytical-frameworks.md]. Evidence: [paste relevant research log sections]. Include a 'So What?' paragraph connecting the framework output to the stated purpose: [paste from company-scoping.md]."
- **Agent I (Industry/Market):** "Research the competitive and market context for [company]. Identify: direct and indirect competitors, competitive positioning, market dynamics, barriers to entry. Use topic mode (Module MKT) if the sector itself is a primary KIT. Otherwise run Phase 10 scope only."

#### Topic mode

- Analytical framework application — one agent per major framework required by the module
- Synthesis drafts — one agent per major deliverable section

**Recommended agent type:** General-purpose (analysis and writing). Each agent returns the research-log block in its final message. The orchestrator appends the block to `research-log.md`.

**Per-agent brief structure:**

- Agent H (framework): "Apply [framework name] to the following evidence set. Template: [paste relevant framework from topic-analytical-frameworks.md]. Evidence: [paste relevant research log sections]."
- Agent I (synthesis draft): "Draft [KIT Answer Register / Critical Findings / Executive Summary] using the evidence in the research log. Template: [paste relevant section from topic-outputs.md]."

### Wave 5 — Synthesis and QA (partial parallelisation)

**Partially parallelisable:**

- Expert QA review — one agent per expert persona (Mode 3 pre-output QA)
- Deliverable file generation — one agent per output file

#### Company mode

**Sequential gate:** Expert QA (Mode 3) must complete before PDF generation. All `[QA FLAG]` annotations must be resolved before invoking `document-skills:pdf`.

**Per-agent brief structure:**

- **Agent J (Expert QA):** "Review the attached synthesis draft as [Expert Name, role from references/company/company-expert-panels.md]. You are performing Mode 3 Pre-Output QA. Identify gaps, weak analysis, missing intelligence, or unsupported claims in your domain. Flag issues as [QA FLAG: Expert Name — description]. Do not soften findings — unresolved objections are valuable outputs."
- **Agent K (Deliverable generation):** "Write [specific deliverable] to [file path] using this template: [paste from references/company/synthesis-outputs.md]. Evidence base: [paste relevant synthesis sections]."

#### Topic mode

**Sequential gate:** Expert QA (Mode 3) must complete before PDF generation. Expert `[QA FLAG]` annotations must be resolved before invoking `document-skills:pdf`.

**Recommended agent type:** General-purpose. Each agent returns the research-log block in its final message. The orchestrator appends the block to `research-log.md`.

**Per-agent brief structure:**

- Agent J (Expert QA — Expert 1): "Review the attached synthesis draft as [Expert Name, role]. Identify gaps, weak analysis, or missing intelligence in your domain. Flag issues as [QA FLAG: description]. Template: [paste expert profile from references/topic/topic-expert-panels.md]."
- Agent K (deliverable generation): "Write the [specific deliverable] to [file path] using this template: [paste template from topic-outputs.md]. Evidence: [paste relevant synthesis sections]."


---

## Timeout and Fallback

### Company mode

- **Research agents (Waves 2-3):** Log incomplete tasks in research log: "Agent timed out — [task] incomplete." Continue without; note the gap in `inaccessible-sources.md`.
- **Analysis agents (Wave 4):** Run the framework sequentially in the main session if agent times out.
- **QA agents (Wave 5):** Do not skip QA. Run the expert review sequentially using the persona from `references/company/company-expert-panels.md` if the agent times out.

Sequential execution is always a valid fallback.

### Topic mode

- **Standard research agents (Waves 2-3):** Proceed after a reasonable wait. Each agent returns a timeout note in its final message: "Agent timed out: [task] incomplete." The orchestrator appends the note to `research-log.md`.
- **Analysis agents (Wave 4):** If an agent does not return, run that framework sequentially in the main session.
- **QA agents (Wave 5):** Do not skip QA. If an agent times out, run the expert review sequentially using the persona from `references/topic/topic-expert-panels.md`.

Sequential execution is always a valid fallback. The wave structure works without agents — dispatch is a performance optimisation, not a dependency.


---

## Source Effort Caps by Tier

Control which web access layers agents may use based on investigation tier. Brief agents with their tier constraint:

| Tier | Max layer | Playwright (Layer 4) | Scraping service (Layer 3) |
|---|---|---|---|
| Tier 1 — Quick Scan | Layer 2 | Never | Never |
| Tier 2 — Standard | Layer 3 | Never | Only after the user says yes |
| Tier 3 — Full Investigation (company mode) | Layer 4 | Yes | Only after the user says yes |
| Tier 3 — Full Investigation (topic mode) | Layer 3 by default, Layer 4 on failure | Yes, as automatic fallback | Only after the user says yes |

Include the tier constraint in every agent briefing: "This is a Tier [N] investigation. Highest web layer: [2 | 3 | 4]. Tier 1 and Tier 2 never use Layer 4. Yes from the user for a paid scraping service: [yes, name of service | no]. Sensitive integrity investigation: [no | yes, browser confirmed by the user | yes, browser not confirmed]. Use Layer 4 only when the highest layer is 4 and a search or fetch has failed, and record the source and why. If the browser is not confirmed, do not use Layer 4. Without a yes, use no paid service. You cannot ask the user: report each blocked source in your final message. Layer 4 limits: no source behind a login, no LinkedIn, never solve a captcha, a captcha or bot check page is a gap, no stealth tools or session cookies, respect site terms, robots rules and rate limits; a 429 rate limit never switches to the browser."

### Topic mode

Layer 4 is the automatic fallback at Tier 3, the free browser read of a public page. An agent that needs it records the source and why, then runs it without asking. In a sensitive integrity investigation, the orchestrator gets the user's confirmation before dispatch. A 429 rate limit never switches to the browser. If Playwright is not installed, or the browser also fails, the agent logs the source as a gap with the reason and moves on.
