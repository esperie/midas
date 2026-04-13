# Trading Infrastructure: Brokerage APIs, Market Data, and Execution

## Executive Summary

Midas's trading infrastructure must support two distinct modes: (1) personal portfolio management of $5M across multiple asset classes with institutional-quality execution, and (2) multi-client management with trade allocation, compliance controls, and custodial separation. The infrastructure stack consists of four layers: brokerage APIs for execution, market data providers for decision inputs, custodial arrangements for client asset safety, and execution quality systems for regulatory compliance. This document maps the available options for each layer.

---

## 1. Brokerage APIs

### 1.1 Alpaca

**Overview**: API-first, commission-free brokerage purpose-built for developers and fintech applications.

**Two distinct products**:

#### Alpaca Trading API (for Midas's own account)

- **Asset classes**: US equities (NYSE, NASDAQ), ETFs, crypto (BTC, ETH, and others)
- **Pricing**: Commission-free equities/ETFs; crypto fees vary
- **Order types**: Market, limit, stop, stop-limit, trailing stop
- **Extended hours**: Pre-market (4:00 AM) and after-hours (8:00 PM ET)
- **Fractional shares**: Yes (down to 0.001 shares)
- **Paper trading**: Full sandbox environment with same API
- **API style**: REST + WebSocket (streaming)
- **Rate limits**: 200 requests/minute (per API key)
- **Account types**: Individual, business, IRA
- **Margin**: 2:1 (standard), 4:1 (day trading)
- **SIPC**: Yes ($500K protection, $250K cash)

**API capabilities**:

```
REST Endpoints:
- /v2/orders          - Order placement, modification, cancellation
- /v2/positions       - Current holdings, P&L
- /v2/account         - Account balance, equity, buying power
- /v2/assets          - Asset metadata, tradability
- /v2/clock           - Market open/close times
- /v2/calendar        - Trading calendar
- /v2/watchlists      - Watchlist management
- /v2/portfolio/history - Historical portfolio performance

WebSocket Streams:
- Trade updates       - Real-time order fills and status changes
- Account updates     - Balance and position changes
```

#### Alpaca Broker API (for multi-client productization)

- **What it is**: White-label brokerage infrastructure; Midas can open, fund, and trade accounts for clients
- **KYC/AML**: Alpaca handles identity verification, OFAC screening
- **Account types**: Individual, joint, trust, IRA, entity
- **Custody**: Alpaca is the custodian (SIPC-insured)
- **Clearing**: Self-clearing or Velox clearing
- **Fee model**: Revenue share on payment for order flow (PFOF) and interest on uninvested cash; or custom pricing
- **Compliance**: Alpaca handles regulatory reporting, 1099s, corporate actions
- **Key advantage**: Midas does NOT need to register as a broker-dealer; Midas registers as an RIA and uses Alpaca's infrastructure

**Broker API endpoints**:

```
- /v1/accounts          - Create/manage client accounts
- /v1/accounts/{id}/documents - KYC document uploads
- /v1/funding           - ACH/wire transfers
- /v1/accounts/{id}/orders   - Trade on behalf of clients
- /v1/accounts/{id}/positions - Client holdings
- /v1/accounts/{id}/activities - Transaction history
```

**Limitations**:

- No options trading
- No futures
- No international markets
- Limited fixed income
- Relatively young firm (founded 2015)
- API uptime has had occasional issues during high-volume periods

**Verdict for Midas**: Strong candidate for multi-client productization (Broker API). Adequate for personal US equities/ETFs. Insufficient alone for $5M diversified portfolio (no options, futures, or international).

### 1.2 Interactive Brokers (IBKR)

**Overview**: Most comprehensive electronic brokerage; institutional-grade execution; global market access.

**API options**:

#### TWS API (Trader Workstation)

- **Languages**: Python, Java, C++, C#
- **Connection**: Requires TWS or IB Gateway running locally or on a server
- **Capabilities**: Full trading, market data, account info, portfolio analytics
- **Limitations**: Requires persistent connection to TWS/Gateway; connection can drop; complex API design

#### Client Portal API (REST)

- **Style**: REST API (more modern than TWS API)
- **Auth**: OAuth-based authentication
- **Capabilities**: Trading, account info, market data (limited)
- **Limitations**: Newer and less feature-complete than TWS API; some advanced order types not supported

#### FIX / CTCI (Institutional)

- **For**: High-volume institutional trading
- **Capabilities**: Direct market access, FIX 4.2/4.4
- **Requirements**: Separate agreement, volume minimums

**Asset classes**:

- Stocks (150+ markets, 33 countries)
- Options (US and international)
- Futures and futures options
- Forex (100+ currency pairs)
- Bonds (corporate, government, municipal)
- Mutual funds
- ETFs
- CFDs (non-US)
- Crypto (limited)
- Metals, warrants, structured products

**Pricing**:

- Tiered or fixed commission structures
- IBKR Lite: $0 commission on US stocks/ETFs (PFOF model)
- IBKR Pro: $0.0035/share (min $0.35) for US stocks; $0.65/contract for options
- Very competitive margin rates (currently ~5.5-6.5% depending on size)

**Advisor/Institutional accounts**:

- **Master/Sub-account structure**: One master account with multiple client sub-accounts
- **Allocation methods**: Pre-trade allocation (specify before order) or post-trade allocation
- **Allocation algorithms**: ProRata, Available Equity, Equal Quantity, Custom
- **Fee billing**: Can bill advisory fees directly from client accounts
- **Client onboarding**: Clients open accounts directly with IBKR; link to advisor master account
- **Reporting**: Client-facing reports, performance attribution
- **Model portfolios**: Define target portfolios, auto-rebalance across accounts

**Limitations**:

- TWS API requires running TWS/Gateway continuously (operational burden)
- API documentation is comprehensive but poorly organized
- Error handling is inconsistent across API versions
- REST API is still catching up to TWS API in feature parity
- Client onboarding UX is notoriously complex
- Support quality is variable

**Verdict for Midas**: Best choice for personal $5M portfolio (broadest asset class coverage, best execution, lowest costs). Strong choice for multi-client management via advisor accounts. API complexity is the main drawback.

### 1.3 Charles Schwab / TD Ameritrade API

**Overview**: Schwab acquired TD Ameritrade (completed 2024). The former TD Ameritrade API (thinkorswim) was one of the best retail trading APIs. The transition to Schwab's API platform is ongoing.

**Current state (2025-2026)**:

- TD Ameritrade API officially deprecated; accounts migrated to Schwab
- Schwab Trader API launched in 2024, gradually expanding capabilities
- Schwab's API is less mature than TD's was

**Schwab Trader API**:

- **Asset classes**: US equities, ETFs, options, mutual funds, fixed income
- **Order types**: Market, limit, stop, stop-limit, trailing stop, conditional orders
- **Auth**: OAuth 2.0
- **Rate limits**: Vary by endpoint (generally 120 requests/minute)
- **Account types**: Individual, joint, IRA, trust, custodial
- **Paper trading**: Not yet available via API (as of 2025)

**Schwab Advisor Services** (for multi-client):

- Custodian for RIAs (one of the "Big Three" alongside Fidelity and Pershing)
- API access for advisor accounts
- Trade allocation and rebalancing tools
- Client onboarding
- Reporting and billing
- $10M+ AUM typically needed for full Schwab Advisor Services relationship

**Limitations**:

- API transition from TD to Schwab has been rocky
- Feature parity with TD API not yet achieved
- Documentation is in flux
- Crypto not supported
- International markets not supported via API

**Verdict for Midas**: Schwab is a top-tier custodian for multi-client management (Schwab Advisor Services). API is currently in transition and less reliable than IBKR or Alpaca. Consider for custodial relationship at scale, but not for primary API integration today.

### 1.4 Tradier

**Overview**: Developer-focused brokerage API with clean REST design; much smaller than Alpaca or IBKR.

**Key features**:

- **Asset classes**: US equities, ETFs, options
- **Pricing**: $0 commissions (equities); $0.35/contract (options)
- **API style**: REST, clean documentation
- **Sandbox**: Full paper trading environment
- **Rate limits**: Generous (6 requests/second sustained)
- **Market data**: Built-in streaming and historical data
- **Auth**: OAuth 2.0

**Strengths**: Very clean API design; good for options trading (which Alpaca lacks); competitive pricing
**Weaknesses**: Small firm; limited to US markets; no futures, forex, or crypto; no advisor/multi-client infrastructure

**Verdict for Midas**: Could supplement Alpaca for options trading. Not a primary infrastructure choice.

### 1.5 Brokerage Comparison Matrix

| Feature           | Alpaca        | IBKR               | Schwab           | Tradier     |
| ----------------- | ------------- | ------------------ | ---------------- | ----------- |
| US Equities/ETFs  | Yes           | Yes                | Yes              | Yes         |
| Options           | No            | Yes                | Yes              | Yes         |
| Futures           | No            | Yes                | No (API)         | No          |
| Forex             | No            | Yes                | No (API)         | No          |
| International     | No            | Yes (150+ markets) | No (API)         | No          |
| Crypto            | Yes (limited) | Limited            | No               | No          |
| Fixed Income      | No            | Yes                | Yes              | No          |
| Commission        | $0            | Low (tiered)       | $0 (equities)    | $0 equities |
| API Quality       | Excellent     | Complex but deep   | Transitioning    | Clean       |
| Multi-Client      | Broker API    | Advisor accounts   | Advisor Services | No          |
| Paper Trading     | Yes           | Yes                | No (API)         | Yes         |
| Fractional Shares | Yes           | Yes                | Yes              | No          |

### 1.6 Recommended Brokerage Architecture

**For Midas's own $5M**:

- **Primary**: Interactive Brokers (broadest asset access, best execution, institutional quality)
- **Secondary**: Alpaca (for US equity strategies requiring commission-free execution and cleaner API)

**For multi-client productization**:

- **Primary option A**: Alpaca Broker API (fastest path to market; Alpaca handles BD infrastructure)
- **Primary option B**: IBKR Advisor accounts (more asset classes; more established)
- **At scale**: Schwab Advisor Services or Fidelity Institutional as custodian

---

## 2. Market Data Providers

### 2.1 Real-Time Market Data

#### Polygon.io

- **Coverage**: US stocks, options, forex, crypto
- **Pricing**: Free tier (15-min delay); Starter $29/mo (5 calls/min); Developer $79/mo; Business $199/mo; Enterprise custom
- **Data types**: Trades, quotes, NBBO, aggregates, snapshots
- **Delivery**: REST + WebSocket
- **Historical**: Tick-level data back to 2004 (stocks), 2007 (options)
- **Strengths**: Comprehensive US equity data; real-time streaming; good documentation; reasonable pricing
- **Weaknesses**: US-only for equities; options data expensive at lower tiers; rate limits on free/starter tiers

#### Alpha Vantage

- **Coverage**: US/international stocks, forex, crypto, commodities, economic indicators
- **Pricing**: Free (25 requests/day); Premium from $49.99/mo
- **Data types**: OHLCV, technical indicators (50+ built-in), fundamental data
- **Delivery**: REST (JSON/CSV)
- **Strengths**: Free tier is generous for development; 50+ built-in technical indicators; fundamental data included
- **Weaknesses**: No real-time streaming; rate limits are restrictive; data quality inconsistencies reported; not suitable for low-latency trading

#### IEX Cloud (now owned by Nasdaq)

- **Coverage**: US stocks, ETFs
- **Pricing**: Free tier (limited); Launch $19/mo; Grow $49/mo; Scale $199/mo
- **Data types**: Real-time quotes, historical prices, fundamentals, news, social sentiment
- **Delivery**: REST + SSE (Server-Sent Events)
- **Strengths**: Clean API; affordable for small-scale use; fundamentals data included
- **Weaknesses**: Limited to US equities; Nasdaq acquisition has caused pricing/product changes; not suitable for institutional use

#### Bloomberg Terminal API (BLPAPI)

- **Coverage**: Global -- all asset classes, all markets
- **Pricing**: Bloomberg Terminal subscription ($2,000/month per seat)
- **Data types**: Everything -- real-time, historical, fundamentals, analytics, news, research, reference data
- **Delivery**: Proprietary API (BLPAPI); Python SDK available
- **Strengths**: Gold standard for financial data; unmatched breadth and quality; used by all institutional investors
- **Weaknesses**: Extremely expensive; terminal subscription required; API is complex; licensing restrictions on data redistribution

#### Refinitiv (LSEG Data & Analytics)

- **Coverage**: Global equities, fixed income, derivatives, commodities, forex
- **Pricing**: Custom enterprise pricing (comparable to Bloomberg for full suite)
- **Delivery**: REST API (Refinitiv Data Platform); Eikon API; Elektron (real-time)
- **Strengths**: Comprehensive global coverage; strong fixed income and derivatives data; AI-ready data feeds
- **Weaknesses**: Expensive; complex product suite; LSEG acquisition has caused transitions

### 2.2 Alternative Data

Alternative data sources that are increasingly important for AI-driven investment strategies:

- **News and sentiment**: RavenPack, Alexandria Technology, GDELT (free)
- **Satellite imagery**: Orbital Insight, Planet Labs -- for commodity tracking, retail foot traffic
- **Social media sentiment**: StockTwits API, Reddit API (limited), Twitter/X API
- **SEC filings**: SEC EDGAR (free); parsed by providers like Calcbench, Last10K
- **Insider trading data**: SEC Forms 3, 4, 5 via EDGAR; OpenInsider (free)
- **Options flow**: Unusual Whales, FlowAlgo -- institutional options activity
- **Economic data**: FRED (Federal Reserve, free); BLS, Census Bureau
- **Earnings transcripts**: Seeking Alpha, FactSet, S&P Capital IQ
- **Web scraping**: Various, with legal considerations (CFAA, terms of service)

### 2.3 Market Data Comparison

| Provider      | Coverage                       | Real-Time | Historical | Price       | Best For                   |
| ------------- | ------------------------------ | --------- | ---------- | ----------- | -------------------------- |
| Polygon.io    | US stocks, options, FX, crypto | Yes       | 2004+      | $29-$199/mo | Primary US equity data     |
| Alpha Vantage | Global stocks, FX, crypto      | Delayed   | Yes        | Free-$50/mo | Development, indicators    |
| IEX Cloud     | US stocks, ETFs                | Yes       | Limited    | $19-$199/mo | Supplementary data         |
| Bloomberg     | Global, all assets             | Yes       | Extensive  | ~$2K/mo     | Institutional, multi-asset |
| FRED          | Economic data                  | N/A       | Extensive  | Free        | Macroeconomic indicators   |

### 2.4 Recommended Data Architecture

**Tier 1 (MVP / Personal use)**:

- Polygon.io (Developer plan, $79/mo) for real-time US equity/options data
- FRED for economic indicators (free)
- SEC EDGAR for filings (free)
- Alpha Vantage free tier for supplementary international data

**Tier 2 (Production / Multi-client)**:

- Polygon.io (Business plan, $199/mo) or equivalent
- Multiple alternative data sources based on strategy requirements
- Consider Bloomberg for multi-asset coverage if AUM justifies cost

**Tier 3 (Institutional scale)**:

- Bloomberg or Refinitiv terminal + API for comprehensive global coverage
- Premium alternative data subscriptions
- Dedicated data infrastructure with real-time processing

---

## 3. Custodian Requirements

### 3.1 Why Custody Matters

When managing other people's money, the central question is: where does the money physically sit? Regulators require client assets to be held by a "qualified custodian" -- a regulated entity separate from the investment adviser.

**Qualified custodians include**:

- Banks (state or national)
- Registered broker-dealers
- Futures commission merchants
- Foreign financial institutions (with conditions)

**Why separation matters**:

- Prevents adviser from misappropriating client funds (Madoff scenario)
- Clients receive independent statements from the custodian
- If the adviser goes bankrupt, client assets are protected
- Regulators can examine custodial records independently

### 3.2 Custodial Models

#### Separately Managed Accounts (SMAs)

- Each client has their own brokerage account at the custodian
- Midas has discretionary trading authority over each account
- Client can see their holdings at any time via custodian's portal
- Client receives statements directly from custodian
- **Advantages**: Transparency, tax control, portability, regulatory simplicity
- **Disadvantages**: Operational complexity at scale, minimum account sizes, trade allocation overhead

#### Omnibus Account

- All client assets pooled in one account in the adviser's name
- Internal sub-accounting tracks each client's share
- **Advantages**: Operational simplicity, easier trade execution
- **Disadvantages**: Less transparent, higher custodial risk, regulatory scrutiny, requires surprise examinations

#### Fund Structure (LP/LLC)

- Client invests in a fund entity; fund holds assets at custodian
- Midas is the fund manager
- **Advantages**: Simpler administration, performance fees standard, single entity to manage
- **Disadvantages**: Less client control, higher setup costs, fund audit required

**Recommendation for Midas**: Start with separately managed accounts (SMAs) via Alpaca Broker API or IBKR Advisor accounts. This is the standard model for RIAs and provides the most transparency and regulatory simplicity. Consider fund structure at larger scale for high-net-worth / institutional clients.

### 3.3 Custodian Options for RIAs

| Custodian               | Minimum AUM | Strengths                                        | API           | Fee Model         |
| ----------------------- | ----------- | ------------------------------------------------ | ------------- | ----------------- |
| Schwab Advisor Services | ~$10M       | Largest RIA custodian; institutional credibility | Yes           | Varies            |
| Fidelity Institutional  | ~$10M       | Comprehensive platform; strong technology        | Yes           | Varies            |
| Pershing (BNY Mellon)   | ~$50M       | Institutional-grade; custody specialists         | Limited       | Custody fees      |
| Alpaca (Broker API)     | $0          | API-first; fast onboarding; developer-friendly   | Excellent     | Revenue share     |
| Interactive Brokers     | $0          | Global markets; lowest costs; advisor accounts   | Yes (complex) | Transaction-based |

### 3.4 Custodial Architecture Decision

**Phase 1 (Personal)**: IBKR for own $5M account (no custodial issues -- it's your money)

**Phase 2 (First clients)**: Alpaca Broker API as custodian

- Fastest path to market
- No custodian minimum AUM
- Alpaca handles all account opening, KYC, compliance reporting
- Limitation: US equities/ETFs and crypto only

**Phase 3 (Scale)**: Add Schwab Advisor Services or Fidelity Institutional

- Required for institutional credibility at $50M+ AUM
- More asset class coverage
- Better client perception
- Higher operational requirements

---

## 4. Execution Quality

### 4.1 Best Execution Obligation

As an RIA, Midas has a fiduciary duty to seek "best execution" for client trades. This does not mean the lowest commission -- it means the best overall result considering:

- Price (most important factor)
- Speed of execution
- Likelihood of execution (fill rate)
- Size of order
- Commission costs
- Market impact

### 4.2 Execution Quality Metrics

**Metrics Midas must track**:

1. **Effective spread**: Difference between execution price and midpoint at time of order
2. **Price improvement**: How often clients get a better price than the NBBO (National Best Bid and Offer)
3. **Fill rate**: Percentage of orders that get fully filled
4. **Time to fill**: How quickly orders are executed
5. **Market impact**: Price movement caused by the order itself
6. **Implementation shortfall**: Difference between decision price and actual execution price

### 4.3 Order Routing Considerations

**Payment for Order Flow (PFOF)**:

- Alpaca and other commission-free brokers route orders to market makers (Citadel, Virtu) for PFOF
- Market makers may provide price improvement but also profit from the order flow
- SEC has proposed rules restricting PFOF (still pending as of 2026)
- For Midas's $5M account, PFOF impact is minimal on typical order sizes
- For large orders, PFOF routing may not achieve best execution

**Direct Market Access (DMA)**:

- Available through IBKR Pro
- Orders go directly to exchanges
- Better execution quality for larger orders
- Higher commissions

**Smart Order Routing (SOR)**:

- IBKR's SmartRouting analyzes all available venues and routes to best price
- Most sophisticated retail SOR available

### 4.4 Execution for Midas's Architecture

**For the $5M personal portfolio**:

- Use IBKR Pro with SmartRouting for best execution
- For large positions (>$50K), use limit orders and consider VWAP/TWAP algorithms
- IBKR provides algorithmic order types: VWAP, TWAP, Adaptive, Accumulate/Distribute

**For multi-client management**:

- Block trading: Place one order for aggregate amount, allocate pro-rata to client accounts
- Pre-trade allocation: Specify allocation before order execution
- Fair allocation: Must ensure all clients receive fair execution (no cherry-picking good fills)
- Trade rotation: Rotate which clients get allocated first to prevent bias

### 4.5 Algorithmic Order Types Available

| Order Type    | Description                            | Use Case                    | Available On      |
| ------------- | -------------------------------------- | --------------------------- | ----------------- |
| VWAP          | Volume-Weighted Average Price          | Large orders over a period  | IBKR              |
| TWAP          | Time-Weighted Average Price            | Distribute evenly over time | IBKR              |
| Adaptive      | Adjusts aggressiveness based on market | Smart execution             | IBKR              |
| Iceberg       | Show only partial size                 | Hide large order size       | IBKR, some others |
| Trailing Stop | Dynamic stop price                     | Risk management             | All brokers       |
| Bracket       | Entry + stop loss + take profit        | Complete trade management   | IBKR, Alpaca      |

---

## 5. Infrastructure Architecture Summary

### 5.1 Recommended Stack

```
Layer 1: Market Data
  Primary:   Polygon.io (US equities, options)
  Secondary: FRED (economics), SEC EDGAR (filings)
  Future:    Bloomberg (global, multi-asset)

Layer 2: Decision Engine (Midas Core)
  Strategy generation, backtesting, risk management
  AI/ML models for signal generation
  Portfolio optimization
  Pre-trade compliance checks

Layer 3: Execution
  Personal:  IBKR TWS API (all asset classes)
  Clients:   Alpaca Broker API (US equities, fast onboarding)
  At scale:  IBKR Advisor + Schwab Advisor Services

Layer 4: Custody
  Personal:  IBKR (self-custodied at broker)
  Clients:   Alpaca (custodian via Broker API) or IBKR
  At scale:  Schwab/Fidelity Institutional

Layer 5: Compliance & Reporting
  Pre-trade: Custom compliance engine
  Post-trade: Execution quality monitoring
  Reporting: Client performance reports, regulatory filings
  Audit trail: Every decision logged and traceable
```

### 5.2 Cost Estimates

| Component              | Personal ($5M)         | Multi-Client (MVP)                      | At Scale ($100M+) |
| ---------------------- | ---------------------- | --------------------------------------- | ----------------- |
| Market Data            | $80-200/mo             | $200-500/mo                             | $2K-5K/mo         |
| Brokerage              | $0-50/mo (commissions) | Alpaca revenue share                    | Negotiated        |
| Compliance             | $0                     | $50-100K/yr (outsourced)                | $200K+/yr         |
| Infrastructure (cloud) | $100-300/mo            | $500-2K/mo                              | $5K-20K/mo        |
| **Total**              | **~$500/mo**           | **~$5-10K/mo + $50-100K/yr compliance** | **$30K+/mo**      |

---

## Sources and References

- Alpaca API documentation (api.alpaca.markets, 2025)
- Interactive Brokers API documentation (interactivebrokers.github.io, 2025)
- Schwab Developer Portal (developer.schwab.com, 2025)
- Tradier API documentation (documentation.tradier.com, 2025)
- Polygon.io API documentation and pricing (polygon.io, 2025)
- Alpha Vantage documentation (alphavantage.co, 2025)
- SEC: Commission Guidance Regarding Clients' Commission Payments (2006)
- SEC: Staff Guidance on Robo-Advisers (IM Guidance Update 2017-02)
- FINRA: Regulatory Notice 15-09 (Equity Trading Initiatives: Best Execution)
- "Best Execution and Payment for Order Flow" -- CFA Institute Research Foundation (2024)
- IBKR Institutional account documentation (2025)
- Schwab Advisor Services documentation (2025)
