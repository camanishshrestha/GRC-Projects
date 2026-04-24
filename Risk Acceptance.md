
Project 05: Writing a Risk Acceptance for a Security Gap
Overview

Every organization has risks it chooses to live with. The difference between strong and weak GRC is whether those decisions are intentional, documented, and defensible.

This project focuses on writing a risk acceptance in a Nepali financial/regulatory context, where decisions must align with NRB expectations, audit requirements, and real business constraints.

┌─────────────────────────────────────────────────────────────────────────────┐  
│                    RISK ACCEPTANCE: FAILURE MODES                           │  
├─────────────────────────────────────────────────────────────────────────────┤  
│                                                                             │  
│   COMMON FAILURES                       WHAT SHOULD HAPPEN                  │  
│   ───────────────                       ────────────────────                │  
│                                                                             │  
│   Drowning in technical jargon          Plain business language             │  
│   "Privilege escalation risk"           "Someone could access financial     │  
│                                         data without approval"             │  
│                                                                             │  
│   Hiding behind vague language          Specific, evidenced statements      │  
│   "Risk is low"                         "Similar cases in industry have     │  
│   "Controls are in place"               resulted in NPR 5–20 Crore loss"   │  
│                                                                             │  
│   Selling the decision                  Honest trade-off explanation        │  
│   Downplays impact                      Clearly states downside             │  
│                                                                             │  
│   Treating it as permission slip        Treating it as business decision    │  
│   IT/Audit "approving" risk             Business **owning** the risk        │  
│                                                                             │  
└─────────────────────────────────────────────────────────────────────────────┘  
The Scenario

Company: XYZ Group of Companies
Industry: Banking / Hydropower / Financial Services
Size: Multi-entity group (approx. 300–500 employees)
Compliance: NRB Guidelines, Internal Audit, PCI DSS (where applicable), ISO 27001

The Security Gap

The internal audit team has identified that the company’s legacy financial reconciliation system:

Uses a shared service account with database admin privileges
The password has not been rotated in 18 months
12–15 team members know the password
The system processes approximately NPR 200–300 billion annually
Replacing the system would require 12–18 months of development
Technical Details
System: Legacy Reconciliation System (internal application)
Database: Oracle (financial + sensitive data)
Authentication: Static username/password stored in config file
Logging: Application logs exist, but no database query logging
Network: Operates within sensitive financial environment
Business Context
Finance team relies on this system for daily operations
Critical for financial reporting and audit
No budget allocated for replacement in current fiscal year
Previous modernization attempt failed
System owner is the CFO
What You Need to Do
Phase 1: Understand the Risk (Not Just the Vulnerability)

Task: Translate the technical finding into business risk.

┌─────────────────────────────────────────────────────────────────────────────┐  
│                    VULNERABILITY VS RISK                                    │  
├─────────────────────────────────────────────────────────────────────────────┤  
│                                                                             │  
│   VULNERABILITY (Technical)             RISK (Business)                     │  
│   ─────────────────────────             ───────────────                     │  
│                                                                             │  
│   Shared service account                No accountability over financial    │  
│                                         data access                         │  
│                                                                             │  
│   No password rotation                  If compromised, long-term access    │  
│                                         possible                            │  
│                                                                             │  
│   12–15 people know password            Higher insider/phishing risk        │  
│                                                                             │  
│   No query logging                      Breach may go undetected            │  
│                                                                             │  
│   DB admin privileges                   Financial records can be altered    │  
│                                         or deleted                          │  
│                                                                             │  
└─────────────────────────────────────────────────────────────────────────────┘  
Phase 2: Assess Likelihood and Impact

Task: Provide specific, evidence-based assessment.

BAD Assessment (Vague)
Likelihood: Low  
Impact: High  
Risk Rating: Medium  
GOOD Assessment (Specific)
┌─────────────────────────────────────────────────────────────────────────────┐  
│                         LIKELIHOOD ASSESSMENT                               │  
├─────────────────────────────────────────────────────────────────────────────┤  
│                                                                             │  
│   FACTOR                                ASSESSMENT                          │  
│   ──────                                ──────────                          │  
│                                                                             │  
│   Threat Actor Interest                 **HIGH**                            │  
│                                                                             │  
│   Attack Complexity                     **LOW**                             │  
│                                                                             │  
│   Detection Capability                  **LOW**                             │  
│                                                                             │  
│   Historical Incidents                  **UNKNOWN**                         │  
│                                                                             │  
│   OVERALL LIKELIHOOD: **MODERATE-HIGH**                                     │  
│                                                                             │  
└─────────────────────────────────────────────────────────────────────────────┘  

┌─────────────────────────────────────────────────────────────────────────────┐  
│                         IMPACT ASSESSMENT                                   │  
├─────────────────────────────────────────────────────────────────────────────┤  
│                                                                             │  
│   IMPACT CATEGORY           POTENTIAL CONSEQUENCE            ESTIMATED COST │  
│                                                                             │  
│   Regulatory Action         NRB penalties                  **NPR 5–20 Cr**   │  
│                                                                             │  
│   Financial Fraud           Data manipulation              **NPR 1–10 Cr**   │  
│                                                                             │  
│   Reputation                Customer trust loss            **HIGH**          │  
│                                                                             │  
│   Business Disruption       System downtime                **10–20L/day**    │  
│                                                                             │  
│   TOTAL POTENTIAL IMPACT: **NPR 10–25 Crore (most likely)**                 │  
│                                                                             │  
└─────────────────────────────────────────────────────────────────────────────┘  
Phase 3: Evaluate Compensating Controls
┌─────────────────────────────────────────────────────────────────────────────┐  
│                    COMPENSATING CONTROLS ASSESSMENT                         │  
├─────────────────────────────────────────────────────────────────────────────┤  
│                                                                             │  
│   CLAIMED CONTROL           EFFECTIVENESS              HONEST ASSESSMENT    │  
│                                                                             │  
│   Network security          **PARTIAL**               Does not prevent       │  
│                                                      insider misuse         │  
│                                                                             │  
│   Application logging       **WEAK**                  No DB-level logs       │  
│                                                                             │  
│   Staff trust               **MINIMAL**               No misuse control      │  
│                                                                             │  
│   Encryption                **IRRELEVANT**            Doesn’t prevent        │  
│                                                      authorized misuse      │  
│                                                                             │  
│   OVERALL: **LIMITED PROTECTION**                                           │  
│                                                                             │  
└─────────────────────────────────────────────────────────────────────────────┘  
Phase 4: Present Options to Business
┌─────────────────────────────────────────────────────────────────────────────┐  
│                         DECISION OPTIONS                                    │  
├─────────────────────────────────────────────────────────────────────────────┤  
│                                                                             │  
│   OPTION A: **FULL REMEDIATION**                                            │  
│   Cost: **NPR 10–15 Crore** | Timeline: **12–18 months** | Risk: LOW        │  
│                                                                             │  
│   OPTION B: **PARTIAL REMEDIATION**                                         │  
│   Cost: **NPR 20–30 lakh** | Timeline: **3 months** | Risk: MEDIUM          │  
│                                                                             │  
│   OPTION C: **ACCEPT RISK**                                                 │  
│   Cost: **0** | Potential Loss: **NPR 10–25 Crore** | Risk: HIGH            │  
│                                                                             │  
│   OPTION D: **ACCEPT WITH MINIMAL IMPROVEMENTS**                            │  
│   Cost: **Minimal** | Timeline: **2 weeks** | Risk: HIGH (reduced)          │  
│                                                                             │  
└─────────────────────────────────────────────────────────────────────────────┘  
Phase 5: Write the Risk Acceptance Document

Create a formal document including:

Executive Summary
Risk Description (plain language)
Likelihood & Impact (with numbers)
Options considered
Selected option with rationale
Residual risks
Conditions & review timeline
Sign-offs (CFO, Compliance, Security)
Success Criteria
Criteria	Evidence
Plain language	CFO can understand
Specific	Uses NPR, timelines
Honest controls	No exaggeration
Options presented	At least 3
Residual risk clear	Explicit risks
Conditions stated	Review defined
Deliverables Checklist
□ Risk description in plain language  
□ Likelihood assessment with evidence  
□ Impact assessment in NPR  
□ Honest control evaluation  
□ Options analysis (minimum 3 options)  
□ Formal risk acceptance document  
□ Conditions and review schedule  
