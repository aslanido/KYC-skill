# KYC Analysis Skill

A lightweight, investigator-style KYC due diligence tool that produces risk-rated reports and structured JSON output.

## Overview

This skill transforms Claude into a senior compliance analyst who thinks like an investigator, not a form-filler. It's designed for quick background checks, entity screening, and corporate research.

**Key Principle:** Lead with the conclusion. A good KYC report lets a compliance officer make a decision in 5 minutes.

## When to Use This Skill

Use **kyc-analysis** when you need:

- Quick background check on a company or individual
- Entity screening before a business relationship
- "Who is [entity]?" research questions
- Risk-rated due diligence report
- Structured data output for systems integration

## Features

### Investigator Mindset

The skill applies professional analyst thinking:

| Principle | Description |
|-----------|-------------|
| **Start with the money** | Understand the business model first. It shapes what risks matter. |
| **Follow the people** | Entities don't commit crimes, people do. Find the UBOs. |
| **Triangulate everything** | One source = claim. Two = pattern. Three = evidence. |
| **Red flags cluster** | PEP + adverse media + high-risk jurisdiction = escalate. |
| **Absence is information** | Can't find financials for a 20-year company? That's a red flag. |

### Risk Rating System

| Rating | Meaning | Action |
|--------|---------|--------|
| **LOW** | Normal business, clean principals, transparent | Standard onboarding |
| **MEDIUM** | Something to watch. PEP, minor litigation, complex structure | Enhanced monitoring |
| **HIGH** | Multiple concerns or single serious flag | Senior review required |
| **CRITICAL** | Sanctions, criminal investigation, fraud indicators | Decline relationship |

### Investigation Flow

1. **Identify** → Lock down the legal entity (name, jurisdiction, registration)
2. **Map ownership** → Trace to natural persons (UBOs with 25%+ ownership)
3. **Profile principals** → Background check key people (PEP, criminal, bankruptcies)
4. **Understand business** → What do they do? Does it make sense?
5. **Screen for red flags** → Sanctions, adverse media, regulatory actions, litigation
6. **Synthesize** → What's the story? Does everything fit?

## Output Files

The skill generates two files per analysis:

### Narrative Report (`kyc_[entity]_report.md`)

Markdown report containing:
- **Executive summary** (1 paragraph with risk rating and key concerns)
- Entity details and ownership structure
- Key personnel profiles
- Risk assessment by category
- Red flags section (if any)
- Information gaps
- Sources

### Structured Data (`kyc_[entity]_data.json`)

JSON file for systems integration containing:
- Entity metadata
- Ownership structure
- Directors and UBOs
- Screening results (sanctions, PEP, adverse media)
- Red flags array
- Risk scores and ratings
- Recommendation

## Usage

Simply ask Claude to analyze an entity:

```
Run KYC analysis on [Company Name]
```

Or reference the skill directly:

```
Use @kyc-analysis/SKILL.md to check [Entity Name]
```

## Example Output

### Good Executive Summary

```
🔴 HIGH RISK. Founder has criminal conviction (tax fraud, 2005). Multiple PEP 
connections through family ties to current administration. Pattern of regulatory 
violations ($3M+ in fines). Recommend decline unless compelling business rationale.
```

### Bad Executive Summary

```
The company was founded in 1985 and operates in the real estate sector. They have 
offices in multiple locations and employ approximately 500 people...
```

The first tells you what you need to know. The second buries the lead.

## Sample Reports

| Entity | Risk | Key Findings |
|--------|------|--------------|
| Norway Ogarsen International Biotechnology | 🔴 HIGH (75) | Shell company indicators, serial dormant incorporation, misleading name |
| Aria Associates Limited | 🟢 LOW (28) | Clean 11-year UK consultancy, micro entity, no adverse findings |

## Files in This Folder

```
kyc-analysis/
├── SKILL.md                           # Skill definition
├── README.md                          # This file
├── schema.json                        # JSON output schema (optional)
├── kyc_[entity]_report.md            # Generated reports
└── kyc_[entity]_data.json            # Generated structured data
```

## Common Pitfalls to Avoid

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Over-documenting clean entities | Wastes reviewer time | If clean, say so briefly |
| Burying the lead | Risk rating hidden at end | Lead with conclusion |
| Missing the forest | 50 minor violations, 1 major fraud | Focus on what matters |
| Speculation without flagging | Stating inference as fact | Use "this suggests..." |

## Comparison to KYC Officer Skill

| Aspect | kyc-analysis | kyc-officer |
|--------|--------------|-------------|
| **Purpose** | Quick research/screening | Full customer onboarding |
| **Complexity** | Lightweight, single pass | 23-state workflow engine |
| **Output** | Report + JSON | Full audit trail + decision |
| **Interaction** | Minimal (provide entity name) | Interactive (customer dialogue) |
| **Use Case** | Background checks, research | Regulatory-compliant onboarding |

Use **kyc-analysis** for quick checks. Use **kyc-officer** for formal onboarding workflows.

## Limitations

- Based on publicly available information only
- Not a substitute for actual compliance database screening
- Educational/simulation purposes
- Requires human review for actual decisions

---

**Version:** 1.0.0  
**Last Updated:** January 2026
