# KYC Skills for Claude

This repository contains **two complementary KYC (Know Your Customer) skills** for Claude, designed for different use cases:

| Skill | Purpose | Complexity | Time |
|-------|---------|------------|------|
| **[kyc-analysis](#kyc-analysis-skill)** | Quick research & screening | Lightweight | 2-5 min |
| **[kyc-officer](#kyc-officer-skill)** | Full regulatory onboarding workflow | 23-state engine | 15-30 min |

---

## KYC Analysis Skill

**Location:** `/kyc-analysis/SKILL.md`

A lightweight, investigator-style due diligence tool for quick background checks and entity screening.

### When to Use

- ✅ Quick "who is this company?" research
- ✅ Pre-meeting background checks
- ✅ Initial screening before formal onboarding
- ✅ Due diligence on counterparties/vendors
- ✅ Batch entity screening
- ✅ Structured data export (JSON)

### Output

Generates two files per analysis:
- `kyc_[entity]_report.md` — Narrative report with executive summary, risk rating, red flags
- `kyc_[entity]_data.json` — Structured data for systems integration

### Quick Start

```
Run KYC analysis on [Company Name]
```

See [`kyc-analysis/README.md`](kyc-analysis/README.md) for full documentation.

---

## KYC Officer Skill

**Location:** `/.claude/skills/kyc-officer/skill.md`

A comprehensive skill that transforms Claude into a professional KYC Officer at a Tier 1 international bank. Implements a deterministic, workflow-driven approach to customer onboarding following global AML/CFT regulations.

### When to Use

- ✅ Formal customer onboarding workflows
- ✅ Regulatory-compliant KYC procedures
- ✅ Interactive customer interviews
- ✅ Enhanced Due Diligence (EDD) scenarios
- ✅ MLRO escalation and decision documentation
- ✅ Training compliance officers
- ✅ Full audit trail requirements

### Features

- **23-state workflow engine** with deterministic execution following a state machine pattern
- **Comprehensive compliance coverage** (BSA, PATRIOT Act, FinCEN CDD, FATF Recommendations, Wolfsberg Principles)
- **Multi-dimensional risk scoring** across screening, jurisdictional, business, and customer profile dimensions
- **Four-tier risk rating system**: LOW, MEDIUM, HIGH, PROHIBITED
- **Parallel screening execution** (Sanctions, PEP, Adverse Media, Enforcement)
- **Enhanced Due Diligence (EDD) procedures** for elevated-risk customers
- **Complete audit trail** with state transitions and decision documentation

### Workflow-Driven Architecture

The skill uses a Markdown-based workflow definition (`workflow.md`) that acts as a state machine. Each state defines:
- **Type**: Task, Choice, Parallel, HumanIntervention, Wait, Success, Failure
- **Instructions**: What action to perform
- **Conditions**: When to transition to next state
- **Output**: Variables to store for later use

This deterministic approach ensures:
- Consistent application of KYC procedures
- Complete regulatory compliance
- Reproducible decision-making
- Full audit trail for regulators

### Risk Assessment

**Risk Scoring Matrix**:
| Factor | Weight | Coverage |
|--------|--------|----------|
| Screening | 40% | Sanctions, PEP, Adverse Media, Enforcement |
| Jurisdictional | 25% | FATF status, Corruption index, AML regime |
| Business Activity | 25% | Industry sector, Complexity, Transparency |
| Customer Profile | 10% | Documentation quality, Consistency |

**Risk Tiers**:
- **LOW (0-30)**: Auto-approve, standard monitoring, annual review
- **MEDIUM (31-60)**: Enhanced due diligence, semi-annual review
- **HIGH (61-85)**: Extensive EDD, MLRO approval, quarterly review
- **PROHIBITED (86-100)**: Auto-reject, file SAR if required

### Screening Capabilities

- **Sanctions Lists**: OFAC SDN, UN Security Council, EU Consolidated, UK HMT
- **PEP Screening**: Current/former government officials, family members, close associates
- **Adverse Media**: 7-year lookback for financial crime indicators
- **Regulatory Enforcement**: SEC, FinCEN, FCA actions and sanctions

### Customer Types Supported

- Individual retail customers
- Corporate entities (private/public)
- Trusts and partnerships
- Complex multi-jurisdictional structures
- High-net-worth individuals
- Politically Exposed Persons (PEPs)

## Quick Start

### Using the Skill

1. **Activate the skill** in your Claude conversation:
   ```
   I'd like to conduct KYC onboarding for a new customer
   ```

2. **Provide customer information** as prompted by the workflow

3. **Make decisions** at Enhanced Due Diligence checkpoints

4. **Receive final risk assessment** and onboarding decision

### Example Interaction

```
User: I need to onboard a corporate customer from the UK

KYC Officer:
Good afternoon, and thank you for bringing this onboarding request.
I'll guide you through our corporate KYC process.

Current State: InitialInformationGathering

Please provide:
1. Full legal name of the entity
2. Country of incorporation
3. Nature of business
4. Expected transaction volumes
5. Purpose of account opening
```

## Repository Structure

```
KYC-skill/
├── README.md                          # This file
└── .claude/
    └── skills/
        └── kyc-officer/
            ├── skill.json             # Skill metadata and configuration
            ├── skill.md               # Main skill prompt (execution engine)
            ├── workflow.md            # Workflow state machine definition
            ├── README.md              # Detailed skill documentation
            └── SAMPLE_SCENARIOS.md    # Test scenarios for workflow testing
```

## Files Description

### `workflow.md` - Workflow State Machine
Defines the complete KYC workflow with 23 states:
- **Customer intake** (InitialInformationGathering, CustomerTypeClassification)
- **Data collection** (IndividualKYC, EntityUnwrapping)
- **Screening** (ComprehensiveScreening with parallel branches)
- **Risk assessment** (JurisdictionalAnalysis, BusinessActivityAnalysis, RiskAssessmentCalculation)
- **Decision routing** (RiskDecisionLogic, SanctionsBlockCheck)
- **Enhanced procedures** (EnhancedDueDiligence, RequestAdditionalEDD)
- **Terminal states** (AutoApprove, ApproveWithMonitoring, AutoReject, RejectWithReason)

### `skill.md` - Skill Execution Engine
Contains the comprehensive instructions for Claude to:
- Interpret and execute the workflow
- Maintain state context and variables
- Apply risk-based assessment methodology
- Communicate professionally with customers
- Document decisions with audit trail
- Handle Enhanced Due Diligence scenarios

### `skill.json` - Configuration
Metadata including:
- Skill name, version, description
- Compliance frameworks covered
- Capabilities and use cases
- Tags for discoverability

### `SAMPLE_SCENARIOS.md` - Test Cases
Five realistic customer scenarios:
1. **Low-Risk Individual** (Sarah Johnson) - Expected: Auto-Approve
2. **Medium-Risk Corporate** (Global Trade Partners) - Expected: Approve w/ Enhanced Monitoring
3. **High-Risk Entity** (Caribbean Holdings) - Expected: Extensive EDD, possibly reject
4. **Prohibited Customer** (Syrian Entity) - Expected: Auto-Reject (sanctions)
5. **Individual PEP** (Dr. Okoro) - Expected: Extensive EDD, conditional approval

## Compliance Framework

The skill implements procedures aligned with:

- **Bank Secrecy Act (BSA)** - US anti-money laundering foundation
- **USA PATRIOT Act Section 326** - Customer Identification Program requirements
- **FinCEN CDD Rule** (31 CFR 1010.230) - Beneficial ownership requirements
- **FATF Recommendations** - Especially R.10 (CDD), R.22 (DNFBPs), R.24 (Transparency)
- **Wolfsberg Group Principles** - International AML standards for private banking

## Use Cases

### Training & Education
- Train new compliance officers on KYC procedures
- Practice Enhanced Due Diligence scenarios
- Learn risk-based assessment methodology
- Understand workflow state transitions

### Process Design
- Model KYC workflows for financial institutions
- Design state machines for compliance processes
- Document decision trees and escalation paths
- Create audit-ready procedure documentation

### Simulation & Testing
- Test customer scenarios against risk appetite
- Simulate high-risk onboarding decisions
- Practice PEP and sanctions screening
- Role-play compliance officer interactions

### Documentation
- Generate templates for KYC policies
- Create audit trail examples
- Document risk assessment methodologies
- Produce training materials

## Key Capabilities

✅ **Deterministic Workflow** - Follows exact state machine, no shortcuts
✅ **Risk-Based Approach** - Different procedures based on risk level (FATF compliant)
✅ **Parallel Screening** - Simultaneous checks across multiple databases
✅ **Enhanced Due Diligence** - Automatic triggering for PEPs, high-risk jurisdictions, adverse media
✅ **Human Intervention Points** - Pauses workflow for compliance officer decisions
✅ **Complete Audit Trail** - Documents every state transition and decision
✅ **Professional Communication** - Customer-facing and internal documentation
✅ **Regulatory Alignment** - Follows BSA, PATRIOT Act, FinCEN, FATF guidelines

## Limitations & Disclaimers

⚠️ **Educational Tool**: This skill is for training and simulation purposes only

⚠️ **Not Production-Ready**: Real KYC requires:
- Integration with actual screening databases (World-Check, Dow Jones, LexisNexis, etc.)
- Human compliance officer review
- Legal counsel consultation
- Core banking system integration
- Document management systems
- Case management platforms

⚠️ **No Legal Advice**: This skill does not constitute legal or compliance advice. Consult qualified professionals for actual customer onboarding.

⚠️ **Jurisdiction-Specific**: Adapt the workflow to your specific regulatory jurisdiction and institutional policies.

## Customization

The workflow can be customized by editing `workflow.md`:

- **Add states**: Define new screening checks or analysis steps
- **Modify risk thresholds**: Adjust scoring weights and rating boundaries
- **Change screening lists**: Update high-risk jurisdictions or sectors
- **Add document requirements**: Specify institution-specific documentation
- **Alter decision logic**: Change when EDD is triggered or auto-reject occurs

## Example Modifications

### Add Cryptocurrency Screening
```markdown
### 5e. CryptocurrencyCheck
- **name**: CryptocurrencyCheck
- **action**: SEARCH
- **instruction**: "Check if customer has cryptocurrency exchange accounts or blockchain exposure."
- **output_variable**: "crypto_results"
```

### Adjust Risk Thresholds
```markdown
Based on total score (0-100):
- **LOW (0-35)**: Standard onboarding     # Increased from 30
- **MEDIUM (36-70)**: Enhanced due diligence  # Increased from 60
```

## Contributing

This is an experimental skill for Claude Code. Enhancements welcome:

- Additional workflow states for specific compliance scenarios
- Integration patterns with real screening APIs
- Enhanced risk scoring algorithms
- Jurisdiction-specific workflow variants
- Additional sample scenarios

## Resources

- **FATF Recommendations**: https://www.fatf-gafi.org/recommendations.html
- **FinCEN CDD Rule**: https://www.fincen.gov/resources/statutes-regulations/administrative-rulings/customer-due-diligence-requirements
- **Wolfsberg Group**: https://www.wolfsberg-principles.com/
- **OFAC Sanctions Lists**: https://sanctionssearch.ofac.treas.gov/

## Skills Comparison

### Output Comparison

| Output | kyc-analysis | kyc-officer |
|--------|--------------|-------------|
| Executive summary | ✅ 1 paragraph | ✅ Full section |
| Risk score | ✅ 0-100 | ✅ 0-100 |
| Risk rating | ✅ LOW/MEDIUM/HIGH/CRITICAL | ✅ LOW/MEDIUM/HIGH/PROHIBITED |
| Screening results | ✅ Summary | ✅ Detailed with audit trail |
| Red flags | ✅ Listed | ✅ Analyzed with severity |
| Recommendation | ✅ Brief | ✅ Full with conditions |
| JSON export | ✅ Yes | ❌ No |
| Workflow audit trail | ❌ No | ✅ Full state transitions |
| Customer communication | ❌ No | ✅ Letters and dialogue |
| EDD procedures | ❌ No | ✅ Full EDD workflow |
| MLRO escalation | ❌ No | ✅ Built-in |

### Workflow Diagram

```
                    ┌─────────────────────┐
                    │   Entity to Check   │
                    └──────────┬──────────┘
                               │
              ┌────────────────┴────────────────┐
              │                                 │
              ▼                                 ▼
    ┌─────────────────┐              ┌─────────────────────┐
    │  kyc-analysis   │              │    kyc-officer      │
    │  (Quick Check)  │              │  (Full Onboarding)  │
    └────────┬────────┘              └──────────┬──────────┘
             │                                  │
             ▼                                  ▼
    ┌─────────────────┐              ┌─────────────────────┐
    │ • Web research  │              │ • 23-state workflow │
    │ • Risk scoring  │              │ • Customer dialogue │
    │ • Report + JSON │              │ • EDD procedures    │
    └────────┬────────┘              │ • MLRO escalation   │
             │                       │ • Full audit trail  │
             ▼                       └──────────┬──────────┘
    ┌─────────────────┐                         │
    │ 2 output files: │                         ▼
    │ • report.md     │              ┌─────────────────────┐
    │ • data.json     │              │ Decision Report:    │
    └─────────────────┘              │ • APPROVE           │
                                     │ • APPROVE + EDD     │
                                     │ • REJECT            │
                                     └─────────────────────┘
```

### Example Use Case Flow

1. **Initial screening** → Use `kyc-analysis` for quick background check
2. **Decision to proceed** → If clean, initiate formal onboarding
3. **Formal onboarding** → Use `kyc-officer` for full workflow
4. **Periodic review** → Use `kyc-analysis` for refresh checks

## License

This skill is provided for educational and training purposes. Ensure compliance with all applicable laws and regulations in your jurisdiction.

---

**Version**: 1.0.0
**Author**: KYC Compliance Team
**Last Updated**: January 2026
