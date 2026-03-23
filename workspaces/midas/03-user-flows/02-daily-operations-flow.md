# Daily Operations Flow: What Midas Does Every Day (Without You)

**Who this is for**: You, the owner, understanding what your system is doing while you go about your life.

**The core idea**: Midas operates on a daily cycle. It does its work around market hours, and you receive a brief summary each evening. On most days, you do not need to do anything.

---

## The Daily Cycle

Here is what happens every trading day. Weekends and market holidays are quiet -- Midas does nothing and you hear nothing.

---

### Before Market Open (6:00 AM - 9:30 AM Eastern)

**What Midas does (you do not see this happening):**

1. **Checks that everything is working.** Midas verifies it can connect to your brokerage account, that market data is flowing, and that there are no system errors from overnight.

2. **Syncs your account.** It pulls your latest account balances and positions from Interactive Brokers to make sure its records match reality. If there is a mismatch (for example, a dividend was deposited overnight), it updates its records.

3. **Checks the market landscape.** It reviews overnight market moves (international markets, futures), any major economic data releases scheduled for today, and whether any of the companies in your portfolio have news (earnings reports, corporate actions like stock splits).

4. **Decides if any action is needed today.** Based on how far your portfolio has drifted from its target allocations, whether there are tax-loss harvesting opportunities, and whether the market environment warrants any tactical adjustments, Midas decides its plan for the day. Most days, the answer is "no action needed."

5. **Runs safety checks.** Before the market opens, Midas confirms all safety boundaries are in place and the risk management system is active.

**What you see:** Nothing, unless there is a problem. If something is wrong (brokerage connection failed, data feed is down), you get an alert.

---

### During Market Hours (9:30 AM - 4:00 PM Eastern)

**What Midas does:**

1. **Monitors your portfolio value in real time.** It tracks how your investments are performing throughout the day.

2. **Executes planned trades.** If Midas decided this morning that rebalancing is needed, it places the trades during market hours. It uses limit orders (setting a maximum price to pay or minimum price to accept) rather than market orders, to get better prices. It avoids trading in the first 15 minutes after market open and the last 15 minutes before close, when prices tend to be volatile.

3. **Watches for risk events.** If the market drops sharply, Midas monitors whether your drawdown protection threshold is being approached. It tracks your single-day loss against your alert threshold.

4. **Looks for tax-loss harvesting opportunities.** If tax-loss harvesting is enabled, Midas scans for positions with meaningful unrealized losses that could be sold to offset gains.

5. **Tracks open orders.** If trades were placed, Midas monitors whether they filled, partially filled, or need to be adjusted.

**What you see:** Nothing, unless something notable happens. You CAN open the dashboard anytime and see live portfolio values, but you do not need to.

**If a trade happens**, you might get a brief notification (depending on your alert preferences):

> "Trade executed: Rebalanced $15,000 from US Stocks to International to maintain target allocation."

**If the market drops significantly** (hitting your daily alert threshold):

> "Market alert: Your portfolio is down 3.2% ($160,000) today. The broad market (S&P 500) is down 3.5%. No safety boundaries have been triggered. Midas is monitoring the situation."

---

### After Market Close (4:00 PM - 6:00 PM Eastern)

**What Midas does:**

1. **Final reconciliation.** It pulls the end-of-day positions and balances from the brokerage and verifies everything matches its records.

2. **Calculates daily performance.** It computes your portfolio's return for the day, compares it to benchmarks, and updates the running total for the month, quarter, and year.

3. **Records everything.** Every decision made, every trade placed, every risk check performed -- all logged in an audit trail.

4. **Prepares your daily summary.** Midas generates a brief evening summary of the day.

---

### Your Evening Summary (sent around 5:00 PM Eastern)

This is the one thing you receive every trading day. It is designed to take less than 30 seconds to read.

**On a quiet day (most days):**

> **Midas Daily Summary -- Tuesday, March 23**
>
> Portfolio value: $5,127,340 (+$2,180 today, +0.04%)
> Year-to-date: +2.5% ($127,340)
> S&P 500 year-to-date: +2.8%
>
> Today's activity: No trades. Portfolio within target allocation.
>
> Nothing requires your attention.

**On a day when trades happened:**

> **Midas Daily Summary -- Wednesday, March 24**
>
> Portfolio value: $5,134,560 (+$7,220 today, +0.14%)
> Year-to-date: +2.7% ($134,560)
> S&P 500 year-to-date: +3.0%
>
> Today's activity:
>
> - Rebalanced: Sold $25,000 of US Stocks, bought $25,000 of International Stocks (your US allocation had drifted 2% above target)
> - Tax-loss harvest: Sold $18,000 of International Bond ETF at a $2,100 loss, replaced with similar fund. Estimated tax savings: ~$700.
>
> Nothing requires your attention.

**On a bad day:**

> **Midas Daily Summary -- Thursday, March 25**
>
> Portfolio value: $4,975,200 (-$159,360 today, -3.1%)
> Year-to-date: -0.5% (-$24,800)
> S&P 500 today: -3.4%
>
> Today's activity: No trades. Midas did not sell into the decline -- your portfolio is diversified and your drawdown threshold (-15%) has not been triggered.
>
> Context: Today's market decline was driven by [brief explanation -- e.g., "unexpected interest rate decision by the Federal Reserve"]. Broad-based declines like this are normal -- the S&P 500 experiences a 3%+ daily drop roughly 5-6 times per year on average.
>
> Your safety boundaries:
>
> - Current drawdown from peak: -3.1% (your protection triggers at -15%)
> - Cash available: $248,000 (your minimum is $100,000)
>
> No action is needed. If you want to discuss, open the dashboard for more detail.

---

## What the Dashboard Shows When You Check In

You can open the Midas dashboard anytime. Here is what you see:

### Top Section: The Big Picture

> **Portfolio Value: $5,127,340**
> Today: +$2,180 (+0.04%) | This Month: +$34,500 (+0.68%) | YTD: +$127,340 (+2.5%)
>
> vs. S&P 500: YTD +2.8% | vs. Bonds: YTD +1.1%

A simple line chart shows your portfolio value over time, with the S&P 500 and a bond index as reference lines.

### Middle Section: What You Own

A clear breakdown of where your money is:

> | Category            | Target | Actual | Value      | Change Today |
> | ------------------- | ------ | ------ | ---------- | ------------ |
> | US Stocks           | 45%    | 46.1%  | $2,363,700 | +$1,050      |
> | International       | 20%    | 19.4%  | $994,200   | -$340        |
> | Bonds               | 20%    | 20.2%  | $1,035,700 | +$520        |
> | Real Estate (REITs) | 5%     | 4.8%   | $246,100   | +$180        |
> | Cash                | 5%     | 4.9%   | $248,640   | +$20         |
> | Tactical Reserve    | 5%     | 4.6%   | $239,000   | +$750        |
>
> Drift status: Within tolerance (no rebalancing needed)

### Bottom Section: Recent Activity

> **Last 7 Days**
>
> - Mar 23: No trades
> - Mar 22: Rebalanced $25,000 US to International
> - Mar 21: No trades
> - Mar 20: Tax-loss harvest -- saved estimated $700
> - Mar 19: No trades
> - Mar 18-19: Weekend (markets closed)
> - Mar 17: No trades
>
> **Next scheduled check: Tomorrow at market open**

### Sidebar: System Health

A simple status indicator:

> System Status: All Good
>
> - Brokerage connection: Connected
> - Market data feed: Active
> - Last sync: 5 minutes ago
> - Safety boundaries: Active
> - Next maintenance window: None scheduled

---

## Weekly Summary (sent every Sunday evening)

A slightly longer summary covering the full week:

> **Midas Weekly Summary -- Week of March 17-21**
>
> **Performance**
> Starting value: $5,092,840
> Ending value: $5,127,340
> Weekly return: +$34,500 (+0.68%)
> S&P 500 this week: +0.72%
> Bond index this week: +0.12%
>
> **Activity This Week**
>
> - Trades executed: 2
> - Tax-loss harvests: 1 (estimated tax savings: $700)
> - Risk alerts: 0
> - System errors: 0
>
> **Portfolio Health**
>
> - All allocations within target ranges
> - Drawdown from peak: -0.2% (your limit: -15%)
> - Cash position: $248,640 (your minimum: $100,000)
>
> **Insight**
> Your portfolio slightly underperformed the S&P 500 this week because bonds and international stocks lagged US stocks. This is expected -- diversification means you will not match the best-performing asset class in any given week, but you also will not match the worst. Over time, diversification reduces the volatility of your returns.
>
> No action needed. Have a good weekend.

---

## Monthly Report (sent on the 1st of each month)

A comprehensive monthly report:

> **Midas Monthly Report -- February 2026**
>
> **Summary**
> Starting value: $5,050,000
> Ending value: $5,127,340
> Monthly return: +1.53% ($77,340)
> Since inception (January 15): +2.55% ($127,340)
>
> **Performance vs. Benchmarks**
> | Benchmark | February | Since Inception |
> | ---------------------------- | -------- | --------------- |
> | Your portfolio | +1.53% | +2.55% |
> | S&P 500 | +1.62% | +2.80% |
> | 60/40 stock/bond blend | +1.21% | +2.10% |
> | Doing nothing (cash) | +0.35% | +0.72% |
>
> **Where the returns came from**
>
> - US Stocks contributed +0.82%
> - International Stocks contributed +0.31%
> - Bonds contributed +0.22%
> - Real Estate contributed +0.15%
> - Tax savings (harvesting) contributed +0.03% (estimated $1,400 saved)
>
> **Costs**
>
> - Trading commissions: $12.40
> - No management fees (you own the system)
>
> **Activity**
>
> - Total trades: 6
> - Tax-loss harvests: 2
> - Rebalancing events: 3
> - Risk events: 0
>
> **What to expect next month**
> No changes to strategy. Your portfolio remains well-diversified and within target allocations. Midas will continue to rebalance as needed and harvest tax losses when opportunities arise.

---

## What Does NOT Trigger a Notification

Midas is designed to be quiet. You only hear from it when something matters. It will NOT notify you about:

- Normal daily market movements (up or down less than your alert threshold)
- Small rebalancing trades under $10,000
- Routine system maintenance
- Market data updates
- Dividend payments (these are included in the daily summary)

---

## What DOES Trigger an Immediate Notification

These are the things Midas will text or email you about right away, outside of the regular summaries:

1. **Your daily loss threshold is hit** (e.g., portfolio down 3%+ in one day)
2. **Your drawdown protection is triggered** (Midas is automatically reducing risk)
3. **A trade failed** (order was rejected by the brokerage)
4. **Brokerage connection lost** (Midas cannot see your account or place trades)
5. **Data feed failure** (Midas cannot get market prices)
6. **A very large trade was executed** (over $100,000 in a single trade)
7. **System error** (something broke and needs your attention)

Each of these is described in detail in the Intervention Scenarios flow (flow 03).
