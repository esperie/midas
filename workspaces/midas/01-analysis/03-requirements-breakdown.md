# Midas - Requirements Breakdown

**Date**: 2026-03-23
**Status**: Proposed
**Complexity**: High
**Risk Level**: High (financial system, regulatory exposure)

---

## Executive Summary

Midas is an autonomous investment management platform. It starts as a personal tool managing $5M, then scales into a multi-client product. The system must make investment decisions, execute trades, manage risk, and comply with financial regulations -- all without human intervention in the loop.

The critical insight: **managing your own money and managing other people's money are fundamentally different businesses.** Phase 1 is a software engineering problem. Phase 2 is a regulated financial services business. The requirements, risks, and regulatory burden change dramatically at that boundary.

---

## 1. Core Capabilities Breakdown

### 1.1 Investment Engine (Core)

These capabilities are needed from day one for personal use.

#### 1.1.1 Portfolio Construction and Optimization

| ID     | Requirement                | Description                                                            | Inputs                                           | Outputs                      | Edge Cases                                                   |
| ------ | -------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------ | ---------------------------- | ------------------------------------------------------------ |
| IE-001 | Asset universe definition  | Define which securities the system can invest in                       | Asset class constraints, exchange listings       | Filtered investable universe | Delisted securities, halted trading, restricted securities   |
| IE-002 | Portfolio optimization     | Compute target allocations using mean-variance or risk-parity approach | Expected returns, covariance matrix, constraints | Target weights per security  | Corner solutions (100% in one asset), infeasible constraints |
| IE-003 | Constraint engine          | Enforce position limits, sector limits, asset class bounds             | User-defined constraints, current positions      | Feasible allocation set      | Conflicting constraints, drift beyond bounds                 |
| IE-004 | Model portfolio management | Define and version model portfolios (templates)                        | Asset weights, rebalance rules                   | Versioned model portfolio    | Version conflicts during live rebalance                      |

**Business logic**: The optimizer runs periodically (daily or on trigger) to compute ideal portfolio weights. It respects hard constraints (no single position > X%, no sector > Y%) and soft constraints (target cash allocation). The output is a set of target weights, not trade orders -- the order management system handles execution.

#### 1.1.2 Trade Execution and Order Management

| ID     | Requirement          | Description                                                          | Inputs                                  | Outputs                          | Edge Cases                                             |
| ------ | -------------------- | -------------------------------------------------------------------- | --------------------------------------- | -------------------------------- | ------------------------------------------------------ |
| IE-010 | Order generation     | Convert target weights to buy/sell orders                            | Target vs current weights, prices       | Order list with quantities       | Fractional shares, minimum lot sizes, odd lots         |
| IE-011 | Order routing        | Submit orders to brokerage API                                       | Order list                              | Order confirmations / rejections | API downtime, partial fills, rate limits               |
| IE-012 | Execution monitoring | Track order status until filled or cancelled                         | Open orders                             | Fill reports                     | Stuck orders, partial fills over days, market halts    |
| IE-013 | Trade reconciliation | Verify fills match expected execution                                | Fill reports, expected orders           | Reconciliation status            | Price slippage, unexpected partial fills               |
| IE-014 | Smart order sizing   | Calculate order sizes accounting for commissions, minimum trade size | Target trade value, commission schedule | Adjusted order size              | Trade too small to be worth commissions, cash rounding |

**Business logic**: Orders are generated as the difference between current and target portfolios. The system should use limit orders during market hours and avoid trading during high-volatility periods. All orders go through a pre-trade risk check before submission.

#### 1.1.3 Risk Management and Monitoring

| ID     | Requirement              | Description                                             | Inputs                             | Outputs                      | Edge Cases                                          |
| ------ | ------------------------ | ------------------------------------------------------- | ---------------------------------- | ---------------------------- | --------------------------------------------------- |
| IE-020 | Pre-trade risk check     | Validate orders against risk limits before submission   | Proposed orders, risk limits       | Approve / reject with reason | Limit breach during market hours, cascading rejects |
| IE-021 | Portfolio risk metrics   | Calculate VaR, drawdown, beta, volatility               | Positions, market data             | Risk dashboard metrics       | Data gaps, extreme market conditions                |
| IE-022 | Concentration monitoring | Alert on single-position or sector concentration        | Current positions                  | Concentration warnings       | Position growth via appreciation (not trading)      |
| IE-023 | Drawdown protection      | Reduce exposure if portfolio drawdown exceeds threshold | Portfolio value history, threshold | Defensive rebalance trigger  | Flash crashes, after-hours gaps                     |
| IE-024 | Correlation monitoring   | Track correlation shifts that affect diversification    | Returns history                    | Correlation matrix, alerts   | Regime changes, crisis correlations                 |

**Business logic**: Risk management operates as a governor on the investment engine. It can block trades (pre-trade) or trigger defensive actions (real-time monitoring). Drawdown protection is the most critical -- if the portfolio falls X% from peak, the system should automatically reduce equity exposure.

#### 1.1.4 Rebalancing Logic and Triggers

| ID     | Requirement         | Description                                         | Inputs                              | Outputs                       | Edge Cases                                     |
| ------ | ------------------- | --------------------------------------------------- | ----------------------------------- | ----------------------------- | ---------------------------------------------- |
| IE-030 | Calendar rebalance  | Rebalance on a fixed schedule (monthly/quarterly)   | Schedule, current vs target weights | Rebalance trade list          | Rebalance day falls on holiday, market closed  |
| IE-031 | Drift rebalance     | Rebalance when allocation drifts beyond threshold   | Drift thresholds, current weights   | Triggered rebalance           | Multiple assets drifting simultaneously        |
| IE-032 | Cash-flow rebalance | Invest new cash or raise cash for withdrawals       | Cash event, target weights          | Investment/liquidation orders | Large cash event requiring multi-day execution |
| IE-033 | Tax-aware rebalance | Prefer rebalancing via new cash to avoid tax events | Tax lot data, target weights        | Tax-optimized trade list      | All lots have gains, forced selling            |

#### 1.1.5 Tax-Loss Harvesting

| ID     | Requirement                   | Description                                                                           | Inputs                           | Outputs                | Edge Cases                                                |
| ------ | ----------------------------- | ------------------------------------------------------------------------------------- | -------------------------------- | ---------------------- | --------------------------------------------------------- |
| IE-040 | Loss identification           | Scan positions for unrealized losses above threshold                                  | Positions, cost basis, threshold | Harvestable loss list  | Wash sale window conflicts                                |
| IE-041 | Wash sale prevention          | Track 30-day wash sale windows across accounts                                        | Trade history, pending orders    | Blocked/allowed status | Dividend reinvestments triggering wash sales              |
| IE-042 | Substitute security selection | Replace harvested position with correlated but not "substantially identical" security | Sold security, universe          | Replacement security   | All substitutes also at a loss, no good substitute exists |
| IE-043 | Loss harvesting execution     | Execute sell-and-replace as atomic operation                                          | Loss opportunity, substitute     | Executed harvest pair  | Partial fill on one leg, price movement between legs      |

**Business logic**: Tax-loss harvesting scans for positions with unrealized losses exceeding a minimum threshold (e.g., $1,000). It sells the losing position and immediately buys a substitute to maintain market exposure. The system must track wash sale windows to avoid IRS disallowance.

#### 1.1.6 Cash Management

| ID     | Requirement             | Description                                           | Inputs                         | Outputs            | Edge Cases                                                |
| ------ | ----------------------- | ----------------------------------------------------- | ------------------------------ | ------------------ | --------------------------------------------------------- |
| IE-050 | Cash target maintenance | Maintain a cash buffer for operations and withdrawals | Target cash %, current cash    | Buy/sell to target | Cash < target after market drop (don't sell into decline) |
| IE-051 | Cash sweep              | Invest excess cash above target                       | Cash balance, target           | Investment orders  | Very small excess not worth trading                       |
| IE-052 | Withdrawal management   | Raise cash for planned withdrawals                    | Withdrawal schedule, positions | Liquidation plan   | All positions at a loss, tax impact                       |

#### 1.1.7 Performance Calculation and Attribution

| ID     | Requirement                     | Description                                      | Inputs                              | Outputs               | Edge Cases                      |
| ------ | ------------------------------- | ------------------------------------------------ | ----------------------------------- | --------------------- | ------------------------------- |
| IE-060 | Time-weighted return (TWR)      | Calculate returns eliminating cash flow effects  | Daily valuations, cash flows        | TWR for any period    | Multiple intra-day cash flows   |
| IE-061 | Money-weighted return (MWR/IRR) | Calculate returns including cash flow timing     | Valuations, cash flow dates/amounts | IRR for any period    | IRR calculation non-convergence |
| IE-062 | Benchmark comparison            | Compare portfolio return vs benchmark            | Portfolio TWR, benchmark returns    | Alpha, tracking error | Benchmark changes composition   |
| IE-063 | Attribution analysis            | Explain return sources (allocation vs selection) | Sector weights and returns          | Attribution report    | Cash drag, rebalancing effects  |

---

### 1.2 Client Management (Product -- Phase 2+)

These capabilities are needed only when managing money for other people.

#### 1.2.1 Client Onboarding

| ID     | Requirement                      | Description                                     | Inputs                                | Outputs                  | Edge Cases                                            |
| ------ | -------------------------------- | ----------------------------------------------- | ------------------------------------- | ------------------------ | ----------------------------------------------------- |
| CM-001 | KYC collection                   | Collect identity verification documents         | Name, DOB, SSN, address, ID documents | Verified identity record | Non-US persons, PO box addresses, name mismatches     |
| CM-002 | AML screening                    | Screen against OFAC/SDN lists and PEP databases | Client identity data                  | Clear / flagged status   | False positives, partial name matches                 |
| CM-003 | Accredited investor verification | Verify accredited status if required            | Income/net worth documentation        | Accreditation status     | Documentation expired, borderline cases               |
| CM-004 | Investment advisory agreement    | Generate and collect signed IAA                 | Client info, fee schedule             | Executed agreement       | Client in different state, varying fee structures     |
| CM-005 | Account opening                  | Open brokerage accounts via custodian API       | Client info, account type             | Active account           | Custodian rejects application, manual review required |

#### 1.2.2 Risk Profiling

| ID     | Requirement               | Description                           | Inputs                                | Outputs                  | Edge Cases                                         |
| ------ | ------------------------- | ------------------------------------- | ------------------------------------- | ------------------------ | -------------------------------------------------- |
| CM-010 | Risk questionnaire        | Assess client risk tolerance          | Questionnaire responses               | Risk score (1-10)        | Inconsistent answers, extreme scores               |
| CM-011 | Suitability determination | Match risk score to model portfolio   | Risk score, investment horizon, goals | Assigned model portfolio | Score change after assignment, life event triggers |
| CM-012 | Risk profile updates      | Periodically re-assess risk tolerance | Updated questionnaire                 | Updated assignment       | Client wants to change despite suitability concern |

#### 1.2.3 Account Management

| ID     | Requirement            | Description                                                | Inputs                 | Outputs                     | Edge Cases                                                   |
| ------ | ---------------------- | ---------------------------------------------------------- | ---------------------- | --------------------------- | ------------------------------------------------------------ |
| CM-020 | Account types          | Support individual, joint, trust, IRA, Roth IRA, SEP, 401k | Account type selection | Properly structured account | Tax rules vary by type, contribution limits                  |
| CM-021 | Household management   | Group related accounts for unified management              | Account groupings      | Household view              | Tax-loss harvesting across household, wash sale coordination |
| CM-022 | Beneficiary management | Record and update beneficiaries                            | Beneficiary info       | Updated records             | Beneficiary is a minor, trust as beneficiary                 |

#### 1.2.4 Billing and Fee Calculation

| ID     | Requirement     | Description                                            | Inputs                            | Outputs                    | Edge Cases                                        |
| ------ | --------------- | ------------------------------------------------------ | --------------------------------- | -------------------------- | ------------------------------------------------- |
| CM-030 | Fee calculation | Calculate management fees (AUM-based, flat, or tiered) | AUM, fee schedule, billing period | Fee amount                 | Mid-period account opening, AUM changes, fee caps |
| CM-031 | Fee collection  | Debit fees from client accounts                        | Fee amount, account               | Fee deduction confirmation | Insufficient cash, need to liquidate for fees     |
| CM-032 | Fee reporting   | Generate fee disclosures and summaries                 | Fee history                       | Fee report                 | Fee waivers, promotional periods                  |

#### 1.2.5 Client Reporting

| ID     | Requirement         | Description                             | Inputs                            | Outputs               | Edge Cases                                            |
| ------ | ------------------- | --------------------------------------- | --------------------------------- | --------------------- | ----------------------------------------------------- |
| CM-040 | Performance reports | Monthly/quarterly performance summaries | Returns, positions, transactions  | PDF/web report        | New account with < 1 month history                    |
| CM-041 | Holdings reports    | Current position detail                 | Positions, market values          | Holdings report       | Pending settlements, in-transit securities            |
| CM-042 | Tax reports         | Year-end tax summaries (1099 support)   | Transactions, dividends, interest | Tax summary           | Wash sales, cost basis adjustments, corporate actions |
| CM-043 | Client portal       | Self-service web dashboard for clients  | All client data                   | Interactive dashboard | Mobile access, accessibility compliance               |

---

### 1.3 Compliance and Regulatory (Phase 2+)

These are legally required when managing other people's money as a Registered Investment Advisor (RIA).

#### 1.3.1 Regulatory Registration

| ID     | Requirement                   | Description                                                |
| ------ | ----------------------------- | ---------------------------------------------------------- |
| CR-001 | SEC or state registration     | Register as Investment Advisor (Form ADV)                  |
| CR-002 | Form ADV Part 2A (brochure)   | Disclose fees, conflicts, strategies to clients            |
| CR-003 | Form ADV Part 2B (supplement) | Disclose advisor qualifications                            |
| CR-004 | State notice filings          | File in states where clients reside                        |
| CR-005 | Annual ADV amendment          | Update Form ADV annually within 90 days of fiscal year-end |

**Note**: These are human/legal tasks, not software features. The system supports them with data, but a compliance consultant handles the filings.

#### 1.3.2 Compliance Software Features

| ID     | Requirement                    | Description                                          | Inputs                    | Outputs                            | Edge Cases                                             |
| ------ | ------------------------------ | ---------------------------------------------------- | ------------------------- | ---------------------------------- | ------------------------------------------------------ |
| CR-010 | Audit trail                    | Immutable log of all investment decisions and trades | System actions            | Searchable audit log               | Log storage at scale, retention requirements           |
| CR-011 | Trade surveillance             | Monitor for prohibited trading patterns              | Trade history             | Alerts for front-running, churning | False positives, legitimate high-frequency rebalancing |
| CR-012 | Best execution documentation   | Document why each execution venue was chosen         | Order routing decisions   | Best execution records             | Multiple venues, price improvement documentation       |
| CR-013 | Books and records              | Maintain SEC-required records (Rule 204-2)           | All business records      | Organized record system            | 5-year retention, some records 6 years                 |
| CR-014 | Compliance calendar            | Track regulatory deadlines                           | Regulatory schedule       | Alerts, reminders                  | Deadline changes, new requirements                     |
| CR-015 | Client communication archiving | Archive all client communications                    | Emails, messages, reports | Searchable archive                 | Multi-channel communication, attachments               |

---

### 1.4 Data and Analytics

#### 1.4.1 Market Data

| ID     | Requirement           | Description                                   | Inputs                     | Outputs                        | Edge Cases                                       |
| ------ | --------------------- | --------------------------------------------- | -------------------------- | ------------------------------ | ------------------------------------------------ |
| DA-001 | Real-time price feeds | Current prices for portfolio valuation        | Market data subscriptions  | Live prices                    | Exchange outages, stale data, after-hours        |
| DA-002 | Historical price data | Daily OHLCV for backtesting and analysis      | Data provider API          | Historical price database      | Adjusted vs unadjusted prices, survivorship bias |
| DA-003 | Fundamental data      | Earnings, revenue, ratios for stock selection | Data provider API          | Fundamental database           | Restated financials, different fiscal years      |
| DA-004 | Economic indicators   | Macro data for regime detection               | Federal Reserve, BLS feeds | Economic indicator database    | Revisions to prior data, seasonal adjustments    |
| DA-005 | Corporate actions     | Splits, dividends, mergers                    | Corporate action feed      | Adjusted positions and history | Complex mergers, spin-offs, rights offerings     |

#### 1.4.2 Analytics

| ID     | Requirement                   | Description                                  | Inputs                          | Outputs               | Edge Cases                                            |
| ------ | ----------------------------- | -------------------------------------------- | ------------------------------- | --------------------- | ----------------------------------------------------- |
| DA-010 | Portfolio analytics dashboard | Unified view of risk, return, allocation     | All portfolio data              | Interactive dashboard | Multiple accounts, different time zones               |
| DA-011 | Backtesting engine            | Test strategies against historical data      | Strategy rules, historical data | Backtest results      | Look-ahead bias, survivorship bias, transaction costs |
| DA-012 | Scenario analysis             | Stress test portfolio under market scenarios | Positions, scenario definitions | Scenario impact       | Correlation assumptions break in stress               |
| DA-013 | Factor analysis               | Decompose returns into factor exposures      | Returns, factor data            | Factor attribution    | Unstable factor loadings over time                    |

---

### 1.5 Infrastructure

#### 1.5.1 Brokerage Integration

| ID     | Requirement               | Description                                 | Inputs           | Outputs               | Edge Cases                                    |
| ------ | ------------------------- | ------------------------------------------- | ---------------- | --------------------- | --------------------------------------------- |
| IN-001 | Brokerage API client      | Authenticate and communicate with brokerage | API credentials  | Authenticated session | Token expiry, rate limits, API versioning     |
| IN-002 | Account data sync         | Pull positions, balances, transactions      | Account queries  | Synced local data     | Reconciliation failures, pending transactions |
| IN-003 | Order submission          | Place buy/sell orders programmatically      | Order parameters | Order confirmations   | Rejected orders, partial fills                |
| IN-004 | Market data via brokerage | Get real-time and delayed quotes            | Symbol queries   | Price data            | Data quality, gaps, halted securities         |

#### 1.5.2 Security and Access Control

| ID     | Requirement           | Description                                     | Inputs                          | Outputs                 | Edge Cases                       |
| ------ | --------------------- | ----------------------------------------------- | ------------------------------- | ----------------------- | -------------------------------- |
| IN-010 | API key management    | Securely store and rotate brokerage credentials | Encrypted credentials           | Authenticated access    | Key rotation, compromised keys   |
| IN-011 | Encryption at rest    | Encrypt all sensitive data in database          | Data writes                     | Encrypted storage       | Key rotation, backup encryption  |
| IN-012 | Encryption in transit | TLS for all external API calls                  | Outbound requests               | Encrypted communication | Certificate expiry, pinning      |
| IN-013 | Access control        | Role-based access for multi-user scenarios      | User identity, requested action | Allow / deny            | Owner vs advisor vs viewer roles |
| IN-014 | Audit logging         | Log all system access and actions               | System events                   | Security audit log      | Log tampering, storage limits    |

#### 1.5.3 Monitoring and Alerting

| ID     | Requirement              | Description                                         | Inputs              | Outputs                        | Edge Cases                                     |
| ------ | ------------------------ | --------------------------------------------------- | ------------------- | ------------------------------ | ---------------------------------------------- |
| IN-020 | System health monitoring | Track uptime, errors, latency                       | Application metrics | Health dashboard               | Cascading failures, degraded mode              |
| IN-021 | Trade alerting           | Notify on trade execution, failures, risk breaches  | System events       | Notifications (email/SMS/push) | Notification fatigue, delivery failure         |
| IN-022 | Market hours awareness   | Know when markets are open, pre-market, after-hours | Market calendar     | Operating mode                 | Holidays, early closes, circuit breakers       |
| IN-023 | Data quality monitoring  | Detect stale, missing, or anomalous market data     | Incoming data       | Data quality alerts            | Legitimate zero-volume days, halted securities |

---

## 2. Architecture Decision Records

### ADR-001: Monolith vs Microservices

**Status**: Proposed

#### Context

Midas is being built by one person with AI agent assistance. The system needs to handle investment logic, brokerage integration, data ingestion, risk management, and (eventually) client management. The question is whether to build this as a single deployable application or as a collection of independent services.

The operator is a single person with $5M at stake. Operational simplicity is not a nice-to-have -- it is a survival requirement. A system that is hard to deploy, debug, or recover is a system that will lose money when something goes wrong at 2am.

#### Decision

**Modular monolith with clear internal boundaries.**

Build a single deployable application with well-defined internal modules. Each module (investment engine, brokerage client, risk manager, data pipeline) has its own directory, its own interfaces, and no direct cross-module database access. But they deploy as one process and share one database.

The internal module boundaries are designed so that any module _could_ be extracted into a service later, but there is no reason to pay that cost now.

#### Consequences

**Positive**:

- Single deployment: one process to monitor, one log stream, one database backup
- Shared transactions: investment decision + order placement + risk update in one atomic operation
- Simple debugging: one stack trace, one debugger, no distributed tracing needed
- Fast development: no API contracts between services, no service discovery, no retry logic between components
- Lower infrastructure cost: one server, one database, one deployment pipeline

**Negative**:

- Scaling is vertical only (add more CPU/RAM) until a module is extracted
- A bug in one module can crash the entire system (mitigated by process supervision)
- All code deploys together, even for a one-line change in one module

#### Alternatives Considered

**Option A: Microservices**

- Each concern (trading, risk, data, clients) is an independent service
- Rejected because: for a team of one, the operational overhead of managing multiple services, inter-service communication, distributed transactions, and coordinated deployments far outweighs any benefit. Microservices solve a team coordination problem that does not exist here.

**Option B: Serverless / Lambda-based**

- Each function (place order, check risk, ingest data) is a cloud function
- Rejected because: investment logic often requires stateful, time-sensitive operations (monitoring open orders, tracking intraday risk). Cold starts and execution time limits make serverless a poor fit for the core trading loop. Could be used for peripheral tasks (report generation) but not the core engine.

---

### ADR-002: Investment Strategy Approach

**Status**: Proposed

#### Context

The system needs an investment strategy. Options range from fully passive (buy index funds and hold) to fully active (stock picking, market timing). The strategy choice has massive implications for system complexity, data requirements, regulatory risk, and expected outcomes.

Research consistently shows that most active managers underperform passive indexes after fees, especially over long time horizons. But a fully passive approach needs almost no software -- just buy VTI and forget about it. The value of building Midas lies in the space between: systematic, rules-based strategies that add value over pure passive through tax optimization, risk management, and disciplined rebalancing.

#### Decision

**Hybrid: strategic passive core with systematic active overlays.**

- **Core (70-80%)**: Broad market index ETFs (US equity, international equity, bonds, real estate). This captures market returns cheaply and reliably.
- **Tactical overlay (10-20%)**: Rules-based tilts driven by valuation signals, momentum, and economic regime indicators. Not stock picking -- factor-based allocation shifts.
- **Opportunistic sleeve (0-10%)**: Cash held for deployment during severe dislocations (>20% market drawdown). Rules-based entry, not discretionary.
- **Alpha layer**: Tax-loss harvesting, smart rebalancing, and cash management. These are the "free" sources of value that a systematic approach can capture better than a human.

All strategy rules are codified and backtestable. No discretionary calls. The AI agent proposes strategy parameters; the rules engine executes. Human override exists as a circuit breaker, not as a decision-making channel.

#### Consequences

**Positive**:

- Core passive allocation captures market returns with minimal complexity
- Systematic overlays are backtestable and auditable (important for compliance)
- Tax-loss harvesting and smart rebalancing can add 0.5-1.5% annually -- meaningful on $5M
- Rules-based approach is explainable to clients (Phase 2)
- No need for real-time high-frequency data infrastructure

**Negative**:

- Factor tilts may underperform for extended periods (value was dead for a decade)
- System complexity is higher than pure passive
- Requires ongoing factor research and strategy validation
- Clients (Phase 2) may want "exciting" active management

#### Alternatives Considered

**Option A: Pure passive (three-fund portfolio)**

- Buy total market, international, and bonds in fixed proportions
- Rejected because: does not justify building a system. A quarterly calendar reminder to rebalance achieves the same result. The system's value lies in doing more than this.

**Option B: Fully active / quantitative**

- Stock selection, sector rotation, market timing based on ML models
- Rejected because: dramatically increases complexity, data costs, and regulatory scrutiny. Backtesting becomes critical and prone to overfitting. Most quantitative strategies have short half-lives. The risk of underperforming a simple index fund is very high. Not appropriate for a v1 system managing real money.

**Option C: Robo-advisor clone (passive + tax-loss harvesting only)**

- Rejected as too limited -- the tactical overlay and opportunistic sleeve are where the system adds meaningful value. But the core architecture should make it easy to run in "pure passive + TLH" mode if the active components do not justify their complexity.

---

### ADR-003: Brokerage Integration Strategy

**Status**: Proposed

#### Context

The system needs to place trades through a brokerage. Options include integrating with a single broker, supporting multiple brokers, or using an aggregation layer. The choice affects development speed, operational risk, and future product flexibility.

For personal use (Phase 1), only one broker is needed. For managing client money (Phase 2), a custodian relationship is required -- typically through a platform like Schwab Advisor Services, Fidelity Institutional, or Interactive Brokers.

#### Decision

**Single broker for Phase 1 (Interactive Brokers), with an abstraction layer that enables adding brokers later.**

Interactive Brokers (IBKR) is the primary integration target because:

- Comprehensive API (REST + WebSocket)
- Supports individual and advisor accounts (Phase 2 ready)
- Low commissions and good execution quality
- Supports fractional shares for most securities
- Institutional/advisor platform available for managing client assets

The brokerage client is implemented behind an abstract interface (`BrokerageClient` protocol). All investment engine code interacts with this interface, never directly with IBKR-specific code. Adding a second broker means implementing the same interface.

#### Consequences

**Positive**:

- One integration to build, test, and maintain initially
- IBKR's advisor platform supports Phase 2 without switching custodians
- Abstraction layer means the investment engine does not care which broker executes
- IBKR API is well-documented and widely used in systematic trading

**Negative**:

- Single point of failure: if IBKR has an outage, no trading occurs
- IBKR API has quirks (connection management, message-based architecture) that require careful handling
- Vendor lock-in risk (mitigated by abstraction layer)
- IBKR's client-facing UI is notoriously complex (irrelevant for API use but matters if clients see it)

#### Alternatives Considered

**Option A: Alpaca**

- Modern REST API, easier to integrate, commission-free
- Rejected as primary because: Alpaca does not have an established institutional/advisor custody platform. Good for Phase 1 personal use but would require a broker switch for Phase 2. Could be added as a second implementation later.

**Option B: Multi-broker from day one**

- Build integrations for IBKR, Schwab, and Fidelity simultaneously
- Rejected because: triples the integration work with no immediate benefit. The abstraction layer means a second broker can be added in Phase 2 when there is a concrete need.

**Option C: Brokerage aggregator (Plaid, DriveWealth, etc.)**

- Use a middleware layer that abstracts multiple brokers
- Rejected because: adds a dependency and a point of failure between the system and the actual broker. Also limits access to broker-specific features. The abstraction layer within the codebase achieves the same goal without the external dependency.

---

### ADR-004: Client Data Architecture

**Status**: Proposed

#### Context

When the system manages money for multiple clients (Phase 2), each client's data (positions, transactions, personal information) must be isolated. The question is how to structure the database: one shared database with tenant IDs, or separate databases/schemas per client.

#### Decision

**Shared database with tenant isolation at the application layer (single-tenant behavior, multi-tenant storage).**

All client data lives in one database, but every table has a `client_id` column. All queries are scoped by `client_id` at the data access layer -- no query can ever run without a client context. For Phase 1, there is exactly one `client_id` (the owner).

This is implemented using DataFlow's multi-tenancy features, which enforce tenant scoping at the framework level rather than relying on developer discipline.

#### Consequences

**Positive**:

- Simple operations: one database to back up, migrate, and monitor
- Efficient for the expected scale (tens to hundreds of clients, not thousands)
- DataFlow enforces tenant scoping automatically
- Easy to query across clients for aggregate reporting (with proper authorization)
- Natural upgrade path from Phase 1 (one client) to Phase 2 (many clients)

**Negative**:

- A bug that omits the tenant filter could expose cross-client data (mitigated by framework-level enforcement)
- Cannot offer clients their own database for regulatory reasons (unlikely requirement for an RIA)
- Large clients with heavy data volumes share I/O with small clients

#### Alternatives Considered

**Option A: Database per client**

- Each client gets their own database instance
- Rejected because: operational complexity scales linearly with clients. Managing migrations, backups, and monitoring across hundreds of databases is not feasible for a one-person operation.

**Option B: Schema per client (PostgreSQL schemas)**

- Each client gets a schema in one database
- Rejected because: migration complexity (every schema must be migrated individually) with minimal security benefit over application-layer isolation. Also not supported well by most ORMs.

---

### ADR-005: Build vs Buy Decisions

**Status**: Proposed

#### Context

Some capabilities are core to the product's value (worth building), some are commodity (should use existing services), and some are legally required but better outsourced to specialists.

#### Decision

**Build the investment brain. Buy everything else possible.**

| Capability                  | Decision  | Rationale                                                                                                                          |
| --------------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Portfolio optimization**  | BUILD     | Core differentiator. The strategy logic is the product.                                                                            |
| **Order management**        | BUILD     | Tightly coupled to strategy. Must integrate with risk checks.                                                                      |
| **Risk management**         | BUILD     | Core to autonomous operation. Cannot be an afterthought.                                                                           |
| **Rebalancing engine**      | BUILD     | Core strategy execution. Tax-awareness requires custom logic.                                                                      |
| **Tax-loss harvesting**     | BUILD     | Significant value-add. Requires deep integration with portfolio state.                                                             |
| **Performance calculation** | BUILD     | Must be precise and auditable. Standard libraries available.                                                                       |
| **Brokerage API client**    | BUILD     | Must be custom to the abstraction layer. Use existing IBKR client libraries as foundation.                                         |
| **Market data ingestion**   | HYBRID    | Use data provider APIs (Yahoo Finance free tier for Phase 1, paid provider for Phase 2). Build the ingestion pipeline and storage. |
| **KYC/AML**                 | BUY       | Use a service (Alloy, Persona, or similar). Regulatory minefield -- do not build.                                                  |
| **Document storage**        | BUY       | S3 or similar. No value in custom file storage.                                                                                    |
| **Email/notifications**     | BUY       | SendGrid, Twilio, or similar. Commodity service.                                                                                   |
| **Client portal frontend**  | BUILD     | Custom UI needed, but use framework (React/Next.js via Nexus).                                                                     |
| **Authentication**          | BUY       | Auth0 or Clerk. Do not build auth systems.                                                                                         |
| **Compliance filing**       | OUTSOURCE | Use a compliance consultant. Software supports with data, not filing logic.                                                        |
| **Custody**                 | BUY       | Use the brokerage's custody services. Never hold client assets directly.                                                           |
| **PDF report generation**   | HYBRID    | Use a library (WeasyPrint, ReportLab) but build templates and data pipelines.                                                      |
| **Backtesting**             | BUILD     | Core to strategy validation. Must integrate with the same strategy code that runs live.                                            |

#### Consequences

**Positive**:

- Development effort is focused on what creates value
- Commodity services handle their own reliability, security, and compliance
- Faster time to market
- Lower total cost (services are cheaper than building and maintaining)

**Negative**:

- Dependency on external services (mitigated by using abstraction interfaces)
- Monthly costs for services add up
- Less control over non-core components
- Service outages affect operations

---

## 3. MVP Scope Definition

### Phase 1: Personal Use (Manage Own $5M)

**Goal**: A working system that autonomously manages $5M according to a defined strategy, with risk guardrails, tax optimization, and clear performance reporting.

**Timeline**: 2-3 autonomous execution cycles (sessions)

#### Must-Have

| ID     | Feature                                                 | Why It Cannot Wait                              |
| ------ | ------------------------------------------------------- | ----------------------------------------------- |
| P1-001 | IBKR API integration (auth, positions, orders)          | Cannot trade without brokerage connectivity     |
| P1-002 | Portfolio model definition (target allocations)         | System needs to know what to invest in          |
| P1-003 | Rebalancing engine (drift-based + calendar)             | Core autonomous investment behavior             |
| P1-004 | Order generation and submission                         | Must actually place trades                      |
| P1-005 | Pre-trade risk checks (position limits, concentration)  | Cannot deploy real money without safety checks  |
| P1-006 | Drawdown protection (automatic de-risking)              | Critical risk guardrail for $5M                 |
| P1-007 | Position and transaction tracking (local database)      | Must know what is owned and what has happened   |
| P1-008 | Daily portfolio valuation and TWR calculation           | Must measure performance                        |
| P1-009 | Basic alerting (trade execution, errors, risk breaches) | Must know when something goes wrong             |
| P1-010 | Market hours awareness                                  | Must not attempt trades when markets are closed |
| P1-011 | Cash management (maintain target cash buffer)           | Avoid being fully invested with no liquidity    |
| P1-012 | Reconciliation (verify local state matches broker)      | Detect and correct data drift                   |

#### Nice-to-Have (Phase 1)

| ID     | Feature                                        | Value                                                              |
| ------ | ---------------------------------------------- | ------------------------------------------------------------------ |
| P1-N01 | Tax-loss harvesting                            | Adds 0.5-1% annual value but complex                               |
| P1-N02 | Factor-based tactical tilts                    | Adds potential alpha but increases risk                            |
| P1-N03 | Backtesting engine                             | Valuable for strategy validation but not needed to start investing |
| P1-N04 | Interactive dashboard (web UI)                 | Helpful for monitoring but CLI + alerts are sufficient initially   |
| P1-N05 | Opportunistic rebalancing (buy-the-dip sleeve) | Adds value in market dislocations but adds complexity              |

#### Explicitly Out of Scope (Phase 1)

- Client onboarding, KYC/AML, or any multi-user features
- Compliance reporting or regulatory filings
- Client portal or public-facing UI
- Fee calculation or billing
- Options, futures, or derivatives trading
- Cryptocurrency or non-traditional assets
- High-frequency or intraday trading
- Social/copy trading features

#### Key Risks (Phase 1)

| Risk                                              | Probability | Impact               | Mitigation                                                                         |
| ------------------------------------------------- | ----------- | -------------------- | ---------------------------------------------------------------------------------- |
| **Brokerage API error causes unintended trade**   | Medium      | Critical ($$$)       | Pre-trade risk checks, position limits, daily reconciliation, paper trading period |
| **Strategy underperforms passive index**          | Medium      | High                 | Benchmark tracking, automatic fallback to passive if underperforming for N months  |
| **System downtime during market hours**           | Low         | High                 | Process supervision, watchdog, alerts on missed rebalance windows                  |
| **Data quality issue causes bad decision**        | Medium      | High                 | Data validation, staleness checks, fallback to last known good data                |
| **Drawdown protection triggers too aggressively** | Medium      | Medium               | Backtesting the drawdown rules, configurable thresholds, cool-down period          |
| **Tax-loss harvesting creates wash sale**         | Medium      | Medium (tax penalty) | 30-day tracking window, cross-account awareness                                    |

**Critical risk mitigation**: Before deploying with real money, the system MUST run in paper trading mode for at least 30 days. All trade signals are generated and logged but not executed. This validates the strategy, the risk checks, and the operational reliability.

---

### Phase 2: Multi-Client (Manage Money for Others)

**Goal**: Extend Midas to manage investments for other people as a registered investment advisor. This is a legal and business transformation, not just a software update.

**Prerequisites (Non-Software)**:

- Register as an RIA with the SEC or state (if AUM < $100M, state registration; above $100M, SEC)
- Engage a compliance consultant
- Set up an IBKR Advisor account or equivalent institutional custody
- Obtain errors and omissions (E&O) insurance
- Create Form ADV Part 2A (disclosure brochure)

**Timeline**: 3-5 autonomous sessions (after Phase 1 is stable)

#### Must-Have

| ID     | Feature                                                    | Why It Cannot Wait                                             |
| ------ | ---------------------------------------------------------- | -------------------------------------------------------------- |
| P2-001 | Multi-client data isolation (tenant scoping)               | Cannot mix client assets                                       |
| P2-002 | Client onboarding workflow (KYC via third-party service)   | Regulatory requirement                                         |
| P2-003 | Risk profiling and suitability                             | Regulatory requirement -- must document investment suitability |
| P2-004 | Model portfolio assignment per client                      | Different clients need different allocations                   |
| P2-005 | Per-client performance reporting                           | Clients must see their own returns                             |
| P2-006 | Fee calculation and collection                             | Business needs revenue                                         |
| P2-007 | Audit trail (immutable decision log)                       | Regulatory requirement                                         |
| P2-008 | Client communication archiving                             | SEC Rule 204-2 requirement                                     |
| P2-009 | Block trading (aggregate orders across clients)            | Efficiency and best execution                                  |
| P2-010 | Account-level rebalancing (respect individual constraints) | Different clients, different constraints                       |

#### Nice-to-Have (Phase 2)

| ID     | Feature                         | Value                                                         |
| ------ | ------------------------------- | ------------------------------------------------------------- |
| P2-N01 | Client self-service portal      | Reduces support burden but can use periodic reports initially |
| P2-N02 | Automated tax report generation | Valuable at tax time but can be manual for first year         |
| P2-N03 | Household-level management      | Important for married clients but adds complexity             |
| P2-N04 | Automated compliance calendar   | Helpful but a spreadsheet works for < 50 clients              |
| P2-N05 | White-label branding            | Nice for professionalism but not needed to start              |

#### Explicitly Out of Scope (Phase 2)

- Self-service signup (clients onboarded manually/semi-manually)
- Mobile app
- Automated regulatory filing
- Multi-custodian support
- Performance fee structures (stick to flat % of AUM)
- International clients (US only)

#### Key Risks (Phase 2)

| Risk                                  | Probability | Impact                            | Mitigation                                                          |
| ------------------------------------- | ----------- | --------------------------------- | ------------------------------------------------------------------- |
| **Regulatory violation**              | Medium      | Critical (fines, shutdown)        | Compliance consultant, conservative interpretation of rules         |
| **Client data breach**                | Low         | Critical (legal, reputational)    | Encryption, access control, security audit                          |
| **Suitability complaint**             | Medium      | High (lawsuit, regulatory action) | Document everything, risk questionnaire, ongoing suitability review |
| **Fiduciary breach allegation**       | Low         | Critical                          | Audit trail, best execution documentation, conflicts policy         |
| **System makes bad trade for client** | Medium      | High                              | Same risk controls as Phase 1, per-client risk limits               |
| **Fee calculation error**             | Low         | Medium                            | Automated testing, quarterly fee audit                              |

---

### Phase 3: Full Product (Self-Service Platform)

**Goal**: Transform Midas from a managed service (advisor + software) into a self-service platform where clients can sign up, fund accounts, and have their money managed autonomously.

**Prerequisites**: Phase 2 running successfully for 12+ months with growing client base.

**Timeline**: 5-8 autonomous sessions

#### Must-Have

| ID     | Feature                                     | Why It Cannot Wait                              |
| ------ | ------------------------------------------- | ----------------------------------------------- |
| P3-001 | Self-service signup and onboarding          | Core platform requirement                       |
| P3-002 | Automated KYC/AML verification              | Cannot manually verify at scale                 |
| P3-003 | Client web portal (full feature)            | Clients need self-service access                |
| P3-004 | Mobile app (iOS + Android)                  | Market expectation for fintech                  |
| P3-005 | Automated account funding (ACH/wire)        | Clients need to deposit money easily            |
| P3-006 | Automated reporting and statements          | Cannot manually generate at scale               |
| P3-007 | Multi-custodian support                     | Different clients may need different custodians |
| P3-008 | Scalable infrastructure (not single-server) | Must handle hundreds/thousands of accounts      |
| P3-009 | SOC 2 Type II compliance                    | Enterprise and institutional clients require it |
| P3-010 | Disaster recovery and business continuity   | Regulatory and operational requirement at scale |

#### Explicitly Out of Scope (Phase 3)

- International markets (Phase 4 if ever)
- Cryptocurrency integration
- Social trading / copy trading
- Lending / margin products
- Banking services

#### Key Risks (Phase 3)

| Risk                                              | Probability | Impact   | Mitigation                                             |
| ------------------------------------------------- | ----------- | -------- | ------------------------------------------------------ |
| **Scaling too fast without operational maturity** | High        | Critical | Controlled growth, waiting lists, manual approval      |
| **Regulatory scrutiny increases with AUM**        | High        | High     | Proactive compliance, regulatory counsel on retainer   |
| **Competition from established robo-advisors**    | High        | Medium   | Differentiate on strategy transparency and performance |
| **Technology reliability at scale**               | Medium      | High     | Redundancy, monitoring, incident response procedures   |

---

## 4. Technical Stack Mapping to Kailash SDK

### 4.1 Framework Component Mapping

| Midas Capability                                           | Kailash Framework         | How It Maps                                                                              |
| ---------------------------------------------------------- | ------------------------- | ---------------------------------------------------------------------------------------- |
| **Investment workflow orchestration**                      | Core SDK                  | Each investment process (rebalance, harvest, onboard) is a Kailash workflow with nodes   |
| **Market data ingestion pipeline**                         | Core SDK                  | Data ingestion as scheduled workflows: fetch -> validate -> store                        |
| **Portfolio optimization**                                 | Core SDK + PythonCodeNode | Optimization math runs in PythonCodeNode; workflow orchestrates inputs/outputs           |
| **Order management**                                       | Core SDK                  | Order lifecycle as a workflow: generate -> risk check -> submit -> monitor -> reconcile  |
| **Risk monitoring**                                        | Core SDK + Kaizen         | Continuous risk agent monitors portfolio state, triggers defensive workflows             |
| **Database operations (positions, transactions, clients)** | DataFlow                  | All persistent data through DataFlow models and auto-generated nodes                     |
| **API for client portal**                                  | Nexus                     | REST API exposing portfolio data, performance, reports                                   |
| **CLI for admin operations**                               | Nexus                     | Admin CLI for manual overrides, diagnostics, system control                              |
| **Autonomous investment agent**                            | Kaizen                    | The "brain" -- an AI agent that decides when and how to rebalance, harvest, etc.         |
| **Strategy research agent**                                | Kaizen                    | An agent that analyzes market conditions and suggests strategy adjustments               |
| **Risk governance**                                        | PACT                      | Financial constraints, trading limits, and approval workflows governed by PACT envelopes |
| **Audit trail**                                            | PACT + DataFlow           | Every decision logged through PACT governance records and DataFlow audit tables          |
| **Multi-tenant data**                                      | DataFlow (multi-tenancy)  | Client isolation enforced at the DataFlow layer                                          |

### 4.2 Core Workflow Designs

#### Rebalance Workflow

```
[MarketDataFetch] -> [PortfolioValuation] -> [DriftCalculation] -> [ShouldRebalance?]
                                                                        |
                                                              Yes       |       No
                                                               v                v
                                                    [OptimizeTargets]     [LogSkip]
                                                           |
                                                    [GenerateOrders]
                                                           |
                                                    [PreTradeRiskCheck]
                                                           |
                                                  Pass     |     Fail
                                                   v              v
                                            [SubmitOrders]  [AlertAndLog]
                                                   |
                                            [MonitorFills]
                                                   |
                                            [Reconcile]
                                                   |
                                            [UpdatePerformance]
```

#### Tax-Loss Harvesting Workflow

```
[ScanUnrealizedLosses] -> [FilterByThreshold] -> [CheckWashSaleWindow]
                                                        |
                                                  Clear  |  Blocked
                                                   v           v
                                          [FindSubstitute]  [LogBlocked]
                                                   |
                                          [GenerateHarvestTrades]
                                                   |
                                          [PreTradeRiskCheck]
                                                   |
                                          [ExecuteHarvest]
                                                   |
                                          [TrackWashSaleWindow]
```

#### Daily Operations Workflow

```
[CheckMarketStatus] -> [SyncBrokerageData] -> [ReconcilePositions]
                                                      |
                                              [CalculateDailyReturns]
                                                      |
                                              [RunRiskChecks]
                                                      |
                                    Issues Found      |      All Clear
                                         v                      v
                                  [AlertAndEscalate]     [CheckRebalanceTriggers]
                                                                |
                                                      Triggered |   Not Triggered
                                                         v              v
                                                  [RunRebalance]  [LogDailyStatus]
```

### 4.3 Kaizen Agent Architecture

The autonomous investment system is best modeled as a multi-agent system using Kaizen:

| Agent                       | Role                                                                                                               | PACT Governance                                                                                       |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| **Portfolio Manager Agent** | Top-level coordinator. Decides when to rebalance, harvest losses, adjust strategy. Delegates to specialist agents. | Full trading authority within risk envelope. Max single-trade size. Max daily trade volume.           |
| **Risk Agent**              | Monitors portfolio risk continuously. Can veto trades and trigger defensive actions. Has override authority.       | Read-only on trading. Write authority on risk alerts. Can trigger circuit breaker (halt all trading). |
| **Data Agent**              | Manages market data ingestion, validation, and storage. Feeds clean data to other agents.                          | No trading authority. Read/write on data stores only.                                                 |
| **Execution Agent**         | Handles order submission, monitoring, and reconciliation. Interacts with brokerage API.                            | Trading authority for approved orders only. Cannot generate its own orders.                           |
| **Tax Agent**               | Scans for harvesting opportunities, manages wash sale windows.                                                     | Can propose trades (harvesting pairs) but cannot execute without Portfolio Manager approval.          |
| **Reporting Agent**         | Generates performance reports, client statements, dashboards.                                                      | Read-only on all data. Write authority on report output only.                                         |

### 4.4 PACT Governance for Financial Safety

PACT governance is critical for Midas. The investment system manages real money, and governance envelopes enforce hard limits on what agents can do:

```
Organization: Midas
  Department: Trading
    Role: PortfolioManager
      Envelope:
        financial:
          max_single_trade: $50,000 (1% of $5M)
          max_daily_volume: $250,000 (5% of $5M)
          max_position_size: 10% of portfolio
        operational:
          allowed_tools: [place_order, cancel_order, get_positions, get_quotes]
          max_actions_per_hour: 50
        temporal:
          trading_hours_only: true

    Role: RiskMonitor
      Envelope:
        financial:
          max_single_trade: $0 (cannot trade)
        operational:
          allowed_tools: [get_positions, get_quotes, trigger_circuit_breaker, send_alert]
          max_actions_per_hour: 1000 (monitoring is high-frequency)

    Role: Executor
      Envelope:
        financial:
          max_single_trade: $50,000 (inherited from parent)
        operational:
          allowed_tools: [place_order, cancel_order, get_order_status]
          requires_approval_from: PortfolioManager
```

**Monotonic tightening** ensures that even if the AI agents evolve their strategies, they cannot exceed the hard limits set in the governance envelopes. A rogue agent cannot increase its own trading limits -- that requires a human changing the envelope definition.

### 4.5 DataFlow Schema (Phase 1 Core)

```python
# Core models for DataFlow

class Security:
    # Investable security master
    symbol: str          # Ticker symbol (primary key)
    name: str            # Full name
    asset_class: str     # equity, fixed_income, reit, commodity, cash
    sector: str          # GICS sector
    exchange: str        # NYSE, NASDAQ, etc.
    is_active: bool      # Currently tradeable

class Position:
    # Current portfolio holdings
    account_id: str      # Account identifier
    client_id: str       # Tenant isolation
    symbol: str          # Security symbol
    quantity: float      # Shares held
    cost_basis: float    # Total cost basis
    acquired_date: date  # For tax lot tracking

class Transaction:
    # Trade history
    id: str              # Unique transaction ID
    client_id: str       # Tenant isolation
    account_id: str
    symbol: str
    side: str            # buy, sell
    quantity: float
    price: float
    commission: float
    executed_at: datetime
    order_type: str      # market, limit
    source: str          # rebalance, harvest, cash_management

class PortfolioModel:
    # Target allocation templates
    model_id: str
    name: str
    version: int
    allocations: dict    # {symbol: weight}
    rebalance_threshold: float  # Drift % to trigger rebalance
    created_at: datetime

class DailyValuation:
    # Daily portfolio snapshots
    client_id: str
    account_id: str
    valuation_date: date
    total_value: float
    cash_value: float
    invested_value: float
    daily_return: float
    cumulative_return: float

class RiskEvent:
    # Risk alerts and actions
    id: str
    client_id: str
    event_type: str      # drawdown, concentration, drift, circuit_breaker
    severity: str        # info, warning, critical
    details: dict
    action_taken: str
    created_at: datetime

class WashSaleWindow:
    # Tax lot tracking for wash sale prevention
    client_id: str
    symbol: str
    sale_date: date
    expiry_date: date    # sale_date + 30 days
    quantity: float
    loss_amount: float
```

### 4.6 Nexus API Design (Phase 2)

```
# Client-facing REST API (via Nexus)

GET  /api/v1/portfolio              # Current holdings and allocation
GET  /api/v1/portfolio/performance   # Returns for various periods
GET  /api/v1/portfolio/transactions  # Transaction history
GET  /api/v1/portfolio/risk          # Current risk metrics
GET  /api/v1/reports                 # Available reports
GET  /api/v1/reports/{id}/download   # Download specific report

# Admin API (authenticated, role-restricted)
POST /api/v1/admin/rebalance         # Trigger manual rebalance
POST /api/v1/admin/circuit-breaker   # Emergency halt trading
GET  /api/v1/admin/system/health     # System health check
GET  /api/v1/admin/audit-log         # Audit trail access
```

---

## 5. Non-Functional Requirements

### 5.1 Performance

| Metric                      | Target                          | Rationale                                              |
| --------------------------- | ------------------------------- | ------------------------------------------------------ |
| Order generation latency    | < 5 seconds                     | Markets move; slow order generation means worse prices |
| Risk check latency          | < 1 second                      | Must not delay order submission meaningfully           |
| Daily valuation calculation | < 30 seconds (100 accounts)     | Should complete before market open                     |
| Market data refresh         | < 10 seconds from source update | Stale data leads to bad decisions                      |
| API response time           | < 500ms (p95)                   | Client portal must feel responsive                     |
| Portfolio optimization      | < 30 seconds                    | Runs infrequently but must not block the pipeline      |

### 5.2 Reliability

| Metric                            | Target                            | Rationale                                     |
| --------------------------------- | --------------------------------- | --------------------------------------------- |
| System uptime during market hours | 99.9% (< 8.7 hours downtime/year) | Missing a trading day has real cost           |
| Data pipeline reliability         | 99.95%                            | Missing market data can cause bad decisions   |
| Order submission success rate     | 99.5% (of valid orders)           | Failed orders delay rebalancing               |
| Reconciliation accuracy           | 100% match or alert               | Data discrepancies must be caught immediately |

### 5.3 Security

| Requirement                | Standard                     | Implementation                                              |
| -------------------------- | ---------------------------- | ----------------------------------------------------------- |
| Data encryption at rest    | AES-256                      | PostgreSQL transparent data encryption or application-level |
| Data encryption in transit | TLS 1.3                      | All API calls, brokerage connections, client portal         |
| Authentication             | OAuth 2.0 / JWT              | Auth0 for client portal, API keys for admin                 |
| Authorization              | RBAC via PACT                | Owner, advisor, client, viewer roles                        |
| Secrets management         | Env vars / vault             | Never in code, never in logs                                |
| Audit logging              | Immutable append-only        | Every action logged with actor, timestamp, details          |
| PII handling               | Encrypted, access-controlled | Client SSN, DOB, address encrypted separately               |

### 5.4 Scalability

| Phase   | Scale                                | Architecture                                 |
| ------- | ------------------------------------ | -------------------------------------------- |
| Phase 1 | 1 account, ~50 positions             | Single server, SQLite                        |
| Phase 2 | 10-100 accounts, ~5000 positions     | Single server, PostgreSQL                    |
| Phase 3 | 100-1000 accounts, ~50,000 positions | Load-balanced, PostgreSQL with read replicas |

---

## 6. User Journey Maps

### 6.1 Owner Journey (Phase 1)

```
1. Initial Setup
   - Install Midas
   - Configure IBKR API credentials
   - Define portfolio model (target allocations)
   - Set risk parameters (max drawdown, position limits)
   - Fund account

2. Paper Trading Period (30 days)
   - System generates trade signals daily
   - All signals logged but NOT executed
   - Owner reviews signals and risk metrics
   - Adjusts strategy parameters if needed
   - Validates system behavior against expectations

3. Go Live
   - Enable live trading
   - System executes first rebalance (buy into target positions)
   - Owner monitors via alerts and dashboard
   - System handles daily operations autonomously

4. Ongoing
   - Daily: system syncs data, calculates returns, checks risk
   - Weekly/monthly: system evaluates rebalancing triggers
   - Quarterly: owner reviews performance vs benchmark
   - Annually: owner reviews strategy, tax situation

5. Intervention Points (when owner MUST act)
   - Circuit breaker triggered (system halts, owner decides)
   - Risk check failure with no automated resolution
   - Brokerage credential rotation
   - Strategy parameter updates
```

### 6.2 Client Journey (Phase 2)

```
1. Onboarding
   - Advisor (owner) initiates client onboarding
   - Client completes KYC through secure link
   - Client completes risk questionnaire
   - System assigns model portfolio based on risk score
   - Advisor reviews and approves suitability
   - Account opened at custodian

2. Funding
   - Client transfers funds to custodian account
   - System detects new cash via daily sync
   - System invests according to assigned model
   - Client receives confirmation

3. Ongoing Management
   - System manages portfolio per model
   - Client receives monthly/quarterly reports
   - Client can view holdings through portal (Phase 2 nice-to-have)
   - Advisor reviews client portfolios quarterly

4. Life Events
   - Client requests withdrawal -> system raises cash
   - Client risk profile changes -> advisor reassigns model
   - Client adds funds -> system invests per model
```

---

## 7. Implementation Roadmap

### Phase 1: Foundation (2-3 autonomous sessions)

**Session 1: Core Infrastructure**

- IBKR API client implementation (behind BrokerageClient interface)
- DataFlow schema: Security, Position, Transaction, DailyValuation
- Market data ingestion workflow (daily prices)
- Position sync and reconciliation
- Market hours calendar

**Session 2: Investment Engine**

- Portfolio model definition and storage
- Portfolio valuation calculation
- Drift calculation and rebalance trigger logic
- Order generation (target vs current weights)
- Pre-trade risk checks
- Order submission and fill monitoring
- Cash management logic

**Session 3: Risk and Operations**

- Drawdown monitoring and protection
- Concentration monitoring
- Daily operations workflow (full pipeline)
- Performance calculation (TWR)
- Alerting system (email/SMS on critical events)
- Paper trading mode
- CLI for manual operations

### Phase 2: Multi-Client (3-5 autonomous sessions)

**Session 4: Multi-Tenancy and Client Management**

- DataFlow multi-tenancy activation
- Client data models (profile, risk score, accounts)
- Onboarding workflow (KYC integration)
- Risk questionnaire and suitability engine
- Per-client model portfolio assignment

**Session 5: Compliance and Reporting**

- Audit trail implementation
- Block trading (aggregate orders across clients)
- Per-client performance reporting
- Fee calculation and collection
- Client communication archiving

**Session 6-7: Client Portal**

- Nexus API deployment
- Client authentication (Auth0 integration)
- Performance dashboard
- Holdings view
- Transaction history
- Report download

### Phase 3: Product (5-8 autonomous sessions)

- Self-service signup
- Automated KYC/AML
- Account funding automation
- Mobile app
- Multi-custodian support
- Advanced reporting
- Infrastructure hardening (redundancy, DR)

---

## 8. Success Criteria

### Phase 1 (Personal Use)

- [ ] System successfully executes a rebalance trade in paper trading mode
- [ ] System correctly calculates portfolio TWR matching manual calculation
- [ ] Pre-trade risk checks correctly block oversized orders
- [ ] Drawdown protection triggers at configured threshold
- [ ] Daily reconciliation matches IBKR positions exactly
- [ ] System operates autonomously for 30 days in paper trading without intervention
- [ ] System operates autonomously for 90 days live with returns within 2% of target benchmark
- [ ] Tax-loss harvesting identifies and executes at least one valid harvest

### Phase 2 (Multi-Client)

- [ ] Client data is provably isolated (no cross-client data leakage)
- [ ] Onboarding workflow completes end-to-end for a test client
- [ ] Per-client performance reports are accurate and generated on schedule
- [ ] Fee calculation matches manual calculation to the penny
- [ ] Audit trail captures every investment decision with rationale
- [ ] Block trades are allocated fairly across clients

### Phase 3 (Product)

- [ ] Self-service signup completes in under 10 minutes
- [ ] System handles 100+ accounts without performance degradation
- [ ] 99.9% uptime during market hours over 90 days
- [ ] SOC 2 Type II audit passed

---

## 9. Open Questions Requiring Human Decision

These are decisions that cannot be made by analysis alone -- they require the owner's input:

1. **Initial investment strategy**: What target allocation do you want? (e.g., 60% US equity / 20% international / 15% bonds / 5% real estate?) Or do you want the system to recommend one?

2. **Risk tolerance**: What is your maximum acceptable drawdown? 10%? 20%? This determines how aggressively the drawdown protection triggers.

3. **Rebalancing frequency**: How often should the system check for drift? Daily with a 5% threshold is standard, but some prefer weekly or monthly.

4. **Paper trading period**: Are you comfortable with 30 days of paper trading before going live? Or do you want longer/shorter?

5. **Phase 2 timing**: Do you want to start pursuing RIA registration now (it takes 3-6 months) while Phase 1 is being built? Or wait until Phase 1 is proven?

6. **Fee structure for clients**: What fee will you charge? Industry standard for robo-advisors is 0.25-0.50% of AUM annually. Traditional advisors charge 0.75-1.25%.

7. **Minimum investment for clients**: What is the minimum account size you will accept? $10,000? $100,000? This affects your target market.

8. **IBKR account**: Do you already have an Interactive Brokers account? If not, do you have a preference for a different broker?
