---
name: research-intel
description: "Deep, multi-source intelligence research on a named company or organisation, or on a named non-company topic: a market, industry, policy or regulation, technology, trend, community, academic field or public figure. Use this skill when the user asks for research, due diligence, a dossier, a deep dive, a brief or intelligence on a named subject, or names this skill. Triggers include: 'research [company]', 'due diligence on [company]', 'background on [company]', 'competitor analysis', 'who is [company]', 'prepare a dossier on [company]'. Also use when preparing for meetings with or pitches to a specific organisation. If the subject is ambiguous between a company and a topic, show the disambiguation prompt."
license: MIT
metadata:
  version: "1.0.0"
---

# Research Intelligence

## Overview

A structured intelligence-gathering skill that performs deep, multi-source research on a named company or organisation (company mode) or on any non-company topic (topic mode). It produces a dossier with provenance-tracked findings, organised into a navigable folder structure.

- The goal is actionable intelligence, not just information.
- Every claim needs a source and a confidence rating.
- Triangulate across independent sources. Do not trust any single source.

---

## Choose the mode

A named company or organisation means company mode. Any other subject means topic mode. Ask when the choice is unclear.

| Subject type | Mode |
|---|---|
| Named legal entity (company, charity, trust, government agency) | company mode |
| Policy, framework, standard, regulation | topic mode |
| Industry, market, or sector | topic mode |
| Technology category or product class (not one specific company) | topic mode |
| Geographic area or community | topic mode |
| Trend, movement, or time-bounded phenomenon | topic mode |
| Public figure (not a company they run) | topic mode |
| Scientific or academic field | topic mode |
| Ambiguous: could be company or topic | Show disambiguation prompt |
| Ambiguous: company mentioned but context suggests market/sector interest | Show disambiguation prompt |

**Disambiguation prompt** (show when ambiguous):
```
Is this research about the organisation itself, or the broader topic?

  C: The organisation [Name], company mode (dossier, people, financials)
  T: The broader topic or landscape, topic mode (market, policy, ecosystem)
  B: Both: company mode first, then topic mode for sector context

(C / T / B)
```

**If user selects C:** Use company mode.
**If user selects T:** Use topic mode.
**If user selects B:** Run company mode first for the entity, then topic mode for the sector context.

When the user names no subject, ask whether the research is about a company or a topic. Then show that mode's invocation menu.

**A change of mode starts a new investigation.** This applies to choice B and to every hand-off between modes. Run the new mode from its Phase 0, with its own dependency check, Scoping Document, STOP gates and output folder. A mode that paused for a hand-off resumes when the other dossier is done.

Full routing logic and natural hand-off points between modes are documented in `references/shared/INTELLIGENCE-STANDARDS.md` Section 14.

---

## Start every investigation

Do these in order after the mode is chosen. Read each file at the exact path, relative to this skill's folder.

1. Read `references/shared/INTELLIGENCE-STANDARDS.md` first, at the start of every investigation.
2. Read the mode file: `references/company/skill.md` or `references/topic/skill.md`.
3. Say the "Announce at start" line from the mode file.
4. Run that mode's dependency check.
5. Read `references/shared/web-access-layers.md` for the four-layer web access protocol.
6. Run Phase 0. Meet the stop gates below.

If a bundled reference under `references/` is missing, say so and carry on with this skill's own steps. A missing reference never removes a gate in this file.

Where a reference file says "both skills" or "each skill", read it as the two modes of this skill.

---

## Stop gates

At each stop gate, ask with the harness question tool when the user's own rules require it. Otherwise ask in plain text.

**The STOP gate rule:** Research does not begin until the user has confirmed the Scoping Document. Do not run any searches, WebFetch calls, or tool checks while awaiting confirmation. The temptation to begin research during scoping is the single most common failure mode. Resist it.

**Skipping the menu does not skip Phase 0.**

**Privacy checkpoint (company mode):** do not begin research until the privacy checkpoint is complete and recorded.

**Wave 2 threshold gate:** pause and report to the user before Wave 3 when the evidence is below the mode's minimum. The minimum is in the mode section below. Do not build synthesis on a thin evidence base.

**Progress reporting:** after completing each wave, post a wave summary before beginning the next wave. The format is in `references/shared/INTELLIGENCE-STANDARDS.md` Section 6.

**PDF generation gate:** generate the PDF only after all of these are true:

- The synthesis source register links every claim to a source.
- The Fabrication Detection Pass is complete with status CLEAR or ISSUES RESOLVED.
- No `[NUMERIC GAP - not sourced]` flags remain.
- All `[QA FLAG]` annotations are resolved.
- The triangulation matrix has no warning mark in the numeric verified column.

---

## Depth

- Ask the user to choose a depth before research starts.
- Use a depth named in the request without asking again.
- Quick Scan is the suggested depth.
- Each mode has its own depth menu. Use the menu for the chosen mode.

---

## Web access

Read `references/shared/web-access-layers.md` for the full protocol. Read `references/shared/layer-4-playwright-protocol.md` before using Layer 4.

- **Layer 1, API-first:** use public APIs where available.
- **Layer 2, search as router:** use WebSearch to find the exact URL, then fetch only that URL. Never crawl blind.
- **Layer 3, scraping service:** paid scraping services stay opt in. Use one only after the user says yes for it. A configured key is not a yes. Otherwise go to the free browser read (Layer 4). Never print a key value.
- **Layer 4, Playwright:** the automatic fallback, and only the free browser read of a public page after a failed search or fetch. When a web search or fetch fails, move to the browser without asking. Record the source and the reason in `research-log.md`.

Limits on Layer 4:

- Tier 1 and Tier 2 never use Layer 4.
- Tier 4 (Revenue Discovery) uses Layer 4 only after the user says yes for that source.
- Do not use Layer 4 on a source behind a login.
- Do not solve or bypass a captcha. The browser never signs in. Log a captcha as a gap unless a person can reach the page and paste its content.
- A 429 rate limit response never switches to the browser. Wait and retry later, or log the source as a gap.
- LinkedIn is not a Layer 4 target. Use only LinkedIn content that is publicly visible without login through Layers 1 to 3, or content the user pastes.
- Layer 4 runs as a normal browser that identifies as automation. Do not use stealth tools or session cookies.
- Respect each site's terms of service, robots rules and rate limits.
- For a sensitive integrity investigation, confirm with the user before any Layer 4.
- When Playwright is not installed, or the browser also fails, log the source as a gap with the reason.

---

## Shared research rules

- **Provenance on everything.** Every fact cites its source with a confidence rating.
- **Confidence:** apply two grades to every piece of information recorded: source reliability A to F and information credibility 1 to 6, written together (for example B2). See `references/shared/INTELLIGENCE-STANDARDS.md` Section 4.
- **Source independence matters.** Two sources citing the same original report = one source.
- **Saturation:** see `references/shared/INTELLIGENCE-STANDARDS.md` Section 5.
- **Research log:** append to `research-log.md` throughout. When parallel agents are used, each agent returns its block in its final message and the orchestrator appends the returned blocks. Agents do not write this file directly. See Section 13 of the standards.
- **Synthesis:** follow `references/shared/INTELLIGENCE-STANDARDS.md` Section 8. Produce the synthesis source register before any synthesis prose. Run the Fabrication Detection Pass (Section 17) before deliverables.
- **Gaps are intelligence.** Interpret every gap.
- **Ethical boundaries:** use public sources only. Do not misrepresent identity or access authenticated systems. Comply with the Privacy Act and other applicable law. Person research covers public role only. The full list is in `references/shared/INTELLIGENCE-STANDARDS.md` Section 16.

---

## Company mode

Read `references/company/skill.md` first. It holds the announce line, the phase summary, the company gates, the folder structure, the waves and the key principles.

Gates and depth rules:

- **STOP gate:** run no search, tool check or WebFetch call until the user confirms the Company Scoping Document.
- **STOP gate:** do not begin research until the privacy checkpoint is complete and recorded. The checkpoint is Phase 0 Step 7 in `references/company/phase-playbook.md`.
- Offer the depth after the company is identified. The menu is in `references/company/investigation-setup.md`.
- Wave 2 threshold: if fewer than 2 independent sources confirm the entity, or a KIT has no findings, pause and report before Wave 3.
- Run director and beneficial owner property or home address searches only when the privacy checkpoint confirms a due diligence purpose that needs them.
- Ask the user before buying any paid extract.
- Confirm the Revenue Discovery Scope with the user before research.

| When | Read |
|---|---|
| Start of every investigation: dependency check, disambiguation, menus, depth, tool check, folder tree | `references/company/investigation-setup.md` |
| Before each research phase | `references/company/phase-playbook.md` |
| Phase 0, entity type classification | `references/company/company-entity-modules.md` |
| Before Wave 1, when dispatching parallel agents | `references/company/company-agent-dispatch.md` |
| During and after the waves: progress, research log, ratings, saturation, refresh | `references/company/ongoing-protocols.md` |
| Phase 5 | `references/company/australian-data-sources.md` |
| Phase 5 and Phase 6 | `references/company/investigative-methods.md` |
| Phase 8 | `references/company/osint-techniques.md` |
| Phase 11: frameworks | `references/company/analytical-frameworks.md` |
| Phase 11: synthesis templates and deliverables | `references/company/synthesis-outputs.md` |
| Phase 11: expert panels, entity-type sets | `references/company/company-expert-panels.md` |
| Purpose based interactive panel (an `expert-panel` session, or a purpose view beyond the entity-type panels) | `references/company/company-expert-panels.md`, section "Purpose-Based Interactive Panel" |

---

## Topic mode

Read `references/topic/skill.md` first. It holds the announce line, the dependency check, the invocation menu, Phase 0, the waves, the folder structure and the key principles.

Gates and depth rules:

- **STOP:** do not run any searches, WebFetch calls, or tool checks until the user has confirmed the Topic Scoping Document.
- If type is ambiguous between two codes, ask the user to confirm before proceeding.
- If the user named a depth in the request, use that depth and skip the depth step.
- Wave 2 threshold: if primary source identification returns fewer than 3 substantive sources for any KIT, pause and report to the user before proceeding to Wave 3. Do not proceed to synthesis on a thin evidence base.
- Public figure research (PRS) covers only the person's public role. Private life is out of scope.

| When | Read |
|---|---|
| After classifying the topic type, before Phase 0 is complete: module KIT defaults, phases, sources, chaining | `references/topic/topic-modules.md` |
| When identifying sources | `references/topic/topic-sources.md` |
| When dispatching parallel agents | `references/topic/topic-agent-dispatch.md` |
| When using OSINT | `references/topic/topic-osint.md` |
| During analysis | `references/topic/topic-analytical-frameworks.md` |
| When producing outputs | `references/topic/topic-outputs.md` |
| Phase 11: synthesis, expert QA, deliverables, PDF | `references/topic/topic-synthesis.md` |
| Phase 11 QA: expert sets | `references/topic/topic-expert-panels.md` |
