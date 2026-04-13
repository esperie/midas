# Investment Strategy Space: What Autonomous Systems Can Deploy

## Executive Summary

This document maps the investment strategy landscape that Midas's autonomous system could employ, assessed through three lenses: theoretical foundation, empirical evidence, and suitability for autonomous execution. The honest conclusion is that AI's strongest contributions to investment management are in portfolio construction, risk management, tax optimization, and operational execution -- not in consistently generating alpha through price prediction. Midas should build its core value proposition around these proven strengths, with speculative alpha-generation strategies as a supplementary (and honestly disclosed) component.

---

## 1. Passive / Index Strategies

### 1.1 Modern Portfolio Theory (MPT) Based Allocation

**Foundation**: Markowitz Mean-Variance Optimization (1952). For a given level of expected return, there exists a portfolio with minimum variance (risk). The set of all such portfolios forms the "efficient frontier."

**How it works in practice**:

1. Define asset classes (US large cap, international, bonds, REITs, commodities, etc.)
2. Estimate expected returns, volatilities, and correlations for each asset class
3. Optimize allocation weights to maximize risk-adjusted return (Sharpe ratio) given constraints
4. Rebalance periodically to maintain target allocation

**What robo-advisors do**: All major robo-advisors (Wealthfront, Betterment, Vanguard) use variants of MPT. They typically use:

- Capital Asset Pricing Model (CAPM) or Black-Litterman model for expected returns
- Historical correlations with adjustments
- ETFs as building blocks
- Client risk tolerance maps to a point on the efficient frontier

**Suitability for Midas**:

- Excellent as the foundation layer -- every portfolio needs a core allocation
- Well-understood, regulatory-friendly, defensible
- Can be automated completely
- This alone does not differentiate Midas from existing robo-advisors

**Risks and limitations**:

- Garbage in, garbage out: expected return estimates are notoriously unstable
- Historical correlations break down in crises (correlation goes to 1)
- MPT assumes normal distributions; real returns have fat tails
- Rebalancing frequency matters but optimal frequency is debated

### 1.2 Factor Investing

**Foundation**: Fama-French Three-Factor Model (1992) and subsequent extensions. Returns are explained by systematic risk factors beyond market beta.

**Established factors with robust academic evidence**:

| Factor         | Description                               | Expected Premium     | Evidence Strength        |
| -------------- | ----------------------------------------- | -------------------- | ------------------------ |
| Market (Beta)  | Broad market exposure                     | ~6-8%/year over cash | Very strong              |
| Size (SMB)     | Small caps outperform large caps          | ~2-3%/year           | Strong, debated recently |
| Value (HML)    | Cheap stocks outperform expensive         | ~3-5%/year           | Strong, cyclical         |
| Momentum       | Recent winners continue winning           | ~5-8%/year           | Strong, but crashes hard |
| Quality        | Profitable, stable companies              | ~3-5%/year           | Strong                   |
| Low Volatility | Low-vol stocks outperform (risk-adjusted) | ~2-4%/year           | Moderate-strong          |

**Implementation**:

- Smart beta ETFs (iShares, Vanguard, DFA) provide factor exposure cheaply
- Direct indexing allows custom factor tilts within a tax-efficient framework
- Multi-factor portfolios blend exposure to capture diversified factor premiums

**Suitability for Midas**:

- Excellent as a strategy layer on top of MPT-based core allocation
- Can be automated: scoring stocks on factors, constructing factor-tilted portfolios
- Academically defensible and regulatory-friendly
- Factor timing (rotating between factors) is much harder and less evidence-supported

### 1.3 Tax-Loss Harvesting (TLH)

**What it is**: Selling positions at a loss to realize capital losses that offset capital gains, then reinvesting in a similar (but not "substantially identical") security to maintain market exposure.

**How much it adds**: Studies suggest TLH adds 0.5-1.5% per year in after-tax returns, depending on market volatility, portfolio turnover, and the investor's tax situation. Wealthfront claims up to 2%+ in some years.

**Automation advantage**: This is one of the clearest areas where automation beats humans. Daily TLH scanning across hundreds of individual positions (direct indexing) captures far more tax losses than quarterly human review.

**Implementation details**:

- Must avoid wash sale violations (30-day rule: can't buy "substantially identical" security within 30 days)
- Must track cost basis across all accounts (including spouse accounts, IRAs)
- Must select replacement securities that maintain portfolio factor exposure
- Most effective in taxable accounts (not applicable to IRAs)
- Diminishing returns over time as cost basis adjusts down

**Suitability for Midas**:

- Very high value-add for taxable accounts
- Fully automatable and algorithmically superior to human execution
- One of Midas's strongest potential differentiators
- Pairs perfectly with direct indexing (owning individual stocks instead of ETFs)

### 1.4 Direct Indexing

**What it is**: Instead of buying an ETF (e.g., SPY for S&P 500), own the individual stocks that comprise the index. This enables:

1. Per-stock tax-loss harvesting (harvest losses on individual stocks while maintaining index exposure)
2. ESG/values-based customization (exclude certain companies or industries)
3. Factor tilts at the individual stock level
4. Concentrated stock management (integrate existing holdings)

**Market context**: Direct indexing AUM has grown rapidly -- estimated $500B+ in 2025. Wealthfront, Betterment, and Vanguard all now offer direct indexing at $100K+ minimums.

**Suitability for Midas**:

- Highly suitable for $5M portfolio and high-net-worth clients
- Maximizes tax-loss harvesting opportunities
- Requires sophisticated rebalancing algorithms (managing 300-500 individual positions)
- Computationally intensive but perfectly suited for autonomous execution

---

## 2. Active / Quantitative Strategies

### 2.1 Momentum

**Foundation**: Jegadeesh and Titman (1993). Securities that have performed well in the recent past (3-12 months) tend to continue performing well in the near future, and vice versa.

**Evidence**: One of the most robust anomalies in finance. Documented across equities, bonds, currencies, and commodities. Persists across countries and time periods.

**Implementation**:

- Cross-sectional momentum: Rank stocks by recent returns; go long winners, short losers
- Time-series momentum (trend following): Go long assets with positive recent returns, short assets with negative returns
- Typical lookback: 12 months return, excluding the most recent month

**Risks**:

- Momentum crashes: Historically, momentum strategies experience sudden, severe drawdowns (e.g., 2009 momentum crash: -40% in 2 months)
- Crowding: As more investors use momentum strategies, the premium may compress
- Transaction costs: High turnover (monthly rebalancing typical)
- Tax inefficient: High short-term capital gains

**Suitability for Midas**:

- Good as one component in a multi-strategy portfolio
- Must be combined with crash protection (volatility scaling, stop-losses)
- Best implemented with a long bias (130/30 or long-only tilts) rather than pure long/short
- Automatable

### 2.2 Mean Reversion

**Foundation**: Prices tend to revert to their historical mean or fundamental value. Overreactions create opportunities.

**Types**:

- **Statistical arbitrage (pairs trading)**: Identify correlated stocks; when the spread widens, bet on convergence. Classic strategy; well-documented but crowded.
- **Oversold/overbought**: Buy after extreme short-term declines (1-5 days); sell after extreme short-term gains. Short-term mean reversion is well-documented.
- **Fundamental mean reversion**: Buy stocks trading far below intrinsic value (deep value). Longer horizon (1-3 years).

**Evidence**: Short-term (days to weeks) mean reversion is robust. Long-term (3-5 year) mean reversion exists but is slow. At intermediate horizons (1-12 months), momentum dominates.

**Suitability for Midas**:

- Pairs trading / statistical arbitrage can be automated but requires careful execution and risk management
- Short-term mean reversion strategies have high turnover and are best for tax-advantaged accounts
- Complements momentum (they tend to be negatively correlated, providing diversification)

### 2.3 Statistical Arbitrage

**What it is**: Broader category of market-neutral strategies that exploit pricing inefficiencies identified through statistical methods.

**Common approaches**:

- Pairs trading (mentioned above)
- Basket trading: Long one basket of stocks, short another, based on statistical relationships
- Principal component analysis (PCA): Identify and trade residual returns after removing common factors
- Cross-asset arbitrage: Exploit mispricings between related instruments (e.g., stock vs. convertible bond)

**Challenges for Midas**:

- These strategies are dominated by well-capitalized hedge funds with faster execution
- Alpha decays quickly as more participants discover and exploit the same patterns
- Requires sophisticated infrastructure (low-latency execution, robust risk management)
- May not be suitable at Midas's initial scale ($5M)

### 2.4 Event-Driven Strategies

**Types**:

- **Earnings momentum**: Trade around earnings announcements based on analyst estimate revisions, surprise predictions, or post-earnings drift
- **Merger arbitrage**: After an acquisition announcement, buy the target (trading below offer price due to deal risk), short the acquirer
- **Index rebalancing**: Trade ahead of or after index reconstitution events (stocks added/removed from indexes)
- **Corporate actions**: Spinoffs, special dividends, share buybacks

**Suitability for Midas**:

- Earnings momentum is highly automatable using NLP on earnings transcripts and analyst reports
- Merger arbitrage requires understanding of deal specifics -- harder to fully automate
- Index rebalancing is crowded but still captures some premium
- These strategies complement factor-based approaches

---

## 3. AI/ML Approaches to Investing

### 3.1 What Works (Evidence-Supported)

#### 3.1.1 Risk Forecasting

- **Evidence**: ML models (random forests, gradient boosting, neural networks) significantly outperform traditional models (GARCH) at forecasting volatility and drawdown risk
- **Application**: Portfolio risk management, dynamic asset allocation, tail risk hedging
- **Why it works**: Risk clustering is more persistent and learnable than returns. Volatility has long memory and regime structure.
- **Midas application**: Core risk management layer. Use ML to forecast portfolio risk and adjust positions accordingly.

#### 3.1.2 Portfolio Optimization

- **Evidence**: ML-enhanced optimization (regularized covariance matrices, Bayesian approaches, hierarchical risk parity) produces more robust portfolios than traditional MPT
- **Application**: Constructing diversified portfolios that are more robust out-of-sample
- **Notable methods**:
  - Hierarchical Risk Parity (HRP, Lopez de Prado 2016): Clusters assets by correlation structure, allocates inversely to risk within clusters
  - Regularized covariance estimation: Shrinkage, factor models, or random matrix theory to produce stable covariance matrices
  - Bayesian Black-Litterman: Combines market equilibrium with quantitative views
- **Midas application**: Core portfolio construction. Replace naive MPT with ML-enhanced optimization.

#### 3.1.3 Natural Language Processing (NLP) for Information Synthesis

- **Evidence**: NLP models can extract sentiment, topics, and forward-looking statements from earnings calls, analyst reports, news, and regulatory filings. Modest but statistically significant predictive power for short-term returns (1-5 day horizon), particularly around earnings events.
- **Application**: Earnings surprise prediction, event detection, sentiment monitoring
- **Midas application**: Information synthesis layer. Not for direct trading signals, but for enriching the decision-making context. LLMs can summarize analyst consensus, identify material changes in company guidance, and flag risk events.

#### 3.1.4 Anomaly Detection

- **Evidence**: ML excels at detecting unusual patterns in market data, order flow, and corporate behavior
- **Application**: Detecting regime changes, identifying unusual trading activity, flagging potential risks
- **Midas application**: Risk management and compliance monitoring

### 3.2 What Doesn't Work (or Works Poorly)

#### 3.2.1 Price Prediction

- **Evidence**: Despite hundreds of papers claiming ML can predict stock prices, the out-of-sample evidence is weak. Key issues:
  - Signal-to-noise ratio in financial data is extremely low (~0.5% for daily returns)
  - Non-stationarity: Relationships change over time, causing model decay
  - Overfitting: ML models with many parameters easily overfit to noise in financial data
  - Transaction costs: Even small predictive advantages may not survive trading costs
  - Crowding: Widely published signals get arbitraged away
- **Honest assessment**: No publicly available ML model has demonstrated consistent, significant alpha generation after transaction costs across multiple market regimes.
- **Midas implication**: Do not promise AI-generated alpha from price prediction. Use ML for risk management and portfolio optimization instead.

#### 3.2.2 Deep Learning for Time Series

- **Evidence**: LSTMs, transformers, and other deep learning architectures have been applied to financial time series. Results:
  - Marginal improvement over simpler models (often not statistically significant)
  - Severe overfitting risk (financial data sets are small relative to model complexity)
  - Poor regime adaptability (models trained on bull markets fail in bear markets)
  - Computational cost high relative to alpha generated
- **Midas implication**: Deep learning may add value for feature extraction (e.g., processing alternative data) but is not reliable for return prediction.

#### 3.2.3 Reinforcement Learning for Trading

- **Evidence**: RL agents can learn trading policies in simulation. However:
  - Simulation-to-reality gap: Market simulators don't capture market impact, liquidity dynamics, or counterparty behavior
  - Non-stationarity: The optimal policy changes as markets change
  - Sample efficiency: RL requires millions of experiences; markets provide limited data
  - Risk of catastrophic failure: RL agents can learn strategies that work well on average but blow up in extreme scenarios
- **Midas implication**: RL is not ready for live autonomous trading. It may be useful for order execution optimization (minimizing market impact) as a narrow application.

### 3.3 Honest AI Strategy Framework for Midas

| AI Application            | Confidence Level | Value Add            | Priority for Midas |
| ------------------------- | ---------------- | -------------------- | ------------------ |
| Risk forecasting          | High             | High                 | Core               |
| Portfolio optimization    | High             | High                 | Core               |
| Tax optimization          | High             | High                 | Core               |
| NLP information synthesis | Moderate         | Moderate             | Secondary          |
| Anomaly/regime detection  | Moderate         | Moderate             | Secondary          |
| Factor timing             | Low-Moderate     | Moderate if it works | Experimental       |
| Return prediction         | Low              | Low after costs      | Experimental only  |
| RL for execution          | Low-Moderate     | Low-Moderate         | Future research    |

---

## 4. Risk Management

### 4.1 Risk Management Framework

Risk management is arguably more important than return generation. A system that preserves capital during downturns and manages drawdowns will outperform a system that generates high returns but experiences catastrophic losses.

**Midas risk management layers**:

#### Layer 1: Strategic Asset Allocation

- Broad diversification across asset classes (equities, bonds, alternatives)
- Based on client's risk tolerance and investment horizon
- Long-term target allocation

#### Layer 2: Tactical Adjustments

- Dynamic allocation shifts based on market conditions
- Risk-off signals: reduce equity exposure during elevated risk periods
- Volatility targeting: scale position sizes inversely with realized volatility
- Maximum: +/- 10-20% from strategic allocation

#### Layer 3: Position-Level Controls

- Maximum single position size (e.g., 5% of portfolio)
- Sector concentration limits (e.g., 25% in any single sector)
- Correlation monitoring (avoid hidden concentration)
- Stop-losses at the position level (e.g., 20% decline from peak)

#### Layer 4: Portfolio-Level Controls

- Maximum portfolio drawdown limit (e.g., 15-20% from peak)
- If drawdown limit is reached, reduce risk systematically
- Portfolio-level stop-loss triggers automatic de-risking
- Daily P&L monitoring

#### Layer 5: System-Level Controls (Kill Switches)

- Maximum daily loss limit
- Maximum number of trades per day
- Anomaly detection (if the system behaves unexpectedly, halt trading)
- Manual override capability (human can pause or stop the system)
- Circuit breakers tied to market conditions (e.g., halt trading if VIX > 40)

### 4.2 Drawdown Management

**Why drawdowns matter more than returns**:

- A 50% loss requires a 100% gain to recover
- A 33% loss requires a 50% gain to recover
- Clients' psychological tolerance for drawdowns is much lower than they claim in questionnaires
- Regulatory risk: large drawdowns attract regulatory scrutiny for autonomous systems

**Drawdown management approaches**:

- **Constant Proportion Portfolio Insurance (CPPI)**: Maintain a floor value; increase risky asset exposure as portfolio grows above floor; decrease as it approaches floor
- **Volatility targeting**: Adjust position sizes so portfolio volatility stays near target (e.g., 10% annualized). Reduces exposure in high-volatility regimes.
- **Trend following**: As a defensive overlay, reduce exposure when broad market trends turn negative
- **Options hedging**: Buy protective puts or put spreads on portfolio. Cost: 1-2% per year, but provides hard floor on losses.

### 4.3 Tail Risk Hedging

**The problem**: Standard diversification fails in crises. Correlations spike; all risk assets decline simultaneously.

**Approaches**:

- **Tail risk funds/strategies**: Dedicate 2-5% of portfolio to convex strategies that profit from extreme market declines. Cost: negative carry (drags returns in normal markets).
- **Put options on indices**: Buy out-of-the-money puts (e.g., 20% OTM S&P 500 puts). Expensive; need to balance cost vs. protection.
- **Managed futures (trend following)**: Historically performs well during equity bear markets. Not a perfect hedge but provides crisis alpha.
- **Treasury bonds**: Long-duration US Treasuries have historically risen during equity crises. However, 2022 showed this correlation can break (stocks and bonds both declined).

**Recommendation for Midas**: Implement volatility targeting as the primary risk management approach. Add options-based tail risk hedging for large portfolios where the cost is justified. Maintain a "risk budget" framework where total portfolio risk is constrained rather than return targets.

---

## 5. Asset Classes

### 5.1 Core Asset Classes for Midas

| Asset Class             | Role in Portfolio                | Implementation               | Expected Return  | Risk Level    |
| ----------------------- | -------------------------------- | ---------------------------- | ---------------- | ------------- |
| US Large Cap Equities   | Core growth                      | SPY, IVV, or direct indexing | 7-10%/year       | Moderate-High |
| US Small Cap Equities   | Growth + size factor             | IWM, SCHA, or direct         | 8-12%/year       | High          |
| International Developed | Diversification                  | EFA, VEA                     | 6-9%/year        | Moderate-High |
| Emerging Markets        | Growth + diversification         | EEM, VWO                     | 8-12%/year       | High          |
| US Aggregate Bonds      | Stability, income                | AGG, BND                     | 3-5%/year        | Low           |
| TIPS                    | Inflation protection             | TIP, SCHP                    | 2-4%/year (real) | Low-Moderate  |
| REITs                   | Real assets, income              | VNQ, SCHH                    | 6-9%/year        | Moderate-High |
| Commodities             | Inflation hedge, diversification | DJP, GSG                     | 3-6%/year        | Moderate-High |

### 5.2 Supplementary Asset Classes

| Asset Class       | Role                                | Implementation                     | Notes                                    |
| ----------------- | ----------------------------------- | ---------------------------------- | ---------------------------------------- |
| Options           | Hedging, income, leveraged exposure | Direct trading (IBKR)              | Requires options-specific strategies     |
| Crypto (BTC, ETH) | Speculative, alternative            | Spot or ETFs (IBIT, ETHA)          | High volatility; small allocation (1-5%) |
| Private Credit    | Income, diversification             | Interval funds, private placements | Illiquid; accredited investors           |
| Gold              | Safe haven                          | GLD, IAU                           | Historically uncorrelated with equities  |

### 5.3 Asset Allocation Models

**Conservative (for risk-averse clients)**:

- 30% Equities / 50% Bonds / 10% REITs / 10% Alternatives

**Moderate (balanced)**:

- 50% Equities / 30% Bonds / 10% REITs / 10% Alternatives

**Aggressive (for growth-oriented clients)**:

- 75% Equities / 10% Bonds / 5% REITs / 10% Alternatives

**Midas's own $5M (assuming moderate-aggressive risk tolerance)**:

- 55% Equities (direct indexing + factor tilts)
- 15% Bonds (duration-managed, TIPS included)
- 10% REITs
- 10% Alternatives (managed futures, commodities)
- 5% Tactical (momentum, event-driven)
- 5% Crypto/Speculative

---

## 6. Strategy Implementation Priorities for Midas

### Phase 1: Foundation (Months 1-6)

**Core strategies (proven, high confidence)**:

1. MPT-based strategic asset allocation with Black-Litterman views
2. Tax-loss harvesting across direct-indexed positions
3. Volatility-targeted risk management
4. Automated rebalancing with threshold-based triggers
5. Multi-asset class diversification via ETFs and direct indexing

**Expected value-add over simple indexing**: 1-3% per year (primarily from tax optimization and risk management)

### Phase 2: Enhancement (Months 6-12)

**Secondary strategies (moderate confidence)**:

1. Factor investing (value, momentum, quality) with systematic exposure management
2. ML-enhanced portfolio optimization (HRP, regularized covariance)
3. NLP-based information monitoring (earnings, news, filings)
4. Dynamic asset allocation based on regime detection

**Expected additional value-add**: 0.5-2% per year (before fees, with wide confidence interval)

### Phase 3: Alpha Research (Months 12+)

**Experimental strategies (lower confidence, honest about uncertainty)**:

1. Cross-asset momentum
2. Earnings momentum / post-earnings drift
3. Options-based strategies (covered calls for income, protective puts for hedging)
4. Alternative data signals

**Expected additional value-add**: Uncertain. Must be tracked with rigorous out-of-sample testing and honestly reported.

---

## 7. Performance Measurement and Benchmarking

### 7.1 Benchmark Selection

| Client Profile  | Primary Benchmark                                                       | Secondary Benchmark             |
| --------------- | ----------------------------------------------------------------------- | ------------------------------- |
| Conservative    | 30/70 Equity/Bond blend (e.g., 30% MSCI ACWI / 70% Bloomberg Aggregate) | Inflation + 2%                  |
| Moderate        | 60/40 blend                                                             | Inflation + 4%                  |
| Aggressive      | 80/20 blend                                                             | MSCI ACWI                       |
| Midas's own $5M | 60/40 or custom multi-asset benchmark                                   | Risk-adjusted (Sharpe, Sortino) |

### 7.2 Performance Metrics

- **Total return**: Gross and net of fees
- **Sharpe ratio**: Return per unit of total risk
- **Sortino ratio**: Return per unit of downside risk
- **Maximum drawdown**: Largest peak-to-trough decline
- **Calmar ratio**: Return / maximum drawdown
- **Information ratio**: Active return / tracking error (vs. benchmark)
- **Alpha**: Return in excess of risk-adjusted benchmark
- **Beta**: Sensitivity to market movements
- **Tax alpha**: After-tax return advantage from tax optimization

### 7.3 Reporting Standards

- **GIPS (Global Investment Performance Standards)**: Voluntary standards for ethical performance reporting. Recommended for institutional credibility.
- **Time-weighted return (TWR)**: Standard for portfolio performance; eliminates impact of cash flows
- **Money-weighted return (IRR)**: Reflects actual investor experience including timing of cash flows
- Report BOTH TWR and MWR/IRR

---

## Sources and References

- Markowitz, H. "Portfolio Selection." Journal of Finance, 1952.
- Fama, E. and French, K. "The Cross-Section of Expected Stock Returns." Journal of Finance, 1992.
- Jegadeesh, N. and Titman, S. "Returns to Buying Winners and Selling Losers." Journal of Finance, 1993.
- Carhart, M. "On Persistence in Mutual Fund Performance." Journal of Finance, 1997.
- Asness, C., Moskowitz, T., and Pedersen, L. "Value and Momentum Everywhere." Journal of Finance, 2013.
- Lopez de Prado, M. "Building Diversified Portfolios that Outperform Out-of-Sample." Journal of Portfolio Management, 2016.
- Gu, S., Kelly, B., and Xiu, D. "Empirical Asset Pricing via Machine Learning." Review of Financial Studies, 2020.
- Israel, R., Kelly, B., and Moskowitz, T. "Can Machines Learn Finance?" Journal of Investment Management, 2020.
- Harvey, C., Liu, Y., and Zhu, H. "...and the Cross-Section of Expected Returns." Review of Financial Studies, 2016. (Discusses replication crisis in factor research)
- Arnott, R., Harvey, C., Kalesnik, V., and Linnainmaa, J. "Reports of Value's Death May Be Greatly Exaggerated." Financial Analysts Journal, 2021.
- AQR Capital Management: Factor investing research papers (aqr.com/insights)
- Wealthfront Research: Tax-Loss Harvesting white paper (2024)
- CFA Institute Research Foundation: "AI and Machine Learning in Asset Management" (2024)
