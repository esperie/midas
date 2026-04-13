# Regulatory Requirements: Investment Management Platforms

## Executive Summary

Managing other people's money is one of the most heavily regulated activities in every jurisdiction. For Midas to move from personal use to productization, the regulatory path is the single most consequential design constraint. In the US, Midas must register as a Registered Investment Adviser (RIA) with the SEC and comply with fiduciary duty, Form ADV disclosure, custody rules, and algorithmic trading oversight. In Singapore, a Capital Markets Services (CMS) License from MAS is required. The EU requires MiFID II compliance. The autonomous nature of Midas adds an additional layer of regulatory complexity -- regulators are increasingly scrutinizing AI-driven investment decisions.

---

## 1. United States (SEC / FINRA)

### 1.1 Registration Requirements

#### Registered Investment Adviser (RIA) -- Primary Path

The Investment Advisers Act of 1940 requires anyone who provides investment advice for compensation to register as an investment adviser. This is Midas's primary regulatory pathway.

**Registration thresholds**:

- **$0-$25M AUM**: Register with the state(s) where you operate
- **$25M-$100M AUM**: Register with state(s) -- may have option to register with SEC if in a state with no adviser registration or if operating in 15+ states
- **$100M+ AUM**: Must register with the SEC

**For Midas starting at $5M personal + scaling to multi-client**: Start with state registration; transition to SEC registration at $100M AUM.

**What registration requires**:

1. **Form ADV Part 1**: Business information, disciplinary history, ownership, conflicts of interest; filed electronically via IARD (Investment Adviser Registration Depository)
2. **Form ADV Part 2 (Brochure)**: Plain-English disclosure to clients about services, fees, investment strategies, risks, disciplinary information; must be delivered to clients before or at engagement
3. **Form ADV Part 2B (Supplement)**: Background of supervised persons who provide investment advice
4. **Form ADV Part 3 (Form CRS)**: Client Relationship Summary -- concise document for retail investors
5. **Compliance program**: Written policies and procedures (Rule 206(4)-7); designated Chief Compliance Officer (CCO)
6. **Code of ethics**: Personal trading rules for supervised persons
7. **Annual updating amendment**: File updated Form ADV within 90 days of fiscal year end

**Costs**:

- SEC registration: $0 filing fee (but significant compliance costs)
- State registration: $200-$500 per state
- IARD filing: $100-$300/year
- Compliance infrastructure: $50K-$200K/year for a small RIA (outsourced CCO, compliance software, audit)
- Errors & Omissions (E&O) insurance: $5K-$20K/year
- Fidelity bond: Required by SEC; covers employee theft

#### Broker-Dealer Registration -- NOT Recommended

If Midas were executing trades on behalf of clients, it might need broker-dealer registration (FINRA). However, by using a third-party broker-dealer (Alpaca, Interactive Brokers, Schwab) for execution and custody, Midas avoids this requirement. The RIA places trades through the broker-dealer under a trading authorization.

**Key distinction**:

- **RIA**: Provides investment advice for a fee; has discretionary authority over accounts; fiduciary duty
- **Broker-Dealer**: Executes securities transactions; holds customer funds/securities; suitability standard (or Reg BI)
- **Midas's model**: RIA with discretionary authority, executing through a BD partner

### 1.2 Fiduciary Duty

As a registered investment adviser, Midas owes a fiduciary duty to clients. This is the highest standard of care in financial regulation.

**Components**:

1. **Duty of Care**: Obligation to provide advice in the client's best interest; must have a reasonable basis for recommendations; must consider the client's objectives, risk tolerance, and financial circumstances
2. **Duty of Loyalty**: Must not place own interests ahead of clients'; must disclose all material conflicts of interest; must seek best execution for client trades
3. **Duty to Act in Good Faith**: Must act honestly and in the client's best interests

**Implications for Midas's autonomous system**:

- The system must have documented investment policies and procedures
- Every automated decision must be traceable and auditable
- Risk parameters must be set appropriately for each client's profile
- The system cannot take actions that benefit Midas at the expense of clients (e.g., churning for fees)
- Conflicts must be disclosed -- e.g., if Midas invests its own $5M in the same strategies as clients

### 1.3 Custody Rule (Rule 206(4)-2)

The custody rule is one of the most important for Midas's architecture.

**Core requirement**: If an RIA has custody of client funds or securities, it must:

1. Use a **qualified custodian** (bank, broker-dealer, futures commission merchant)
2. Ensure the custodian sends **quarterly account statements** directly to clients
3. Undergo an **annual surprise examination** by an independent public accountant

**How Midas avoids direct custody issues**:

- Client assets are held at a qualified custodian (Alpaca, IBKR, Schwab)
- Midas has **trading authority** (discretion to trade) but not physical custody
- Clients receive statements directly from the custodian
- Note: The SEC considers the ability to debit advisory fees directly from client accounts as having "custody" -- this triggers the surprise exam requirement unless using a qualified custodian that sends direct statements

**Architectural implication**: Midas MUST NOT hold client funds. All money flows through qualified custodians. Midas only has API-level trading authority.

### 1.4 Regulation Best Interest (Reg BI)

Reg BI primarily applies to broker-dealers, not RIAs. However, understanding it is important because:

- If Midas uses a BD partner (like Alpaca), the BD has Reg BI obligations
- The SEC may apply similar principles to RIA robo-advisors
- Reg BI requires: disclosure, care, conflict of interest, and compliance obligations

### 1.5 Algorithmic Trading and AI Oversight

**Current regulatory landscape (as of 2026)**:

- The SEC does not have specific regulations for AI-driven investment advisers (yet)
- However, the SEC's 2023-2024 proposed rules on "Predictive Data Analytics" (PDA) would require RIAs to evaluate and mitigate conflicts of interest associated with AI/ML models used in investor interactions
- The SEC has issued risk alerts about robo-advisors focusing on: adequacy of disclosures, suitability of algorithm-driven advice, effectiveness of compliance programs
- FINRA has published guidance on algorithmic trading oversight

**Key SEC robo-adviser guidance** (2017 IM Guidance Update, updated 2021):

1. Adequate disclosure of the algorithm's methodology, risks, and limitations
2. Sufficient information gathering from clients (suitability questionnaire)
3. Effective compliance program tailored to the automated advice model
4. Disclosure that human oversight may be limited or absent

**Implications for Midas**:

- Must clearly disclose that investment decisions are made autonomously by AI
- Must disclose the types of strategies employed and their risks
- Must have human oversight mechanisms (kill switches, drawdown limits, compliance alerts)
- Must document and test the AI system's decision-making process
- Must have procedures for handling algorithm failures or market disruptions
- Should anticipate future regulations specifically targeting AI advisers

### 1.6 Anti-Money Laundering (AML) and Know Your Customer (KYC)

While AML/KYC is primarily a broker-dealer obligation, RIAs have:

- **FinCEN Customer Identification Program (CIP)**: Required if the RIA holds custody or has certain banking relationships
- **OFAC screening**: Must not transact with sanctioned persons/entities
- **Suspicious Activity Reports (SARs)**: Must file if suspicious activity is detected

**For Midas**: The custodian/BD partner (Alpaca, IBKR) handles most KYC/AML. Midas should still maintain its own client identification and screening procedures.

### 1.7 Key US Regulations Summary

| Regulation                   | Applies To             | Key Requirement                    | Midas Impact                        |
| ---------------------------- | ---------------------- | ---------------------------------- | ----------------------------------- |
| Investment Advisers Act 1940 | RIAs                   | Registration, fiduciary duty       | Must register as RIA                |
| Rule 206(4)-2 (Custody)      | RIAs with custody      | Qualified custodian, surprise exam | Use third-party custodian           |
| Rule 206(4)-7 (Compliance)   | All RIAs               | Written compliance program, CCO    | Must have compliance infrastructure |
| Form ADV                     | All RIAs               | Disclosure to clients              | Must file and deliver               |
| Reg BI                       | Broker-Dealers         | Best interest standard             | Applies to BD partners              |
| SEC PDA Proposed Rules       | RIAs using AI          | AI conflict mitigation             | Anticipate and design for           |
| FINRA Rules                  | BDs, algo trading      | Supervisory obligations            | Applies to BD partners              |
| Bank Secrecy Act / AML       | Financial institutions | KYC, suspicious activity reporting | Partially applies                   |

---

## 2. Singapore (MAS)

### 2.1 Capital Markets Services License

The Securities and Futures Act 2001 (SFA) requires anyone who carries on a business of providing financial advisory services or dealing in capital markets products to hold the appropriate license.

**License type for Midas**: Capital Markets Services (CMS) License for Fund Management or Investment Advisory

**Categories**:

- **Licensed Fund Manager (LFM)**: For managing collective investment schemes or discretionary mandates; requires $250K base capital
- **Registered Fund Management Company (RFMC)**: Simplified regime for managers with fewer than 30 qualified investors and AUM below S$250M; requires $250K base capital but lighter regulatory requirements
- **Accredited/Institutional Fund Manager (A/I FM)**: For managers serving only accredited or institutional investors; base capital S$250K; lighter reporting

**For Midas in Singapore**:

- If managing money for Singapore-based clients, a CMS license is required
- The RFMC route is attractive for early-stage productization (fewer than 30 qualified investors)
- If Midas only manages its own $5M, no license is required (personal investment)

### 2.2 MAS Requirements

**Application requirements**:

1. Fit and proper criteria for directors and key officers
2. Minimum base capital (S$250K for RFMC; S$1M for full LFM)
3. Professional indemnity insurance
4. Risk management framework
5. Business plan and internal controls documentation
6. Compliance arrangements (appointment of compliance officer)

**Ongoing obligations**:

- Annual audit
- Regular regulatory filings
- Customer due diligence / AML-CFT compliance
- Risk-based capital adequacy
- Record keeping (5 years minimum)
- Business conduct requirements (fair dealing, best execution)

### 2.3 MAS Technology Risk Management

MAS has specific requirements for technology-driven financial services:

- **Technology Risk Management (TRM) Guidelines**: Apply to all financial institutions; cover cybersecurity, data protection, IT governance
- **Notice on Outsourcing**: If Midas uses cloud infrastructure or third-party APIs, outsourcing requirements apply
- **FEAT Principles (Fairness, Ethics, Accountability, Transparency)**: MAS's AI governance framework for financial institutions. Not legally binding but expected as best practice.
  - Fairness: AI should not discriminate
  - Ethics: Aligned with institutional values
  - Accountability: Clear responsibility for AI decisions
  - Transparency: Explainability of AI-driven decisions

### 2.4 Singapore FinTech Regulatory Sandbox

MAS operates a FinTech Regulatory Sandbox that allows firms to test innovative financial services within a well-defined space and duration:

- Relaxation of certain requirements for sandbox period
- Must demonstrate genuine innovation and benefit to consumers
- Time-limited (typically 6-12 months, extendable)
- Must have exit strategy (full license or graceful shutdown)

**Potential for Midas**: Could apply for sandbox to test autonomous investment management before full CMS license application.

---

## 3. European Union (MiFID II)

### 3.1 Markets in Financial Instruments Directive II

MiFID II is the primary regulatory framework for investment services in the EU/EEA (27 EU member states + Norway, Iceland, Liechtenstein).

**Relevant activities**:

- **Portfolio management**: Managing individual portfolios on a discretionary basis -- this is Midas's core activity
- **Investment advice**: Providing personalized recommendations

**Authorization**:

- Must be authorized by the national competent authority (NCA) of an EU member state
- Passport: Once authorized in one member state, can provide services across the EU
- Capital requirements vary by activity type (EUR 75K-750K depending on services)

### 3.2 Key MiFID II Requirements

**Client categorization**:

- Retail clients: Highest level of protection
- Professional clients: Reduced obligations
- Eligible counterparties: Lightest obligations

**Suitability and appropriateness**:

- Must assess client's knowledge, experience, financial situation, investment objectives, and risk tolerance
- For discretionary management: full suitability assessment required
- Must provide periodic suitability reports

**Best execution**:

- Must take all sufficient steps to obtain the best possible result for clients
- Must have an execution policy and disclose it to clients
- Must monitor execution quality

**Algorithmic trading specific requirements (MiFID II Article 17)**:

- Must have effective systems and risk controls
- Must notify the NCA that the firm engages in algorithmic trading
- Must be able to provide a description of the algorithm's strategies, trading parameters, and risk controls
- Must maintain records of algorithmic trading systems
- Must have kill switch functionality
- Must test algorithms before deployment and have change management procedures

**Product governance**:

- Must define a target market for investment products
- Must monitor product performance and suitability over time

### 3.3 AI-Specific EU Regulations

**EU AI Act (2024, effective 2025-2026)**:

- Financial services AI is classified as "high-risk" under Annex III
- Requirements: risk management system, data governance, technical documentation, transparency, human oversight, accuracy/robustness
- Investment management AI must: be transparent about AI use, provide explanations of decisions, maintain human oversight, conduct regular testing
- Penalties: Up to 7% of global annual turnover for non-compliance

**DORA (Digital Operational Resilience Act, effective January 2025)**:

- Applies to all EU financial entities
- ICT risk management requirements
- Incident reporting
- Digital operational resilience testing
- Third-party risk management

### 3.4 EU Regulatory Path for Midas

The EU is the most demanding jurisdiction for AI-driven investment management due to the combination of MiFID II, EU AI Act, and DORA. Recommended approach:

1. Establish in a business-friendly EU jurisdiction (Ireland, Luxembourg, or Netherlands)
2. Obtain portfolio management authorization from the local NCA
3. Passport services across the EU
4. Comply with EU AI Act high-risk requirements from day one

---

## 4. Regulatory Exemptions and Alternatives

### 4.1 US Exemptions

**Exemption from SEC registration** (Section 203(b)):

- **Intrastate adviser exemption**: If all clients are in one state and adviser doesn't advise on listed securities. Narrow and impractical for Midas.
- **Private fund adviser exemption (Section 203(m))**: For advisers solely to private funds with less than $150M AUM in the US. Requires filing as exempt reporting adviser (ERA). Could be relevant if Midas structures as a private fund rather than separate accounts.
- **Venture capital fund adviser exemption**: Not applicable.
- **Foreign private adviser exemption**: Not applicable for US operations.

**De minimis exemption**: States generally exempt advisers with fewer than 5 clients in the state. However, this is impractical for a scalable product.

**Technology platform vs. adviser**: Some argue that a platform providing investment tools (not advice) is not an investment adviser. However, the SEC has consistently held that automated investment advice IS investment advice. Midas cannot avoid registration by calling itself a "technology platform" if it provides personalized investment recommendations.

### 4.2 Singapore Exemptions

- **Exempt fund manager**: Not available for retail investors
- **Accredited investor exemption**: If serving only accredited investors (net personal assets exceeding S$2M, income exceeding S$300K/year), lighter regulatory burden
- **Sub-threshold fund manager**: AUM below certain thresholds may have simplified requirements

### 4.3 Private Fund Structure

Instead of managing individual client accounts (separately managed accounts / SMAs), Midas could structure as a private fund:

- **Advantages**: Single entity to manage; simpler compliance for limited number of investors; performance fees (2/20) are standard
- **Disadvantages**: Less flexible for clients; higher minimum investment; more complex fund documentation; audit requirements
- **SEC implications**: Exempt reporting adviser status at under $150M AUM; full registration above

### 4.4 Sub-Advisory / Outsourced CIO Model

Midas could act as a sub-adviser to existing RIAs:

- RIA partners handle client relationships, compliance, and custody
- Midas provides the autonomous investment strategy as a managed model
- Reduces Midas's direct regulatory burden
- Similar to how asset managers provide model portfolios to Orion or Tamarac
- Revenue model: basis points on AUM or flat licensing fee

---

## 5. Compliance Technology (RegTech)

### 5.1 Key Compliance Requirements for Midas

| Requirement            | Description                           | Frequency                        |
| ---------------------- | ------------------------------------- | -------------------------------- |
| Form ADV filing        | Registration and disclosure           | Annual update + material changes |
| Client suitability     | Risk assessment questionnaire         | At onboarding + periodic review  |
| Portfolio reporting    | Performance and holdings              | Quarterly (minimum)              |
| Fee billing disclosure | Fee calculation and deduction         | Monthly/quarterly                |
| Best execution review  | Trade execution quality               | Quarterly review, annual report  |
| Compliance testing     | Annual compliance review              | Annual                           |
| Books and records      | All communications, trades, decisions | Continuous; retain 5+ years      |
| Advertising review     | Marketing material compliance         | Before publication               |
| Code of ethics         | Personal trading, conflicts           | Annual acknowledgment            |

### 5.2 RegTech Solutions

- **Compliance software**: RIA in a Box, SmartRIA, NRS Comply -- provide compliance workflow, policy templates, annual review frameworks
- **Trading compliance**: Charles River, Eze Castle -- pre-trade and post-trade compliance checks
- **Surveillance**: NICE Actimize, Behavox -- communications monitoring, trade surveillance
- **Reporting**: Addepar, Orion, Black Diamond -- client reporting and performance (covered in market landscape)
- **KYC/AML**: Alloy, Jumio, Onfido -- identity verification, sanctions screening
- **Archiving**: Smarsh, Global Relay -- communications archiving (emails, chats, all electronic communications)

### 5.3 Compliance Architecture for Midas

**Automated compliance layer** (must be built into the system):

1. **Pre-trade compliance**: Before any trade is executed, check against:
   - Client investment policy statement (IPS) constraints
   - Concentration limits
   - Restricted securities list
   - Wash sale rules
   - Cross-trade restrictions (trading between client accounts)
2. **Post-trade compliance**: After execution, verify:
   - Best execution achieved
   - Trade allocation fairness (if trading across multiple accounts)
   - Settlement confirmation
3. **Continuous monitoring**:
   - Drift from target allocation
   - Drawdown alerts
   - Unusual trading patterns
   - Regulatory filing deadlines
4. **Audit trail**: Every decision the autonomous system makes must be logged with:
   - Timestamp
   - Input data considered
   - Decision rationale
   - Action taken
   - Outcome

---

## 6. Recommended Regulatory Path for Midas

### Phase 1: Personal Use ($5M, self-directed)

- **No registration required** for managing your own money
- Use this phase to build the system, establish track record, and refine strategies
- Duration: 6-18 months
- Key deliverable: Auditable performance track record

### Phase 2: Friends and Family (Informal, <5 clients)

- **De minimis exemption** may apply in some states
- However, if charging fees, registration is likely required
- **Recommended**: Register as a state-registered investment adviser even at this stage to establish the compliance foundation
- Duration: 6-12 months
- Key deliverable: Compliance infrastructure, client onboarding process

### Phase 3: Productization (Multi-client, >$5M external AUM)

- **Full state RIA registration** (or SEC if $100M+ AUM)
- Establish custodial relationships (Alpaca Broker API, or IBKR advisor accounts)
- Implement client suitability questionnaire and IPS generation
- File Form ADV Parts 1, 2A, 2B, 3
- Appoint CCO (can be outsourced initially)
- Obtain E&O insurance
- Duration: Ongoing
- Key deliverable: Registered, operational, compliant investment adviser

### Phase 4: Scale ($100M+ AUM)

- **SEC registration** required at $100M+
- Enhanced compliance requirements (annual compliance review, surprise examination if custody)
- Consider institutional custodian (Schwab Advisor Services, Fidelity Institutional)
- Duration: Ongoing

### Phase 5: International (Optional)

- Singapore: CMS license (RFMC initially)
- EU: MiFID II authorization in chosen jurisdiction
- Consider: Regulatory complexity increases significantly with each jurisdiction

---

## 7. Critical Regulatory Risks

### Risk 1: AI Accountability Gap

**Issue**: When an autonomous AI system makes a poor investment decision that harms clients, who is responsible?
**Current law**: The RIA (Midas) is responsible. Fiduciary duty cannot be delegated to an algorithm.
**Mitigation**: Human oversight mechanisms; clear documentation that the RIA accepts responsibility for all algorithmic decisions; kill switches and drawdown limits.

### Risk 2: Suitability of Autonomous Advice

**Issue**: Regulators may challenge whether a fully autonomous system can adequately assess client suitability and provide appropriate advice.
**Current precedent**: SEC has accepted robo-advisors with sufficient disclosures and questionnaires. However, Midas's autonomous active management is more complex than passive allocation.
**Mitigation**: Robust client onboarding (detailed risk questionnaire, financial situation assessment); clear investment policy statements; periodic suitability reviews; ability for clients to set constraints.

### Risk 3: Evolving Regulation

**Issue**: SEC's proposed PDA rules and the EU AI Act will impose new requirements on AI-driven investment advisers. Regulations are a moving target.
**Mitigation**: Design the system with explainability, auditability, and human oversight from day one. These are likely requirements in any future regulatory framework.

### Risk 4: Cross-Border Complexity

**Issue**: If clients are in multiple jurisdictions, Midas must comply with each jurisdiction's regulations.
**Mitigation**: Start with US-only clients; expand internationally only after establishing a strong compliance foundation.

### Risk 5: Advertising and Performance Claims

**Issue**: SEC Rule 206(4)-1 (Marketing Rule, effective November 2022) regulates how RIAs can advertise. AI-driven returns must be presented accurately with appropriate disclosures.
**Mitigation**: Follow the Marketing Rule strictly; include all required disclosures; do not guarantee or imply guaranteed returns; present performance net of fees with appropriate benchmarks.

---

## Sources and References

- SEC: Investment Advisers Act of 1940; Rules 206(4)-2, 206(4)-7, 206(4)-1; IM Guidance Update 2017-02 (Robo-Advisers)
- SEC: Proposed Rule on Predictive Data Analytics (2023)
- FINRA: Regulatory Notice 15-09 (Algorithmic Trading); FINRA Rules 3110 (Supervision)
- MAS: Securities and Futures Act 2001; Capital Markets Services License guidelines; FEAT Principles (2019, updated 2022)
- EU: MiFID II (Directive 2014/65/EU); EU AI Act (Regulation 2024/1689); DORA (Regulation 2022/2554)
- SEC Form ADV Instructions (2024 revision)
- "Regulation of Robo-Advisers" -- SEC Office of Investor Education and Advocacy
- RIA in a Box: Compliance cost estimates (2024-2025 RIA Benchmarking Survey)
- "The Fiduciary Duty of AI: Challenges for Investment Advisers Using AI" -- Harvard Law School Forum on Corporate Governance (2024)
