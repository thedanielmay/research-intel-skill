# Company Expert Panels

## Purpose

Provides embedded QA expert personas for Phase 11 synthesis review. Distinct from the interactive `expert-panel` skill — these are structured reviewers applied during synthesis, not a full adversarial panel session.

Read this file at the start of Phase 11 (Synthesis) for every Full Investigation (Tier 3). Select the expert set matching the entity type classified in Phase 0.

---

## Integration Modes

### Mode 1 — Synthesis Reviewer (default, always invoke for Full Investigations)

During Phase 11, before generating any deliverable, route the draft synthesis through the 2-3 most relevant experts for the entity type. Each expert reviews their domain and flags gaps, weak analysis, or missing intelligence.

**When:** Always, during Phase 11 synthesis. Select experts matching the entity module.

### Mode 2 — On-Demand Consultant

Available during any research phase when unexpected findings, thin results, or contradictory information appear in a specific expert's domain. Invoke by asking: "What would [Expert Name] look for here?"

**When:** When a phase produces unexpected, thin, or contradictory results in a specific domain.

### Mode 3 — Pre-Output QA (Full Investigation only)

Before generating the final PDF synthesis, route the complete dossier through ALL three experts for the entity type. Each reviews the full output from their perspective and flags critical omissions as `[QA FLAG]` annotations.

**When:** Always, as the final step before invoking `document-skills:pdf`. Reviews outputs only — does not re-run research phases.

**Scope:** the full synthesis output, including the RAG Dashboard, the Engagement Playbook and the Competitive Profile.

---

## Expert Sets by Entity Module

### STA — Startup / Early-Stage

**Expert 1: Startup Investor**
- Persona: Partner at an Australian VC or early-stage fund, 12+ years evaluating and backing startups across tech, climate, and enterprise software sectors
- Domain: Funding history accuracy, team quality signals, product-market fit evidence, runway and burn assessment
- Key questions:
  - "Is the funding history consistent across Crunchbase, LinkedIn, and media — or are there discrepancies?"
  - "Has the founding team's prior track record been verified, not just described?"
  - "Is product-market fit assessed from user evidence or from the company's own claims?"
  - "Has burn rate been estimated — does the funding runway match the team size and spend signals?"
  - "Have the co-investors been profiled — do they signal smart money or warm money?"
- Will challenge: PMF claims without user evidence, funding history from company self-report only, missing competitor context
- Invoke in Mode 2 when: Phase 6 (People) returns thin founder track record, or funding history cannot be verified across two independent sources

**Expert 2: Early-Stage Customer**
- Persona: Head of Innovation or technology buyer at an ASX200 enterprise, 10+ years evaluating and piloting emerging technology vendors
- Domain: Enterprise readiness signals, user evidence quality, reference customer credibility, procurement risk
- Key questions:
  - "Are the reference customers named — and are they credible validators for this company's claims?"
  - "Has the product been independently reviewed — G2, Capterra, App Store reviews?"
  - "Is there evidence of renewal — or only evidence of first sales?"
  - "Have the integration and security requirements been assessed from a buyer's perspective?"
  - "What is the exit risk — what happens to the customer if this startup fails?"
- Will challenge: Customer proof from company case studies only, missing independent user reviews, enterprise readiness not assessed
- Invoke in Mode 2 when: Phase 9 (Customers) returns only company-provided testimonials

**Expert 3: Technical Due Diligence Analyst**
- Persona: CTO or principal engineer with 15+ years, 5+ years conducting technical due diligence for PE/VC
- Domain: Technology stack quality, IP ownership, technical team depth, code quality signals
- Key questions:
  - "Has the GitHub or public code repository been reviewed for team size, commit activity, and code quality signals?"
  - "Is the technology proprietary or commodity — is there a defensible IP moat?"
  - "Has the technical team been assessed separately from the founding team?"
  - "Are there any patents filed — and are they granted or only applied for?"
  - "Has the infrastructure dependency map been assessed — is this AWS-native with standard tooling, or is there custom infrastructure risk?"
- Will challenge: Technology claims without technical evidence, IP described without checking patent register, team depth not assessed beyond founders
- Invoke in Mode 2 when: Phase 8 (Digital Footprint) does not include code repository analysis

---

### SME — Private SME

**Expert 1: SME Credit Analyst**
- Persona: Senior credit analyst at a major Australian bank, 12+ years assessing SME lending risk
- Domain: Financial health signals, debt position, PPSR charges, cash flow proxies, insolvency risk
- Key questions:
  - "Has PPSR been checked for registered charges — security interests reveal the debt load?"
  - "Have the ASIC document filings been checked for late lodgements — a pattern of late lodgement signals financial stress?"
  - "Has the government contract history (AusTender) been used to proxy revenue — and has it trended up or down?"
  - "Are there any director disqualifications or related-entity insolvencies in the ownership network?"
  - "Has the industry-specific credit risk been assessed — some sectors have structural default rates?"
- Will challenge: Financial assessment without PPSR, missing director disqualification check, no proxy revenue estimate
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) is thin on financial signals or PPSR was not accessed

**Expert 2: Fair Work and Employment Lawyer**
- Persona: Senior employment lawyer, 12+ years advising SMEs and employees on Fair Work matters
- Domain: Employment compliance, Fair Work Commission decisions, contractor classification, underpayment risk
- Key questions:
  - "Has the Fair Work Commission decision register been searched for decisions involving this employer?"
  - "Has the ASIC banned and disqualified register been checked for all current and historical directors?"
  - "Has Modern Slavery Act reporting been checked — applies to entities with >$100m revenue?"
  - "Are there known underpayment investigations — ASIC or media reports?"
  - "Has the employment model been assessed — high contractor use may signal classification risk?"
- Will challenge: Legal section that only covers litigation, missing Fair Work and employment compliance angle
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) does not include Fair Work Commission search

**Expert 3: B2B Sales Director**
- Persona: State sales director at a professional services firm, 15+ years selling to SMEs and mid-market
- Domain: Customer acquisition and retention signals, procurement patterns, buying centre mapping
- Key questions:
  - "Has the customer profile been built from evidence — procurement records, case studies, directory listings — or inferred?"
  - "Is there a government procurement component — AusTender and state portal history?"
  - "Have the sales channels been identified — direct, channel partner, platform?"
  - "Is there evidence of customer concentration risk — do 1-3 customers make up most revenue?"
  - "Has the sales leadership been assessed — is there a CRO or VP Sales, or is the founder still selling?"
- Will challenge: Customer section built from website testimonials only, missing procurement history analysis
- Invoke in Mode 2 when: Phase 9 (Customers) does not include AusTender search

---

### ASX — ASX-Listed Company

**Expert 1: Equity Research Analyst**
- Persona: Senior equity analyst covering ASX-listed companies, 12+ years at a major investment bank or research house
- Domain: Financial performance analysis, analyst consensus, valuation, capital allocation, earnings quality
- Key questions:
  - "Has the financial analysis used the full financial statements — P&L, balance sheet, and cash flow — or only headline figures?"
  - "Has the earnings quality been assessed — are reported earnings inflated by non-cash items or one-off gains?"
  - "Has revenue by segment been analysed where disclosed?"
  - "Has the capital allocation track record been assessed — acquisitions, buybacks, dividend policy?"
  - "Is the analyst consensus captured — and has it been compared to the company's own guidance?"
- Will challenge: Financial summary without cash flow analysis, no earnings quality assessment, missing analyst consensus context
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) does not include cash flow analysis

**Expert 2: Corporate Governance Specialist**
- Persona: Senior governance advisor at a proxy advisory firm (ISS, CGI Glass Lewis), 10+ years advising institutional shareholders
- Domain: Board composition, independence, remuneration alignment, related party transactions, disclosure quality
- Key questions:
  - "Has the board composition been assessed for independence — are the independent directors genuinely independent?"
  - "Has the executive remuneration structure been assessed for alignment with long-term shareholder value?"
  - "Are there related party transactions disclosed — and are they on arm's length terms?"
  - "Has the company's disclosure quality been assessed — do they disclose more or less than required?"
  - "Have any significant votes against management resolutions at the AGM been noted?"
- Will challenge: Board section that only describes rather than assesses, no remuneration structure analysis, missing related party transaction check
- Invoke in Mode 2 when: Phase 6 (People) does not assess board independence

**Expert 3: Institutional Investor Relations Advisor**
- Persona: Senior IR advisor with 12+ years working with ASX-listed companies and institutional investors
- Domain: Shareholder register, institutional sentiment, retail vs. institutional composition, activist risk
- Key questions:
  - "Has the shareholder register been mapped — who are the substantial holders and what are their interests?"
  - "Have any activist or activist-adjacent shareholders been identified?"
  - "Has the trading pattern been assessed — is there unusual volume or short interest?"
  - "Have the institutional holders been compared to sector peers — are the right funds holding this stock?"
  - "Has any recent change in register been identified — are long-term holders selling?"
- Will challenge: People section that misses major shareholders as key stakeholders, no register analysis
- Invoke in Mode 2 when: Phase 9 (Customers/Stakeholders) does not map the shareholder register

---

### PEQ — PE-Backed / VC-Backed (Mature)

**Expert 1: PE Investment Professional**
- Persona: Principal or Director at an Australian PE fund, 10+ years executing buyouts and portfolio company transformations
- Domain: Ownership structure, fund strategy and vintage, exit timeline, value creation thesis
- Key questions:
  - "Has the fund vintage been identified — a 2019 fund is under exit pressure; a 2023 fund is still building?"
  - "Has the ownership chain been traced to the ultimate fund entity?"
  - "Have other portfolio companies in the same fund been identified — do they signal a pattern?"
  - "Has the management team composition been compared to pre-acquisition — how many were replaced?"
  - "Have add-on acquisitions been identified via ASIC and media — they are a primary PE value creation lever?"
- Will challenge: PE ownership described without fund vintage, missing portfolio pattern analysis, no management change tracking
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) does not trace ownership to ultimate fund entity

**Expert 2: Portfolio Company CFO**
- Persona: CFO with experience at 3+ PE-backed portfolio companies, 15+ years in private company finance
- Domain: PE financial structures, debt covenants, EBITDA management, management incentive plans
- Key questions:
  - "Has PPSR been checked for the debt security structure — PE deals involve significant leverage?"
  - "Has the EBITDA proxy been estimated — multiple of revenue and cost signals?"
  - "Are there management incentive plan signals — LinkedIn titles like 'Principal' or 'Partner' on the leadership team?"
  - "Has the acquisition price been estimated from available signals (multiples, sector comparables)?"
  - "Are there cost-out signals — Glassdoor reviews mentioning restructuring, job postings showing role rationalisation?"
- Will challenge: PE financial analysis without PPSR or debt structure assessment, no EBITDA estimation
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) does not include PPSR or proxy financial analysis

**Expert 3: M&A Lawyer**
- Persona: Partner at a top-tier M&A practice, 15+ years advising on PE transactions and portfolio company exits
- Domain: Corporate structure, ASIC compliance, pre-exit preparation signals, legal risk in portfolio companies
- Key questions:
  - "Has the ASIC document history been checked for recent restructuring activity — amalgamations, share transfers?"
  - "Has the registered charge history been checked — PPS Register reveals lender security interests?"
  - "Have any litigation signals been identified — a company preparing for exit cleans up legal issues?"
  - "Have any ASIC compliance issues been flagged — late lodgements in a PE company are unusual and signal problems?"
  - "Has the company structure complexity been assessed — multiple layers of SPVs and holding companies are a red flag?"
- Will challenge: Legal section without ASIC document history review, no charge registration analysis
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) does not review ASIC document history for recent activity

---

### NFP — NFP, Charity, or Social Enterprise

**Expert 1: NFP Financial Analyst**
- Persona: Senior auditor or financial advisor, 12+ years working exclusively with charities and NFPs in Australia
- Domain: ACNC financials, revenue diversification, reserves adequacy, grant cliff risk, financial sustainability
- Key questions:
  - "Has the revenue concentration been analysed — what share comes from the top 1, 3, and 5 funding sources?"
  - "Has the reserves ratio been calculated — how many months of operating expenditure are held in unrestricted reserves?"
  - "Has the grant cliff risk been assessed — are major grants ending in the next 12-24 months?"
  - "Has the workforce cost ratio been checked — for most social services NFPs, it should be 55-75%?"
  - "Has the ACNC Annual Information Statement been reviewed for governance declarations?"
- Will challenge: Financial section without reserves ratio, no grant cliff analysis, workforce cost ratio not calculated
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) does not calculate reserves ratio or revenue concentration

**Expert 2: Social Impact Evaluator**
- Persona: Senior evaluator or impact measurement specialist, 10+ years designing and assessing social impact programs
- Domain: Mission delivery evidence, outcome measurement, program logic, beneficiary voice
- Key questions:
  - "Is the mission delivery assessed from independent evidence — evaluation reports, ACNC AIS outcomes — or from the organisation's own claims?"
  - "Has the beneficiary voice been represented in the intelligence — what do beneficiaries say, not just funders and management?"
  - "Has the theory of change been articulated — is there a logical pathway from activities to outcomes?"
  - "Have the counterfactual claims been assessed — would beneficiaries achieve similar outcomes without this organisation?"
  - "Have the organisation's self-reported impact metrics been triangulated against any independent evidence?"
- Will challenge: Mission delivery section built entirely from the organisation's own materials, no independent evaluation evidence
- Invoke in Mode 2 when: Phase 9 (Customers/Stakeholders) Beneficiary Analysis section is thin or sourced only from organisation materials

**Expert 3: Community Sector Regulator**
- Persona: Former ACNC investigator or state peak body compliance officer, 10+ years in NFP governance and compliance
- Domain: ACNC compliance, governance obligations, sector-specific regulation, related entity complexity
- Key questions:
  - "Has the ACNC compliance history been checked — any formal warnings, compliance agreements, or revocations?"
  - "Has the related entity structure been mapped — NFPs often have complex structures with trading arms, subsidiaries, and associated entities?"
  - "Have the sector-specific regulatory obligations been identified — NDIS, Aged Care, Housing, Mental Health?"
  - "Has the board composition been assessed for governance obligations — skills, independence, declared conflicts?"
  - "Have the annual reporting obligations been met — is the most recent AIS lodged and publicly available?"
- Will challenge: Compliance section without ACNC compliance history, complex group structure not mapped, no sector-specific regulatory check
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) does not check ACNC compliance history or group structure

---

### GOV — Government Agency or Statutory Body

**Expert 1: Public Sector Auditor**
- Persona: Senior performance auditor at ANAO or a state audit office (VAGO, NSWAO), 12+ years conducting performance and financial audits
- Domain: Budget accuracy, audit findings, program effectiveness, financial management quality
- Key questions:
  - "Have ANAO or state audit body performance audits of this agency been located and reviewed?"
  - "Has the budget trend been checked against Portfolio Budget Statements for 3+ years?"
  - "Have any qualified audit opinions been identified in the agency's financial statements?"
  - "Has the proportion of discretionary vs. statutory spending been assessed — this shapes the agency's flexibility?"
  - "Has the Senate Estimates transcript history been checked — agencies reveal operational details under questioning?"
- Will challenge: Budget analysis without multi-year trend, no audit history review, Senate Estimates not searched
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) does not reference Portfolio Budget Statements or ANAO

**Expert 2: Regulatory Policy Advisor**
- Persona: Former senior policy officer or regulatory advisor, 15+ years at federal or state government agencies
- Domain: Legislative mandate, ministerial relationship, policy direction, regulatory culture
- Key questions:
  - "Has the enabling legislation been directly accessed — not described from secondary commentary?"
  - "Has the current Minister's stated policy direction for this agency been captured — ministerial speeches and media releases?"
  - "Has the agency's enforcement culture been assessed — proactive vs. reactive, principle-based vs. rules-based?"
  - "Have the agency's formal consultation documents been reviewed — these reveal its working priorities?"
  - "Is there evidence of any tension between the agency's mandate and its current political direction?"
- Will challenge: Policy section built from agency website rather than primary legislation and ministerial direction
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) does not reference enabling legislation directly

**Expert 3: Senate Estimates Specialist**
- Persona: Senior parliamentary adviser or journalist who covers Senate Estimates, 10+ years tracking agency accountability
- Domain: Agency accountability, political pressure, leadership under scrutiny, disclosed vs. undisclosed problems
- Key questions:
  - "Have the most recent 2-3 Senate Estimates transcripts been reviewed for this agency?"
  - "Have any Senate committee recommendations directed at this agency been found — and has the agency responded?"
  - "Have any FOI disclosures involving this agency been located via Right to Know (righttoknow.org.au)?"
  - "Has media coverage of the agency's Senate appearances been reviewed?"
  - "Have whistleblower or protected disclosure signals been checked — public interest disclosures sometimes surface in media?"
- Will challenge: Government agency brief without Senate Estimates analysis, no FOI disclosure check
- Invoke in Mode 2 when: Phase 3 (News/Media) does not include Senate Estimates coverage

---

### FOR — Foreign Subsidiary or Branch

**Expert 1: International M&A and FDI Advisor**
- Persona: Senior cross-border M&A advisor or foreign investment specialist, 12+ years advising on inbound investment to Australia
- Domain: Parent entity profile, subsidiary role, FIRB implications, capital repatriation structures
- Key questions:
  - "Has the parent entity's annual report been reviewed for Australia-specific references?"
  - "Has FIRB clearance been confirmed — foreign acquisitions above threshold require FIRB approval?"
  - "Has the repatriation structure been considered — intercompany loans and management fees affect Australian entity economics?"
  - "Is the subsidiary financially material to the parent — or is Australia a small market for them?"
  - "Has the parent's strategic intent for Australia been assessed — growth, maintenance, or exit?"
- Will challenge: Foreign subsidiary analysis that doesn't reference parent entity filings, no FIRB check
- Invoke in Mode 2 when: Phase 5 (Financial/Legal) does not review parent entity filings

**Expert 2: Australian Corporate Lawyer**
- Persona: Partner at an Australian law firm, 12+ years advising foreign entities operating in Australia
- Domain: Australian regulatory obligations for foreign companies, ASIC foreign company registration, local director requirements
- Key questions:
  - "Has the ASIC foreign company extract been reviewed — local directors and registered office are required?"
  - "Have the Australian regulatory obligations been assessed — does this entity need an AFS licence, ACL, or other Australian authorisation?"
  - "Has the transfer pricing risk been considered — ATO scrutiny of related-party transactions between parent and subsidiary?"
  - "Has the employment structure been reviewed — is the Australian entity employing local staff directly?"
  - "Have the local director obligations been confirmed — at least one Australian resident director is required?"
- Will challenge: Foreign subsidiary profile without ASIC foreign company registration review, no Australian regulatory obligation assessment
- Invoke in Mode 2 when: Phase 1 (Identification) does not confirm Australian registered entity type

**Expert 3: Country Manager / Regional Director**
- Persona: Experienced country manager or regional director, 15+ years leading Australian operations of international companies
- Domain: Local authority and decision-making power, commercial autonomy, Australian team quality, HQ relationship
- Key questions:
  - "Has the local leadership's authority level been assessed — do they have P&L responsibility or are they a sales office?"
  - "Has the Australian team size and composition been estimated from LinkedIn?"
  - "Has the headquarters relationship been assessed — how much local autonomy does the Australian operation have?"
  - "Have the local commercial achievements been identified — growth, contracts won, market position?"
  - "Is there evidence of tension between local needs and HQ direction — Glassdoor often surfaces this?"
- Will challenge: Foreign subsidiary brief that describes the parent but not the local team authority and autonomy
- Invoke in Mode 2 when: Phase 6 (People) does not assess local leadership authority relative to parent entity

---

## Purpose-Based Interactive Panel

The sections below apply to the purpose based interactive panel. Use them in an interactive `expert-panel` skill session, or when Phase 11 QA for a specific purpose orientation is needed beyond the entity-type panels. They do not replace the entity-type panels above. "IS.md" means `references/shared/INTELLIGENCE-STANDARDS.md`.

### The Six Experts

#### 1. Competitive Intelligence Analyst
**Persona:** Senior CI practitioner, 15 years at firms like Kroll, FTI Consulting, and McKinsey competitive strategy.

**Domain:** Analytical rigour, intelligence methodology, decision-relevance of findings.

**Specialises in:**
- Intelligence Requirements Definition (IRD) — ensuring the investigation answers the right questions
- Analysis of Competing Hypotheses (ACH) — evaluating ambiguous or contradictory findings
- Forward-looking analysis — scenario planning, early warning indicators, strategic implications
- Capability assessment — distinguishing what a company *has* from what it can *do*
- Value chain analysis — operational strengths and vulnerabilities
- Source-type weighting — ensuring triangulation reflects true source independence
- Currency assessment — flagging stale data that may mislead

**Key questions this expert asks:**
- "What decision is this intelligence supporting? Is the analysis oriented toward that decision?"
- "What is the competing hypothesis? What evidence contradicts the primary narrative?"
- "Are we confusing information comprehensiveness with analytical depth?"
- "Is the confidence rating justified by truly independent sources?"
- "What are the three things that actually matter for the decision-maker?"

**Invoke when:** Synthesis feels comprehensive but unfocused. Contradictory evidence found. User needs forward-looking strategic assessment. Triangulation matrix has confidence ratings that feel inflated.

---

#### 2. Investigative Journalist
**Persona:** Senior investigative journalist, 20 years at The Age, The Guardian Australia, and ABC Four Corners. Specialises in corporate investigations, fraud, money flows, and power networks.

**Domain:** Finding what companies and people are actively trying to hide. Deep Australian public records expertise.

**Specialises in:**
- Historical ASIC document analysis ("Shedding Skin" technique) — tracking name changes, director rotations, address shifts
- Related entity constellation mapping — finding all companies sharing directors, addresses, auditors
- Property and land title searches — verifying ownership claims and revealing encumbrances
- PPSR analysis — revealing secured creditors and the true financial picture
- Insolvency and disqualification searches — AFSA, ASIC Published Notices, Banned and Disqualified Register
- Beneficial ownership tracing — following corporate chains, trust structures, nominee arrangements
- Court and tribunal records — AustLII, Federal Court, state tribunals, Fair Work Commission
- Political donations and lobbyist registers — AEC transparency, lobbyist registers
- Wayback Machine as investigative tool — deleted pages, removed team members, retracted claims
- The "Inversion Question" — "What would the evidence look like if the opposite were true?"
- Narrative consistency audit — cross-checking a company's story across all sources and over time

**Key questions this expert asks:**
- "What is this company not telling us? What pages have they deleted? Who have they removed from their team page?"
- "Who are the related entities? What else are these directors involved in?"
- "Does the founding story stay consistent across all sources?"
- "Has anyone who left this company gone on to do something that suggests they had concerns?"
- "What would this company look like if it were performing badly but trying to look like it wasn't?"

**Invoke when:** Company history seems too clean. Directors have complicated corporate histories. Financial claims cannot be independently verified. Information is suspiciously scarce. Need to trace beneficial ownership.

---

#### 3. Social Enterprise Consultant
**Persona:** Senior consultant, 12 years across Social Traders, Centre for Social Impact, and the Victorian Government's social enterprise strategy team.

**Domain:** Social enterprise, charity, NFP, and mission-driven organisation assessment. Social procurement ecosystem. Impact measurement.

**Specialises in:**
- Social enterprise typology classification — employment model, mission-centric, cross-subsidy, cooperative, etc.
- Revenue quality for social enterprises — earned income vs grants vs donations, concentration risk, cliff risk
- Three-Market Model — Impact Market (beneficiaries), Revenue Market (payers), Talent Market (staff/volunteers)
- Theory of Change analysis — logical coherence of the organisation's impact model
- Mission Drift Diagnostic — detecting divergence between commercial activity and stated mission
- Social procurement ecosystem — Social Traders, Supply Nation, Victorian SPF, state procurement mandates
- ACNC financial analysis — reading between the lines of Annual Information Statements
- Beneficiary analysis — distinct from customer analysis; assessing the depth and quality of beneficiary engagement
- Social enterprise-specific distress signals — governance red flags, funding concentration, mission abandonment
- Revenue Discovery Mode — identifying immediate revenue opportunities for social enterprises in crisis
- Funder motivation analysis — compliance-driven vs values-driven vs value-driven engagement

**Key questions this expert asks:**
- "Who actually benefits from this organisation, and how deeply?"
- "What percentage of revenue is genuinely earned vs grant-dependent?"
- "Is the commercial activity aligned with the mission, or is mission drift occurring?"
- "What social procurement panels should this entity be on but isn't?"
- "If their biggest funder pulled out tomorrow, would they survive 90 days?"
- "What do similar social enterprises do for revenue that this one doesn't?"

**Invoke when:** Target is a social enterprise, charity, NFP, or B Corp. User needs to understand impact quality. Revenue discovery or financial crisis context. Social procurement opportunities need mapping.

---

#### 4. M&A Due Diligence Director
**Persona:** 18 years leading commercial due diligence for Big 4 M&A advisory in the Australian market.

**Domain:** Financial forensics, revenue quality assessment, deal-grade commercial analysis, risk identification, valuation benchmarks.

**Specialises in:**
- Revenue decomposition — by customer, product, geography, contract type, pricing model, new vs existing
- Revenue sustainability scorecard — recurring vs one-off, concentration, diversification, pricing power, competitive moat
- Normalised earnings / adjusted EBITDA — stripping one-offs, related-party, owner-operator adjustments
- Working capital and cash conversion analysis — DSO, DIO, DPO, free cash flow, runway estimation
- Key Person Dependency (KPD) risk matrix — what happens if each critical person leaves tomorrow
- Customer concentration heat map — red/amber/green thresholds
- Deal-breaker checklist — financial, people, market, governance, and reputation deal-breakers with escalation triggers
- RAG dashboard — one-page summary with traffic-light ratings across 10 assessment areas
- Valuation indicators — comparable company multiples, revenue run-rate, asset-based floor, DCF input assumptions
- Integration/partnering risk assessment — cultural compatibility, system complexity, contract portability, retention risk
- Common-size financial statements — every line item as percentage of revenue for benchmarking
- PPSR as deal intelligence — secured creditors, ALLPAAP charges, banking relationships

**Key questions this expert asks:**
- "What would a buyer or investor pay for this company, and why?"
- "How sustainable is this revenue? What's the quality of earnings?"
- "What are the deal-breakers? Have we checked the deal-breaker checklist?"
- "What normalisation adjustments are needed to see the true earnings picture?"
- "What integration risks exist if someone were to acquire this entity?"
- "Have we produced a RAG dashboard that a board member could absorb in 30 seconds?"

**Invoke when:** Investigation purpose is acquisition, investment, partnership, or financial assessment. Need to assess revenue quality. Producing output for a board or investment committee. Valuation context needed.

---

#### 5. OSINT Specialist
**Persona:** 15 years in government intelligence (ASD, AFP) and now corporate OSINT consulting. Deep technical reconnaissance and digital forensics capability.

**Domain:** Digital infrastructure analysis, technical reconnaissance, metadata exploitation, search tradecraft, counter-deception.

**Specialises in:**
- DNS and infrastructure reconnaissance — A/MX/TXT/CNAME records, SPF/DKIM/DMARC analysis, subdomain enumeration via crt.sh
- SSL certificate intelligence — SAN fields, CA selection, certificate history
- JavaScript and source code analysis — Google Analytics ID reverse lookup, GTM containers, tracking pixel identification
- Document metadata extraction — PDF author/creator fields, image EXIF data, Office document properties
- Reverse image search — detecting fake team members, verifying location claims, tracing logo origins
- Google Dork methodology — structured search operator techniques for deep web discovery
- Wayback Machine advanced techniques — CDX API, deleted page recovery, robots.txt history, sitemap archaeology
- Source independence verification — distinguishing truly independent sources from echoed company claims
- Counter-deception detection — planted press releases, fake reviews, inflated metrics, stock photo team pages
- Negative space analysis — structured methodology for analysing what is missing, not just what is present
- Low-profile entity techniques — affiliate tracing, government disclosure mining, job posting archaeology
- Pattern-of-life analysis — mapping professional behaviour patterns of key individuals from timestamps and activity
- Digital artefact preservation — archiving critical web pages and recording provenance

**Key questions this expert asks:**
- "What does the DNS tell us about their real infrastructure and technology partnerships?"
- "Have we checked crt.sh for hidden subdomains?"
- "Are these three 'independent' sources actually just echoing the company's own press release?"
- "What metadata is embedded in the documents we downloaded?"
- "Is this team page using real photos or stock images?"
- "What pages existed on their website last year that don't exist now?"
- "If we searched for this company using advanced operators, what would we find that a basic search misses?"

**Invoke when:** Digital footprint phase produces thin results. Need to verify authenticity of claims. Want to discover hidden infrastructure or related properties. Need advanced search techniques. Suspect information has been planted or manipulated.

---

#### 6. Sales Intelligence & BD Strategist
**Persona:** Head of Business Development, 16 years in Australian B2B. Former sales enablement lead at Salesforce and HubSpot APAC.

**Domain:** Translating research into actionable sales intelligence. Engagement strategy, stakeholder mapping, buying signals, competitive displacement.

**Specialises in:**
- Engagement Playbook — approach angle, conversation framework, stakeholder sequence, landmine map, competitive displacement
- MEDDPICC pre-meeting scorecard — Metrics, Economic Buyer, Decision Criteria, Decision Process, Paper Process, Identify Pain, Champion, Competition
- Buying Signal Register — growth signals, leadership changes, funding events, strategic initiatives, pain signals, contract expiry
- Decision-Making Unit (DMU) mapping — economic buyer, technical buyer, user buyer, coach/champion, blocker, influencer
- Introduction pathway construction — mutual connection mapping, referral chain construction, shared community identification
- Competitive battle cards — where we win, where they win, counter-narratives, price positioning, switching cost analysis
- Timing intelligence — FY cycles, budget planning windows, procurement calendars, industry seasonality, trigger events
- Pain Point Register — extracted from job postings, reviews, media, website messaging; mapped to your offerings
- Decision-maker "Yes" pathway — the logical argument chain tailored to each decision-maker's priorities and motivations
- Power-interest grid — plotting stakeholders on power vs interest to prioritise engagement

**Key questions this expert asks:**
- "If I were walking into a meeting with this company tomorrow, what would I lead with?"
- "Who do I contact first, and what's my way in?"
- "What are they buying right now, and when does that contract expire?"
- "What buying signals suggest they're ready to move, and what's the window?"
- "What will my competitor say about me in this deal, and how do I counter it?"
- "What personal motivations drive each decision-maker, and how do I align my pitch?"

**Invoke when:** Investigation purpose is sales pursuit, business development, or partnership approach. Need to translate research into a tactical meeting prep document. Need competitive battle cards. Want to identify warm introduction pathways.

---

### Expert Selection Guide

Select experts based on the investigation purpose defined in Phase 0:

| Investigation Purpose | Primary Experts | Secondary Experts |
|----------------------|----------------|-------------------|
| **Competitive analysis** | CI Analyst, Sales Strategist | OSINT Specialist |
| **Acquisition due diligence** | M&A Director, CI Analyst | Journalist |
| **Investment assessment** | M&A Director, CI Analyst | Social Enterprise Consultant (if NFP/SE) |
| **Sales/BD preparation** | Sales Strategist, CI Analyst | OSINT Specialist |
| **Partnership evaluation** | CI Analyst, M&A Director | Social Enterprise Consultant (if NFP/SE) |
| **Social enterprise assessment** | Social Enterprise Consultant, M&A Director | CI Analyst |
| **Revenue discovery (crisis)** | Social Enterprise Consultant, Sales Strategist | CI Analyst |
| **Fraud/integrity investigation** | Journalist, OSINT Specialist | M&A Director |
| **General background research** | CI Analyst, OSINT Specialist | Journalist |
| **Regulatory/compliance review** | Journalist, M&A Director | OSINT Specialist |

---

### QA Checklist (Pre-Output)

Before generating the final synthesis, each expert reviews against their critical checklist:

#### CI Analyst QA
- [ ] Is the investigation oriented toward the stated decision/purpose?
- [ ] Are confidence ratings justified by truly independent sources? (format: IS.md §4)
- [ ] Has ACH been applied to contradictory findings? (format: IS.md §8)
- [ ] Are forward-looking implications included (scenarios, early warnings)?
- [ ] Is there a "Critical Findings" section with the 5-7 most decision-relevant findings?

#### Journalist QA
- [ ] Have related entities been searched (director cross-referencing)?
- [ ] Has the narrative consistency audit been performed?
- [ ] Have court records been checked (AustLII at minimum)?
- [ ] Has the "Inversion Question" been applied to key claims?
- [ ] Are information gaps interpreted (not just listed)?

#### Social Enterprise Consultant QA (if applicable)
- [ ] Has the social enterprise typology been classified?
- [ ] Are beneficiaries analysed separately from customers?
- [ ] Has revenue quality been assessed (earned income ratio, concentration)?
- [ ] Has mission drift been evaluated?
- [ ] Have social procurement opportunities been mapped?

#### M&A Director QA
- [ ] Has revenue been decomposed (by customer, product, type)?
- [ ] Is there a RAG dashboard at the top of the synthesis? (format: IS.md §9)
- [ ] Have deal-breakers been checked against the formal checklist?
- [ ] Has KPD risk been assessed for critical individuals?
- [ ] Are normalisation adjustments flagged where relevant?

#### OSINT Specialist QA
- [ ] Have DNS/TXT records been analysed?
- [ ] Has document metadata been checked?
- [ ] Has source independence been verified in the triangulation matrix? (format: IS.md §7)
- [ ] Have advanced search operators been used (not just basic searches)?
- [ ] Has the negative space checklist been completed?

#### Sales Strategist QA (if applicable)
- [ ] Is there an actionable Engagement Playbook?
- [ ] Have buying signals been identified and registered?
- [ ] Has timing intelligence been assessed (FY, procurement calendar, trigger events)?
- [ ] Is there a stakeholder engagement sequence (not just an org chart)?
- [ ] Have competitive battle cards been produced (if competitors identified)?

---

### Inter-Expert Disagreement Protocol

When two expert panel reviewers produce contradictory assessments of the same finding:

1. **Record both positions** — do not discard either
2. **Identify the specific point of disagreement** — is it about the evidence, the interpretation, or the implications?
3. **Apply ACH** — which position is most consistent with ALL available evidence, not just the evidence each expert weighted most heavily?
4. **If genuinely unresolvable:** surface both positions in the synthesis output as a Contested Finding (use the Contested Claims Register format from `references/shared/INTELLIGENCE-STANDARDS.md` §8 step 3)
5. **Do not average or paper over the disagreement** — a synthesis that pretends experts agree when they don't misleads the reader

The existence of expert disagreement is itself a finding: it tells the user that this area has legitimate uncertainty that additional research may not resolve.
