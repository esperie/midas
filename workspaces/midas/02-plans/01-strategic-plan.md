# Midas Strategic Plan

**Date**: 2026-03-23
**Status**: Proposed -- Awaiting Stakeholder Approval
**Capital at Risk**: $5,000,000 (Phase 1), scaling to multi-client AUM (Phase 2+)
**Risk Level**: High (financial system with fiduciary obligations)

---

## 1. Vision Statement

Midas is an autonomous investment management system that manages your $5 million portfolio without requiring your daily attention. It buys diversified investments, continuously monitors risk, optimizes your taxes, and protects your money with automatic safety mechanisms -- all while you go about your life. Once proven on your own money, Midas becomes a product that lets you manage investments for other people as a registered investment advisor, earning fees on the capital you manage. The long-term goal is a self-service platform where clients sign up, fund accounts, and have their money managed autonomously -- a modern wealth management business powered by intelligent automation.

---

## 2. Strategic Positioning

### Where Midas Sits in the Market

The automated investment space is crowded at the low end (Wealthfront, Betterment, Schwab -- all offering passive rebalancing) and at the high end (Renaissance Technologies, Two Sigma -- quantitative hedge funds with billions in infrastructure). Midas occupies a specific gap that no existing platform fills:

**Fully autonomous + multi-asset + AI-native + tax-optimized + multi-client**

| Dimension               | Robo-Advisors (Wealthfront, Betterment) | Quant Platforms (QuantConnect) | Brokerages (IBKR)      | Midas     |
| ----------------------- | --------------------------------------- | ------------------------------ | ---------------------- | --------- |
| Autonomous operation    | Partial (rebalancing only)              | No (human builds strategies)   | No (tools only)        | Full      |
| Active risk management  | None                                    | Tools only                     | Tools only             | Built-in  |
| Multi-asset class       | Limited (ETFs only)                     | Excellent                      | Excellent              | Broad     |
| Multi-client management | No (Betterment B2B partial)             | No                             | Yes (advisor accounts) | Yes       |
| AI-driven decisions     | No (rule-based)                         | No (human-coded)               | No                     | Yes       |
| Tax optimization        | Excellent (Wealthfront)                 | None                           | None                   | Excellent |

### What Makes Midas Different

1. **Genuine autonomy**: Not automated rebalancing -- actual autonomous strategy execution with built-in risk management, drawdown protection, and tax optimization running without human involvement.
2. **Hybrid investment approach**: A passive core (70-80%) that captures market returns reliably, combined with systematic overlays (10-20%) that add value through factor tilts and tactical adjustments, plus an opportunistic sleeve (0-10%) that deploys capital during severe market dislocations.
3. **Safety-first design**: A multi-layered circuit breaker system with 12 trigger conditions, tiered drawdown protection, and broker-level standing stop-loss orders as the ultimate backstop.
4. **Track-record-first business model**: Your own $5M runs for 12+ months before accepting any client money. Auditable, transparent performance that builds trust through evidence, not marketing.

### Who Midas Serves

**Phase 1 (You)**: A high-net-worth individual with $5M who wants full control and transparency over an autonomous investment strategy, with tax optimization and risk management that a human advisor cannot execute as consistently.

**Phase 2 (First Clients)**: 5-10 accredited investors -- friends, family, or professional contacts -- who trust you personally and want access to the same system managing your own money. $10-50M aggregate AUM.

**Phase 3 (Product)**: Small RIAs (1-10 person firms managing $50M-$500M) who need a modern operating system for their practice, and affluent individual investors ($100K-$5M) who want more sophistication than Wealthfront offers.

### Addressable Market

| Segment                   | Total Addressable Market (US) | Midas Target Share (5 years) | Revenue Potential      |
| ------------------------- | ----------------------------- | ---------------------------- | ---------------------- |
| Mass affluent ($100K-$1M) | $15T                          | 0.01% ($1.5B AUM)            | $3.75M/year at 0.25%   |
| Affluent ($1M-$5M)        | $20T                          | 0.005% ($1B AUM)             | $5M/year at 0.50%      |
| High-net-worth ($5M+)     | $25T                          | 0.001% ($250M AUM)           | $1.25M/year at 0.50%   |
| RIA white-label (B2B)     | $10T                          | 0.005% ($500M AUM)           | $2.5M/year at SaaS fee |
| **Total**                 |                               | **$3.25B AUM**               | **$12.5M/year**        |

These are conservative estimates assuming Midas achieves product-market fit and regulatory standing.

---

## 3. Phased Roadmap

### Phase 1: Personal Investment Tool

**What gets built**: A working system that connects to Interactive Brokers, implements the hybrid investment strategy (passive core + systematic overlays), manages risk automatically, optimizes taxes, and reports performance -- all running without your daily involvement.

**What you can DO at the end**:

- Your $5M is invested according to a defined strategy, autonomously managed
- You receive alerts on significant events (trades, risk breaches, circuit breaker activations)
- You can view a dashboard showing portfolio value, returns vs. benchmark, risk metrics, and tax savings
- You can trigger an emergency stop at any time (circuit breaker) via CLI or SMS
- The system handles daily rebalancing checks, drawdown protection, cash management, and reconciliation

**Estimated timeline**: 3 autonomous execution sessions for implementation + 60 days of paper trading before live deployment

**Decisions you must make before this phase**:

1. What is your target allocation? (e.g., 60% US equity / 20% international / 15% bonds / 5% real estate)
2. What is your maximum acceptable drawdown? (Recommended starting point: 25%)
3. Do you already have an Interactive Brokers account?
4. Are you comfortable with 60 days of paper trading before live deployment?
5. Who is your backup operator? (Someone who can reset the circuit breaker if you are unavailable)

**Risks addressed**:

- Tiered drawdown protection prevents selling everything at the bottom of a crash (Risk #1: F-LOGIC-005, score 20/25)
- Price anomaly filtering prevents false triggers from bad data (Risk #4: F-DATA-004, score 12/25)
- Rebalancing loop prevention stops runaway trading (Risk #7: F-LOGIC-002, score 9/25)
- Circuit breaker system provides last-line-of-defense emergency halt (12 trigger conditions, 6 failure modes addressed)
- Idempotent order submission prevents duplicate trades after connection drops (Risk #10: F-BROKER-001, score 12/25)
- API credential security protects against unauthorized access (Risk #3: SEC-001, score 10/25)

**Phase 1 deliverables**:

| Session                        | Focus                         | Deliverables                                                                                                                                                                                                                                  |
| ------------------------------ | ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Session 1: Core Infrastructure | Brokerage connection and data | IBKR API client (behind abstract interface), DataFlow schema (securities, positions, transactions, valuations), market data ingestion, position sync, reconciliation, market hours calendar                                                   |
| Session 2: Investment Engine   | Strategy execution            | Portfolio model definition, valuation calculation, drift detection, rebalancing logic, order generation, pre-trade risk checks, order submission and monitoring, cash management                                                              |
| Session 3: Risk and Operations | Safety and monitoring         | Tiered drawdown protection, circuit breaker system (12 triggers, 6-step activation, reset procedure), concentration monitoring, daily operations workflow, performance calculation (TWR), alerting (email/SMS), paper trading mode, admin CLI |

**Success criteria (Phase 1)**:

- System executes a rebalance trade correctly in paper trading
- Portfolio TWR calculation matches manual verification
- Pre-trade risk checks block oversized orders
- Drawdown protection triggers at configured thresholds with tiered response
- Daily reconciliation matches IBKR positions exactly
- System operates autonomously for 60 days in paper trading without intervention
- After live deployment, returns are within 2% of target benchmark over 90 days

---

### Phase 2: Multi-Client Management

**What gets built**: The system expands to manage investments for other people. Client data is isolated, each client gets a portfolio matched to their risk tolerance, and the system handles compliance requirements (audit trail, fee calculation, reporting).

**What you can DO at the end**:

- Onboard new clients through a structured process (identity verification, risk assessment, account opening)
- Manage 10-100 client accounts with isolated data and individual risk profiles
- Generate performance reports for each client
- Calculate and collect management fees automatically
- Maintain an immutable audit trail of every investment decision
- Aggregate orders across clients for better execution (block trading)

**Estimated timeline**: 3-5 autonomous execution sessions (after Phase 1 is stable and has run live for at least 6 months)

**Prerequisites (non-software, must be completed BEFORE Phase 2 software)**:

- Register as a Registered Investment Advisor (state-level if under $100M AUM, SEC if over $100M)
- Engage a compliance consultant ($20K-$50K in legal fees)
- Set up an IBKR Advisor account for institutional custody
- Obtain Errors & Omissions insurance
- Create Form ADV Part 2A (disclosure brochure)

**Decisions you must make before this phase**:

1. When will you begin the RIA registration process? (Recommended: start during Phase 1 -- it takes 3-6 months)
2. What fee will you charge clients? (Industry range: 0.25-1.00% of AUM annually)
3. What is the minimum account size you will accept? ($10K? $100K? This determines your target market.)
4. Who are your first 10 potential clients? (Real people, not hypothetical segments.)
5. What is your fee model? (AUM-based, flat subscription, or performance-based)

**Risks addressed**:

- RIA registration (Risk #2: REG-001, score 20/25) -- must be completed before managing any client money
- Fiduciary documentation (Risk #6: REG-002, score 15/25) -- audit trail and suitability checks
- PII encryption (Risk #11: REG-006, score 10/25) -- column-level encryption for SSN, DOB, financial data
- Single person dependency (Risk #8: OPR-001, score 12/25) -- backup operator and business continuity plan
- Wash sale coordination across client accounts (Risk #12: F-LOGIC-003, score 9/25)

**Phase 2 deliverables**:

| Session                     | Focus                   | Deliverables                                                                                                                                                        |
| --------------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Session 4: Multi-Tenancy    | Client data isolation   | DataFlow multi-tenancy, client data models, onboarding workflow with KYC integration, risk questionnaire, suitability engine, per-client model portfolio assignment |
| Session 5: Compliance       | Regulatory requirements | Immutable audit trail, block trading across clients, per-client performance reporting, fee calculation and collection, client communication archiving               |
| Sessions 6-7: Client Portal | Client-facing interface | Nexus REST API, client authentication (Auth0), performance dashboard, holdings view, transaction history, report downloads                                          |

**Success criteria (Phase 2)**:

- Client data is provably isolated (no cross-client data leakage in testing)
- Onboarding workflow completes end-to-end for test clients
- Per-client performance reports are accurate and generated on schedule
- Fee calculation matches manual calculation to the penny
- Audit trail captures every investment decision with rationale
- Block trades are allocated fairly across clients

---

### Phase 3: Self-Service Platform

**What gets built**: Midas transforms from a managed service into a self-service platform where clients can sign up online, verify their identity, fund accounts, and have their money managed -- all without manual intervention from you.

**What you can DO at the end**:

- Clients sign up and get started in under 10 minutes
- Mobile app for iOS and Android
- Automated identity verification and regulatory compliance at scale
- Multi-custodian support (not locked to IBKR)
- Disaster recovery and business continuity that meets institutional standards
- SOC 2 Type II certification for enterprise and institutional clients

**Estimated timeline**: 5-8 autonomous execution sessions (after Phase 2 has run successfully for 12+ months with growing client base)

**Prerequisites**:

- Phase 2 running successfully with retained, satisfied clients
- Proven track record of at least 12-18 months
- Revenue from Phase 2 demonstrating viable unit economics
- SEC registration if AUM exceeds $100M

**Decisions you must make before this phase**:

1. Is there proven demand? (Are Phase 2 clients retained and referring others?)
2. Should this be B2C (direct to investors), B2B (tools for advisors), or both?
3. Budget for institutional infrastructure (redundancy, DR, SOC 2 audit: $50K-$150K)
4. Hiring: compliance officer, potentially a quant researcher?
5. International expansion (Singapore via MAS license)?

**Risks addressed at this stage**:

- Scaling too fast without operational maturity
- Increased regulatory scrutiny with growing AUM
- Competition from established robo-advisors
- Technology reliability at production scale

---

### Phase 4: Scale (Year 4-5, if validated)

**What this looks like**:

- Institutional channels and partnerships
- International expansion (Singapore MAS license, building on the Endowus regulatory precedent)
- Target $500M-$1B AUM
- Potential fundraising if unit economics are proven
- Strategy marketplace (third-party quants publish strategies, expanding the platform model)

This phase is deliberately left open. It depends entirely on whether Phases 1-3 prove the business model. Building plans for Phase 4 now would be premature.

---

## 4. Architecture Summary

### For the Non-Technical Reader

Think of Midas as a team of specialized workers, each with a specific job, all working together inside one office (one computer system):

- **The Portfolio Manager** decides when and how to adjust investments. It looks at how far the current portfolio has drifted from the target and decides whether to trade.
- **The Risk Monitor** watches everything the Portfolio Manager does and can veto any decision that looks too risky. It can also hit the emergency stop button if something goes seriously wrong.
- **The Data Worker** fetches market prices, company information, and economic data. It checks that the data is fresh and accurate before passing it along.
- **The Executor** is the only one allowed to actually place trades with the brokerage. It only acts on instructions approved by the Portfolio Manager and vetted by the Risk Monitor.
- **The Tax Specialist** scans the portfolio for opportunities to save on taxes (selling investments at a loss to offset gains) while making sure it does not violate tax rules.
- **The Reporter** generates performance summaries, tax reports, and dashboards.

Each worker has strict limits on what it can do. The Tax Specialist cannot place trades. The Data Worker cannot change the investment strategy. The Risk Monitor can stop everything but cannot start trades. These limits are enforced by the system, not by trust -- even if one component malfunctions, it cannot exceed its authority.

The circuit breaker is the fire alarm. When it goes off, everything stops. It can only be restarted by a human.

### Technical Appendix

**Architecture pattern**: Modular monolith with clear internal boundaries. Single deployable application with well-defined modules (investment engine, brokerage client, risk manager, data pipeline). Each module could be extracted into a service later, but there is no reason to pay that operational cost now.

**Rationale**: For a single-operator system managing real money, operational simplicity is paramount. One process to monitor, one log stream, one database backup. Microservices solve team coordination problems that do not exist here. Serverless is rejected because investment logic requires stateful, time-sensitive operations.

**Technology mapping (Kailash SDK)**:

| Midas Capability                  | Kailash Framework         | How It Maps                                                                           |
| --------------------------------- | ------------------------- | ------------------------------------------------------------------------------------- |
| Investment workflow orchestration | Core SDK                  | Each process (rebalance, harvest, onboard) is a Kailash workflow                      |
| Market data ingestion             | Core SDK                  | Scheduled workflows: fetch, validate, store                                           |
| Portfolio optimization            | Core SDK + PythonCodeNode | Optimization math in PythonCodeNode                                                   |
| Order management                  | Core SDK                  | Order lifecycle as workflow: generate, risk check, submit, monitor, reconcile         |
| Risk monitoring                   | Core SDK + Kaizen         | Continuous risk agent monitors portfolio, triggers defensive workflows                |
| Database operations               | DataFlow                  | All persistent data through DataFlow models                                           |
| Client-facing API                 | Nexus                     | REST API for portfolio data, performance, reports                                     |
| Admin CLI                         | Nexus                     | Manual overrides, diagnostics, system control                                         |
| Autonomous investment agents      | Kaizen                    | Multi-agent system with specialized roles                                             |
| Financial governance              | PACT                      | Trading limits, approval workflows, hard constraints enforced by governance envelopes |
| Audit trail                       | PACT + DataFlow           | Every decision logged through governance records                                      |
| Multi-tenant data                 | DataFlow                  | Client isolation enforced at framework level                                          |

**Brokerage integration**: Interactive Brokers (IBKR) as primary broker, accessed through an abstract `BrokerageClient` interface. All investment logic interacts with this interface, never with IBKR-specific code. A second broker (Alpaca for consumer-facing clients) can be added by implementing the same interface.

**Data architecture**: Shared database with tenant isolation at the application layer. Single database with `client_id` on every table, enforced at the DataFlow framework level. Phase 1 uses SQLite; Phase 2 migrates to PostgreSQL.

**Build vs. Buy decisions**:

| Capability                                                                                               | Decision                | Rationale                                                 |
| -------------------------------------------------------------------------------------------------------- | ----------------------- | --------------------------------------------------------- |
| Portfolio optimization, order management, risk management, rebalancing, tax-loss harvesting, backtesting | BUILD                   | Core differentiators -- the product IS these capabilities |
| KYC/AML verification                                                                                     | BUY (Alloy, Persona)    | Regulatory minefield, commodity service                   |
| Authentication                                                                                           | BUY (Auth0)             | Commodity, security-critical, do not build                |
| Compliance filing                                                                                        | OUTSOURCE               | Legal expertise required                                  |
| Custody                                                                                                  | BUY (brokerage custody) | Never hold client assets directly                         |
| Email/SMS notifications                                                                                  | BUY (SendGrid, Twilio)  | Commodity services                                        |

---

## 5. Investment Strategy

### The Hybrid Approach

Midas uses a three-layer investment strategy designed to capture market returns reliably while adding value through tax optimization, risk management, and disciplined systematic tilts.

**Layer 1: Passive Core (70-80% of portfolio)**

Broad market index ETFs across asset classes (US equities, international equities, bonds, real estate). This layer captures the market return cheaply and reliably. Research consistently shows that most active managers underperform passive indexes after fees over long periods. The passive core ensures Midas does not take that losing bet.

Example allocation for a growth-oriented investor:

- 40% US total market (VTI or equivalent)
- 15% International developed (VXUS or equivalent)
- 5% Emerging markets
- 15% US aggregate bonds
- 5% Real estate (VNQ or equivalent)

**Layer 2: Tactical Overlay (10-20% of portfolio)**

Rules-based tilts driven by valuation signals, momentum factors, and economic regime indicators. This is not stock picking -- it is systematic factor-based allocation shifts. Example: when value stocks are historically cheap relative to growth stocks, tilt 5% toward value ETFs. When momentum signals are strong, increase equity allocation slightly.

All overlay rules are codified, backtestable, and auditable. No discretionary calls. If the overlay underperforms a passive benchmark by more than 2% annualized over a rolling 12-month period, it automatically disables and falls back to pure passive.

**Layer 3: Opportunistic Sleeve (0-10% of portfolio)**

Cash held for deployment during severe market dislocations (greater than 20% market drawdown). Rules-based entry: when the market drops significantly, this sleeve deploys into equities at discounted prices. This is the "buy when others are fearful" mechanism, executed by rules rather than emotion.

**Layer 4: Alpha Layer (embedded across all layers)**

Tax-loss harvesting, smart rebalancing, and cash management. These are the "free" sources of value that a systematic approach captures better than a human:

- **Tax-loss harvesting**: Scans daily for positions with unrealized losses above a threshold, sells them, and immediately buys a correlated substitute to maintain market exposure. Estimated value: 0.5-1.5% per year on $5M.
- **Smart rebalancing**: Uses new cash inflows to rebalance (avoiding unnecessary sells and tax events). Asymmetric thresholds prevent oscillation.
- **Cash management**: Maintains a buffer for operations and withdrawals without being over-allocated to cash (which drags returns).

### What This Is NOT

- It is not high-frequency trading. Midas trades days or weeks apart, not milliseconds.
- It is not stock picking. The system invests in diversified ETFs, not individual stock bets.
- It is not market timing. The tactical overlay shifts allocations by small amounts based on long-term signals, not day-to-day market predictions.
- It is not "AI that beats Wall Street." The AI drives risk management, tax optimization, and disciplined execution -- areas where evidence of value is strong. It does not claim to generate alpha through prediction.

---

## 6. Risk Management Framework

### How the System Protects Money

Midas has four layers of protection, each operating independently:

**Layer 1: Pre-Trade Risk Checks**

Before any trade is submitted to the brokerage, it passes through a set of checks:

- No single position can exceed 10% of the portfolio
- No sector can exceed 25% of the portfolio
- No trade can exceed 1% of portfolio value ($50,000 on $5M)
- Daily aggregate trading volume cannot exceed 5% of portfolio value ($250,000)
- Cash cannot drop below 1% of portfolio value ($50,000 floor)

If any check fails, the trade is blocked and an alert is sent. The system does not override these checks under any circumstances.

**Layer 2: Tiered Drawdown Protection**

When the portfolio declines from its peak value, the system responds in stages rather than all at once. This prevents the catastrophic mistake of selling everything at the bottom of a temporary dip:

| Portfolio Decline from Peak | Response                                                             |
| --------------------------- | -------------------------------------------------------------------- |
| 10%                         | Reduce equity allocation by 20%                                      |
| 15%                         | Reduce equity allocation by 40%                                      |
| 20%                         | Reduce equity allocation by 60%                                      |
| 25%                         | Full defensive posture (10% equity only) + circuit breaker activates |

Re-entry rules prevent buying back immediately when prices recover. A sustained recovery (5% above trough, maintained for 5 trading days) is required before increasing equity allocation. This prevents whipsaw losses -- the single highest-impact investment logic risk (estimated $100K-$750K if unmitigated).

**Layer 3: Circuit Breaker**

The circuit breaker is the emergency stop. When it activates, all automated trading halts immediately and can only be restarted by a human. It triggers on any of 12 conditions:

| Trigger                          | Threshold                                              |
| -------------------------------- | ------------------------------------------------------ |
| Portfolio drawdown from peak     | Greater than 25%                                       |
| Daily loss                       | Greater than 5% ($250,000)                             |
| Trade rejection rate             | Greater than 50% of orders in a batch                  |
| Position reconciliation mismatch | Greater than 1%                                        |
| Data staleness                   | Greater than 30 minutes during market hours            |
| API disconnection                | Greater than 5 minutes during market hours             |
| Excessive daily trades           | Greater than 50 in a day                               |
| System crash frequency           | Greater than 3 in 1 hour                               |
| Manual trigger                   | Operator or backup operator command (CLI, SMS, or API) |
| Risk limit breach                | Any hard limit violated                                |
| Unexpected position              | Position not ordered by Midas                          |
| Cash depletion                   | Below 0.5% of portfolio                                |

When triggered, the circuit breaker:

1. Immediately halts all order generation (less than 1 second)
2. Cancels all open orders at the brokerage (within 5 seconds)
3. Syncs and reconciles full portfolio state (within 30 seconds)
4. Sends alerts via SMS, email, and a third channel like Slack (within 1 minute)
5. Optionally reduces equity exposure if the trigger was drawdown-related
6. Logs everything to an append-only audit file

**Reset requires explicit human action.** There is no automatic reset. The operator must acknowledge the event, the system must pass reconciliation, and a 24-hour cooling period follows with doubled rebalancing thresholds and halved trade sizes.

The circuit breaker state is persisted to disk, so it survives system crashes and restarts. If the circuit breaker is active and the system restarts, it stays in emergency halt mode.

**Layer 4: Broker-Level Standing Orders (Backstop)**

Standing good-till-cancelled (GTC) stop-loss orders at the broker level. These execute independently of Midas -- even if the system is completely offline, the server is destroyed, or the network is down. They are the absolute last line of defense.

Trade-off: standing stop-loss orders can trigger during flash crashes that would otherwise recover quickly. The operator must decide whether this backstop is worth the whipsaw risk.

### Financial Risk Scenarios

Based on backtesting the hybrid strategy against historical market events:

| Scenario                                   | Historical Precedent    | Unmitigated Impact on $5M | Mitigated Impact (with Midas protections)                 |
| ------------------------------------------ | ----------------------- | ------------------------- | --------------------------------------------------------- |
| Flash crash (10% in minutes)               | May 2010, Aug 2015      | -$500K unrealized         | -$50K to -$150K                                           |
| Prolonged bear market (30%+ over months)   | 2007-2009, 2000-2002    | -$1.5M to -$2.5M          | -$500K to -$1M                                            |
| Black swan (exchange halt, broker failure) | Lehman 2008, March 2020 | Up to -$5M                | -$250K to -$2M (irreducible)                              |
| Strategy drift (underperformance)          | Value factor 2010-2020  | -$50K to -$250K/year      | Auto-disable overlay, fall back to passive                |
| Liquidity crisis                           | March 2020 bonds        | -$25K to -$250K           | Liquid ETFs only, limit orders, suspend non-urgent trades |

### 88 Identified Risk Items

The risk analysis identified 88 distinct risk items across 7 categories:

- 5 brokerage integration failures
- 5 investment logic failures
- 4 data pipeline failures
- 4 infrastructure failures
- 6 financial scenarios
- 10 regulatory risks
- 4 operational risks
- 6 security threats
- 12 circuit breaker triggers
- 6 circuit breaker failure modes
- 26 additional sub-risks in the detailed analysis

All risks have at least one concrete mitigation. The top 15 by severity are tracked in the priority matrix (see risk analysis document for full detail).

---

## 7. Regulatory Path

### Phase 1: No Registration Required

Managing your own money does not require regulatory registration. Phase 1 is a personal investment tool and has no compliance requirements beyond standard tax reporting.

### Phase 2: Investment Adviser Registration

**When to start**: Begin the registration process during Phase 1 development, not after. Registration takes 3-6 months. Delaying registration delays the entire Phase 2 launch.

**What is required**:

| Requirement              | Detail                                                     | Cost                         |
| ------------------------ | ---------------------------------------------------------- | ---------------------------- |
| RIA registration         | State-level if under $100M AUM; SEC if over $100M          | Part of legal fees           |
| Form ADV Parts 2A and 2B | Disclosure brochure describing fees, strategies, conflicts | $20K-$50K legal fees (total) |
| Compliance program       | Written policies, designated Chief Compliance Officer      | Consultant or hire           |
| E&O insurance            | Errors & Omissions professional liability coverage         | $5K-$15K/year                |
| Annual ADV amendment     | Update Form ADV within 90 days of fiscal year end          | Part of ongoing compliance   |
| Books and records        | SEC Rule 204-2: maintain records for 5-6 years             | Software handles this        |
| State notice filings     | File in states where clients reside                        | Part of legal fees           |

**Estimated cost**: $20K-$50K initial registration + $10K-$30K/year ongoing compliance.

**Critical rule**: Do NOT manage any client money before registration is complete. Operating without registration carries personal criminal liability under the Investment Advisers Act of 1940. This is the second-highest-severity risk in the entire analysis (REG-001, score 20/25).

### Phase 3: SEC Registration and Institutional Compliance

When AUM exceeds $100M, the system transitions from state to SEC registration. Additional requirements include:

- SOC 2 Type II audit ($50K-$150K)
- Annual surprise custody examination
- Enhanced record-keeping and examination readiness
- Potentially international licensing (Singapore MAS if expanding to Asia)

### The "AI Advisor" Regulatory Question

Regulators (SEC) are increasingly scrutinizing AI-driven investment advisors. The system's decision-making must be:

- **Explainable**: Every trade decision must have a documented rationale in the audit trail
- **Auditable**: A regulator must be able to trace any trade back to the rule or signal that triggered it
- **Controlled**: The AI agent proposes actions; a rules engine with hard constraints validates them; the governance system (PACT) enforces limits

The strategy of using PACT governance envelopes to enforce hard trading limits is specifically designed for regulatory defensibility. Agents cannot exceed their authority regardless of their "reasoning."

---

## 8. Financial Model

### Build Cost

| Item                                       | Phase 1     | Phase 2        | Phase 3        | Total                   |
| ------------------------------------------ | ----------- | -------------- | -------------- | ----------------------- |
| Software development (autonomous sessions) | 3 sessions  | 3-5 sessions   | 5-8 sessions   | 11-16 sessions          |
| RIA registration (legal)                   | $0          | $20K-$50K      | $0             | $20K-$50K               |
| E&O insurance                              | $0          | $5K-$15K/year  | $5K-$15K/year  | $10K-$30K first 2 years |
| Compliance consultant                      | $0          | $10K-$30K/year | $10K-$30K/year | $20K-$60K first 2 years |
| SOC 2 audit                                | $0          | $0             | $50K-$150K     | $50K-$150K              |
| **Total build cost**                       | **Minimal** | **$35K-$95K**  | **$65K-$195K** | **$100K-$290K**         |

### Running Cost

| Item                             | Phase 1 (monthly) | Phase 2 (monthly) | Phase 3 (monthly) |
| -------------------------------- | ----------------- | ----------------- | ----------------- |
| Cloud infrastructure             | $50-$200          | $200-$500         | $1K-$5K           |
| Market data (Polygon/equivalent) | $100-$200         | $200-$500         | $500-$1K          |
| Secrets manager                  | $40               | $40               | $100              |
| Auth0 (client auth)              | $0                | $50-$200          | $200-$1K          |
| KYC service                      | $0                | $50-$200          | $500-$2K          |
| Notifications (Twilio, SendGrid) | $10               | $50-$100          | $200-$500         |
| Annual pen test (prorated)       | $0                | $400-$1.2K        | $400-$1.2K        |
| **Total monthly**                | **$200-$450**     | **$990-$2.7K**    | **$2.9K-$10.7K**  |

### Revenue Model (Phase 2+)

| AUM Milestone        | Fee Rate | Annual Revenue | Monthly Revenue |
| -------------------- | -------- | -------------- | --------------- |
| $10M (5-10 clients)  | 0.50%    | $50,000        | $4,167          |
| $50M (20-50 clients) | 0.40%    | $200,000       | $16,667         |
| $100M                | 0.35%    | $350,000       | $29,167         |
| $500M (with B2B)     | 0.30%    | $1,500,000     | $125,000        |
| $1B (5-year target)  | 0.25%    | $2,500,000     | $208,333        |

**Break-even**: At approximately $20-30M AUM, the revenue from fees covers the running costs and ongoing compliance expenses. At 0.50% on $25M AUM, that is $125K/year in revenue against roughly $50-80K/year in operating costs.

### Value of Tax Optimization (Personal Portfolio)

On a $5M portfolio, tax-loss harvesting and tax-coordinated portfolio management can save $10K-$50K per year in taxes. This alone can justify the cost of running the system, even without any investment performance improvement.

---

## 9. Success Metrics

### Phase 1 Success (Personal Use -- measured after 90 days live)

| Metric                                 | Target                            | How Measured                        |
| -------------------------------------- | --------------------------------- | ----------------------------------- |
| System uptime during market hours      | 99.9%                             | Monitoring dashboard                |
| Daily reconciliation accuracy          | 100% match                        | Automated daily check               |
| Portfolio return vs. benchmark         | Within 2% of target               | TWR calculation vs. benchmark index |
| Tax-loss harvesting value              | At least 1 valid harvest executed | Transaction log                     |
| Circuit breaker false positive rate    | Less than 20%                     | Post-analysis of all triggers       |
| Paper trading period stability         | 60 days without intervention      | System logs                         |
| Drawdown protection correct activation | Triggers at configured thresholds | Backtest validation                 |

### Phase 2 Success (Multi-Client -- measured after 6 months)

| Metric                       | Target                       | How Measured           |
| ---------------------------- | ---------------------------- | ---------------------- |
| Client data isolation        | Zero cross-client data leaks | Security testing       |
| Client onboarding completion | End-to-end in under 1 hour   | Process timer          |
| Performance report accuracy  | Matches manual calculation   | Quarterly audit        |
| Fee calculation accuracy     | To the penny                 | Monthly reconciliation |
| Audit trail completeness     | Every decision documented    | Compliance review      |
| Client retention             | 90% after 12 months          | Client count tracking  |
| AUM growth                   | $10-50M within 12 months     | Account aggregation    |

### Phase 3 Success (Product -- measured after 12 months)

| Metric                       | Target                     | How Measured         |
| ---------------------------- | -------------------------- | -------------------- |
| Self-service signup time     | Under 10 minutes           | UX measurement       |
| System handles 100+ accounts | No performance degradation | Load testing         |
| Market hours uptime          | 99.9% over 90 days         | Monitoring           |
| SOC 2 Type II                | Audit passed               | Audit report         |
| Revenue vs. operating costs  | Revenue exceeds costs      | Financial statements |

### Long-Term Health Indicators

- **Strategy alpha after fees**: Is the hybrid approach outperforming a simple 60/40 index fund after all costs? If not after 24 months, simplify to passive + TLH only.
- **Whipsaw cost tracking**: Are the drawdown protection actions costing more than they save? Review quarterly. If whipsaw costs exceed saved losses over 12 months, thresholds are too aggressive.
- **Client acquisition cost**: How much does it cost to acquire each new client? If the cost exceeds the first-year revenue from that client, the unit economics do not work.
- **Regulatory compliance score**: Track any regulatory findings, deficiency letters, or examination issues. Target: zero findings.

---

## 10. Open Questions -- Consolidated

These questions come from all analysis documents. They must be answered before implementation begins. Questions are grouped by when they are needed.

### Before Phase 1 Implementation (Must Answer Now)

| #   | Question                                                  | Context                                                                                                                                                                                                       | Recommendation                                                                                                    |
| --- | --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| 1   | **What is your target allocation?**                       | The system needs to know what to invest in. Example: 60% US equity / 20% international / 15% bonds / 5% real estate. Or do you want the system to recommend one based on your risk profile?                   | Start with a standard 60/30/10 (equity/bonds/alternatives) split and adjust based on your risk tolerance answers. |
| 2   | **What is your maximum acceptable drawdown?**             | This is the most important parameter in the system. It determines how aggressively drawdown protection triggers. A 20% limit means more whipsaw risk. A 40% limit means larger paper losses during downturns. | Start at 25%. This limits potential loss while avoiding excessive whipsawing based on historical backtesting.     |
| 3   | **Do you already have an Interactive Brokers account?**   | IBKR is the recommended primary broker for Phase 1. If you prefer a different broker, the architecture supports it but it changes the first session's work.                                                   | IBKR is recommended. Best execution, lowest costs, advisor platform ready for Phase 2.                            |
| 4   | **Who is your backup operator?**                          | If you are unavailable and the circuit breaker fires, someone needs to be able to respond. This person needs credentials and a laminated card with emergency shutdown instructions.                           | Designate someone you trust. They need basic training (15 minutes) and sealed credentials.                        |
| 5   | **Are you comfortable with 60 days of paper trading?**    | The risk analysis recommends extending the original 30-day paper trading period to 60 days. This provides more market conditions to validate the system but delays live deployment by 30 days.                | 60 days recommended. 30 days may not include enough market volatility to test defensive mechanisms.               |
| 6   | **Should the circuit breaker include defensive trading?** | When the circuit breaker fires on a drawdown trigger, should the system reduce equity exposure (adds protection but introduces whipsaw risk) or hold all positions until you intervene?                       | Enable it. The trade-off favors protection on a $5M portfolio.                                                    |
| 7   | **Do you want broker-level standing stop-loss orders?**   | Standing GTC stop-loss orders at IBKR execute even if Midas is completely offline. They are the ultimate backstop but can trigger during flash crashes that would otherwise recover.                          | Yes, set them at a wide level (30-35% below peak) as a catastrophic backstop only.                                |
| 8   | **What is this $5M relative to your total net worth?**    | If this is 100% of your wealth, the strategy should be more conservative. If it is 20%, you can accept more volatility. This directly affects target allocation and drawdown thresholds.                      | Your answer determines whether we calibrate toward preservation (conservative) or growth (moderate).              |
| 9   | **Rebalancing frequency and threshold?**                  | How often should the system check for drift? Daily with a 5% threshold is standard. Some prefer weekly or monthly.                                                                                            | Daily checks, 5% drift threshold, with a 24-hour cooldown between rebalancing operations.                         |
| 10  | **Budget for external security services?**                | Secrets manager (~$500/year), annual penetration testing ($5K-$15K/year for Phase 2), excess SIPC insurance. Total: approximately $6K-$16K/year.                                                              | Yes. This is negligible relative to the capital at risk.                                                          |

### Before Phase 2 (Can Answer Later, But Start Thinking Now)

| #   | Question                                              | Context                                                                                                                                                                   |
| --- | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 11  | **When will you start the RIA registration process?** | Takes 3-6 months. Starting now means Phase 2 is not delayed by regulatory lead time.                                                                                      |
| 12  | **Who are your first 10 potential clients?**          | Real people. If you cannot name them, you do not have product-market fit yet.                                                                                             |
| 13  | **What fee will you charge?**                         | 0.25% is the Wealthfront benchmark. 0.50% is reasonable with active risk management and tax optimization. 1.00% is traditional advisor pricing.                           |
| 14  | **What is the minimum account size?**                 | $10K minimum opens the mass affluent market. $100K focuses on affluent and HNW.                                                                                           |
| 15  | **What does "autonomously invest" mean to you?**      | Are you comfortable with fully hands-off (the system decides everything within risk limits)? Or do you want to approve certain actions (large trades, new asset classes)? |

### Strategic Questions (Answer When Ready)

| #   | Question                                      | Context                                                                                                                                                                                                                          |
| --- | --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 16  | **What is your competitive moat?**            | Technology alone is not a moat. What prevents a well-funded competitor from replicating Midas in 6 months? Regulatory license, proprietary data, distribution channel, brand trust, switching costs?                             |
| 17  | **B2C or B2B?**                               | B2C (managing retail investors' money directly) requires brand trust and regulatory infrastructure. B2B (selling tools to advisors -- the "Shopify for wealth management" model) has better margins and lower regulatory burden. |
| 18  | **What happens when the system loses money?** | Every strategy has drawdown periods. When your $5M drops to $4M, will you trust the system and stay the course, override and go to cash, or shut down? And what will you tell clients when the same happens to their money?      |
| 19  | **Have you considered starting simpler?**     | The fastest path to investing your $5M wisely is opening a Wealthfront account and buying index funds. The question is whether the SOFTWARE is worth building as a business. Phase 1 answers that question.                      |
| 20  | **International expansion?**                  | Singapore is a natural expansion market (MAS-regulated, Endowus as precedent, no capital gains tax for individuals). But this is a Phase 4 consideration at earliest.                                                            |

---

## Appendix A: Three Inconsistencies Between Requirements and Risk Analysis

The risk analysis identified three inconsistencies with the requirements breakdown. These have been incorporated into this plan:

**1. Paper Trading Duration**: Requirements specified 30 days. Risk analysis recommends 60 days because 30 days may not include enough market volatility to test the tiered drawdown protection (F-LOGIC-005) and optimizer sanity checks (F-LOGIC-001). **This plan uses 60 days.**

**2. Circuit Breaker Specification**: Requirements mentioned the circuit breaker (Risk Agent can "trigger circuit breaker") but did not define triggers, actions, or reset procedures. The risk analysis provides a complete specification with 12 triggers, 6-step activation sequence, reset procedure, and 6 circuit breaker failure modes. **This plan incorporates the full circuit breaker design.**

**3. Standing Stop-Loss Orders**: The risk analysis recommends broker-level GTC stop-loss orders as a backstop independent of Midas availability. Requirements did not mention this. **This plan adds it as a Phase 1 decision item (Question 7).**

---

## Appendix B: Go-to-Market Priorities (from Competitor Analysis)

Based on the competitive landscape analysis:

1. **Year 1**: Personal $5M portfolio. Build system, establish track record. No clients. No revenue. Focus: prove the system works.
2. **Year 2**: Friends/family pilot. 5-10 accredited investors. $10-50M AUM. State RIA registration. Focus: validate multi-client operations.
3. **Year 3**: Public launch. Consumer app + B2B/RIA partnerships. Target $100M+ AUM. SEC registration. Focus: product-market fit.
4. **Year 4-5**: Scale. Institutional channels. International expansion (Singapore). Target $500M-$1B AUM. Focus: growth and defensibility.

### Critical Success Factors (from Competitor Analysis)

1. **Demonstrated performance**: Auditable track record is table stakes. Without it, no sophisticated investor will trust an AI system.
2. **Regulatory compliance**: Early and thorough. Regulators will scrutinize an AI-driven adviser more than a traditional one.
3. **Risk management credibility**: Maximum drawdowns must be controlled. One large drawdown event in the first year of client money will be fatal.
4. **Transparent methodology**: Publish investment philosophy, strategy descriptions, risk framework. Build trust through openness.
5. **Operational reliability**: Near-perfect uptime. Trading failures, missed opportunities, or phantom orders are unacceptable.

---

## Appendix C: Priority Matrix -- Top 15 Risks

| Rank | Risk ID      | Description                               | Score (P x I) | Phase   | Status                                        |
| ---- | ------------ | ----------------------------------------- | ------------- | ------- | --------------------------------------------- |
| 1    | F-LOGIC-005  | Drawdown protection whipsawing            | 20/25         | Phase 1 | Mitigated by tiered response design           |
| 2    | REG-001      | Operating without proper RIA registration | 20/25         | Phase 2 | Requires early start on registration          |
| 3    | SEC-001      | Compromised brokerage API credentials     | 10/25         | Phase 1 | IP whitelisting, 2FA, secrets manager         |
| 4    | F-DATA-004   | Price anomalies triggering false drawdown | 12/25         | Phase 1 | Price confirmation, VWAP-based calculations   |
| 5    | F-BROKER-004 | Rate limiting during volatile markets     | 12/25         | Phase 1 | Priority queue, pre-computed defensive orders |
| 6    | REG-002      | Fiduciary breach allegation               | 15/25         | Phase 2 | Audit trail, suitability checks               |
| 7    | F-LOGIC-002  | Rebalancing loop (infinite trade cycle)   | 9/25          | Phase 1 | Cooldown period, asymmetric thresholds        |
| 8    | OPR-001      | Single person dependency                  | 12/25         | Phase 1 | Backup operator, break-glass procedure        |
| 9    | F-DATA-001   | Stale market data causing wrong decisions | 9/25          | Phase 1 | Staleness detection, halt-on-stale            |
| 10   | F-BROKER-001 | API disconnection during open orders      | 12/25         | Phase 1 | Idempotent orders, quiesce period             |
| 11   | REG-006      | Privacy/data breach with client PII       | 10/25         | Phase 2 | Column-level encryption, PII filtering        |
| 12   | F-LOGIC-003  | Wash sales from tax-loss harvesting       | 9/25          | Phase 1 | 60-day window tracking, blacklist             |
| 13   | F-LOGIC-001  | Optimizer producing degenerate portfolios | 10/25         | Phase 1 | Hard constraints, sanity checks               |
| 14   | F-INFRA-002  | Server crash during trade execution       | 8/25          | Phase 1 | Transaction log, idempotent orders            |
| 15   | OPR-004      | Vendor dependency (broker outage)         | 8/25          | Phase 1 | Abstraction layer, SIPC coverage              |

---

_This strategic plan should be reviewed and approved by the stakeholder before implementation begins. It should be updated after Phase 1 paper trading results are available, after Phase 2 regulatory registration is complete, and annually thereafter._
