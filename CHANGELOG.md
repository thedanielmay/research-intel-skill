# Changelog

## 0.1.0 - 2026-11-26

- Combined company-intel and topic-intel into one research-intel skill. The router dispatches to the company and topic bodies under `skills/research-intel/references/`.
- Moved the three shared reference files into `references/shared/` and copied each once after checking that both sources were identical.
- Moved the company and topic reference files, plus the files that sat beside each old SKILL.md, into `references/company/` and `references/topic/`.
- Did not copy the superseded `references/expert-panels.md`. The Phase 11 QA pointers now name `company-expert-panels.md`.
- Fixed every internal file path to the new layout. Replaced em dashes and en dashes inside printed code block templates only.
- Did not copy metadata.json files.
- Every key check covers the same four services and never prints a key. Keys stay in environment variables and out of the research log.
- Added `skills/research-intel/SKILL.md`: mode choice, the shared stop gates and rules, and the reading list for each mode.
- A change of mode now starts the new mode at its own Phase 0, with its own gates.
- Replaced the old skill names with `research-intel` company mode or topic mode. Removed the company dependency on the old topic skill, because both are now one skill.
- Changed rule: Layer 4 (browser) is now an automatic fallback when a web search or fetch fails. It no longer needs a yes for each source. Tier 1 and Tier 2 still never use it. Login, captcha, terms of service, rate limit and sensitive investigation rules still apply. Paid scraping services stay opt in.
- Added a line on how to ask at a stop gate.
- Fixed the tier line in agent briefings, which forbade the wrong layer.
- Added the purpose based interactive panel rules from the old company expert panels file to `references/company/company-expert-panels.md`.
