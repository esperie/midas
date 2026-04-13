# Intervention Scenarios: When You Need to Take Action

**Who this is for**: You, the owner, when something needs your attention.

**The core idea**: Most days, Midas runs itself. This flow covers the situations where you need to step in -- either because the system flagged something, or because you want to make a change.

---

## Scenario 1: Circuit Breaker Triggered (Market Crash)

### What happened

The market has dropped significantly. Your portfolio has fallen past the drawdown protection threshold you set during setup (for example, 15% from its peak value).

### What Midas already did (before you even saw the alert)

Midas automatically reduced your stock exposure. It did NOT sell everything and go to cash. It followed a measured approach:

1. Sold enough stocks to reduce your equity exposure by approximately 50%
2. Moved the proceeds into short-term bonds and cash
3. Kept your core positions intact (just smaller)
4. Logged every trade with the reason "drawdown protection triggered"

### What you receive

An urgent notification (text message + email):

> **URGENT: Drawdown Protection Activated**
>
> Your portfolio has fallen 15.2% from its peak value of $5,250,000.
> Current value: $4,452,000 (-$798,000 from peak)
>
> What Midas did:
>
> - Sold $1,180,000 of stock positions (reducing equity exposure from 65% to ~33%)
> - Moved proceeds to short-term Treasury bonds and cash
> - Your remaining stock positions are intact, just smaller
>
> Your portfolio is now in "defensive mode":
>
> - 33% stocks (was 65%)
> - 42% bonds (was 20%)
> - 25% cash (was 10%)
>
> **What happens next:**
> Midas will NOT automatically buy back into stocks. It will hold this defensive position until you decide what to do.
>
> Open Midas to review your options.

### What you see when you open the dashboard

A prominent alert bar at the top:

> **Defensive Mode Active** -- Your drawdown protection triggered on [date]. Midas is holding a conservative position until you choose next steps.

Below that, three clear options:

> **Option A: Stay defensive**
> Keep the current conservative allocation. Midas will continue to manage the defensive portfolio, earning bond interest and keeping your money safe. Choose this if you think the market has further to fall.
>
> **Option B: Gradually return to normal**
> Midas will slowly move back to your original allocation over the next 30 trading days. If the market drops further during this period, it will pause and wait. Choose this if you think the worst is over but want to be cautious.
>
> **Option C: Return to normal immediately**
> Midas will buy back into your original allocation within 1-2 trading days. Choose this if you believe the drop was temporary and you want full market exposure.
>
> [Choose Option A] [Choose Option B] [Choose Option C]

### Decision point

You pick one of the three options. There is no wrong answer -- each reflects a different view of what the market will do next.

### What Midas communicates

After you choose, Midas confirms:

> "Got it. You chose to gradually return to your normal allocation over 30 days. Midas will buy approximately $39,000 of stocks each trading day, moving from 33% equity back to 65%. If the market drops another 5% during this period, Midas will pause and notify you.
>
> You can change this decision anytime from your dashboard."

---

## Scenario 2: System Error or Brokerage Disconnection

### What happened

Midas lost its connection to Interactive Brokers. This could be because IBKR is having technical problems, your API credentials expired, or there is an internet connectivity issue.

### What Midas does automatically

It stops trying to place new trades. Your existing investments stay exactly where they are -- nothing is bought or sold. Midas continues to attempt reconnection every 5 minutes.

### What you receive

A notification:

> **System Alert: Brokerage Connection Lost**
>
> Midas cannot connect to Interactive Brokers as of 10:23 AM Eastern.
>
> What this means:
>
> - Midas cannot place new trades
> - Midas cannot see your current account balance
> - Your existing investments are not affected (they are held at IBKR, not in Midas)
>
> What Midas is doing:
>
> - Attempting to reconnect every 5 minutes
> - No trades will be placed until the connection is restored
>
> What you can do:
>
> - Check IBKR's status page to see if they are experiencing issues: [link]
> - Log in to IBKR directly to verify your account is fine: [link]
> - If the issue persists for more than 1 hour, you may need to re-authorize Midas (instructions below)

### What you see on the dashboard

A warning banner:

> **Brokerage Disconnected** since 10:23 AM | Midas is attempting to reconnect
> Last known portfolio value: $5,127,340 (as of 10:22 AM)

### If the connection is restored automatically

You get a follow-up notification:

> "Brokerage connection restored at 10:48 AM. Midas verified your account -- everything looks correct. Normal operations have resumed."

### If it is NOT restored within 2 hours

An escalated alert:

> "Brokerage connection has been down for 2 hours. Midas is unable to trade. Please take one of these steps:
>
> 1. Log in to Interactive Brokers directly to check your account
> 2. Re-authorize Midas using the Settings page
> 3. If IBKR is having a system-wide outage, no action is needed -- Midas will reconnect automatically when they come back online"

### Decision point

If the issue requires re-authorization, you go to Settings and re-connect your brokerage (same process as initial setup). If IBKR is down, you wait. Your money is safe at IBKR regardless.

---

## Scenario 3: Strategy Underperforming

### What happened

Your portfolio has been consistently underperforming the S&P 500 for several months. Midas notices this and proactively tells you about it.

### What you receive (in your monthly report)

An additional section with a yellow highlight:

> **Performance Check-In**
>
> Your portfolio has underperformed the S&P 500 for 3 consecutive months:
>
> | Month    | Your Portfolio | S&P 500 | Difference |
> | -------- | -------------- | ------- | ---------- |
> | January  | +1.2%          | +2.1%   | -0.9%      |
> | February | +0.8%          | +1.5%   | -0.7%      |
> | March    | -0.3%          | +0.4%   | -0.7%      |
>
> **Why this is happening:**
> Your portfolio is more diversified than the S&P 500. In periods when US large-cap stocks outperform everything else (which they have done recently), a diversified portfolio will lag. Your international stocks, bonds, and real estate have all been slower than US stocks this quarter.
>
> **Is this a problem?**
> Not necessarily. Diversification means you trade some upside in the best-performing asset class for less downside when that asset class falls. Over 10+ year periods, diversified portfolios have historically provided similar returns to the S&P 500 with significantly less volatility.
>
> **Your options:**
>
> 1. **Stay the course** -- diversification is doing what it is supposed to do. Most financial advisors would recommend this.
> 2. **Increase US stock allocation** -- if you want more US stock exposure, you can adjust your preferences. This increases potential returns but also increases risk.
> 3. **Switch to a simpler strategy** -- you could move to a single US stock market fund (like the S&P 500). Simpler, but you lose the benefits of diversification.
>
> [Keep current strategy] [Adjust allocation] [Talk through options]

### Decision point

You can either stay the course (recommended), or make changes. If you choose to adjust, Midas walks you through the same preference screens from setup, showing how different allocations would have performed historically.

### What Midas communicates if you change nothing

> "No changes made. Your strategy is unchanged. I'll continue to provide performance comparisons in your monthly reports."

---

## Scenario 4: You Want to Withdraw Cash

### What you do

You open the dashboard and tap "Withdraw Cash."

### What you see

> **Withdraw Cash**
>
> Available cash in your account: $248,640
> Minimum cash balance (your setting): $100,000
> Available for immediate withdrawal: $148,640
>
> How much would you like to withdraw? $****\_\_****
>
> **If you need more than $148,640:**
> Midas will need to sell some investments to raise cash. This may take 1-3 business days for the trades to settle. Midas will sell in the most tax-efficient way possible (selling positions with losses first to offset any gains).

### If the amount is within available cash

> "Withdrawal of $50,000 initiated. This will be transferred to your linked bank account within 1-2 business days.
>
> Updated cash balance: $198,640
> Your minimum cash setting ($100,000) is still satisfied."

### If the amount requires selling investments

> "You want to withdraw $300,000, but only $148,640 is available as cash. Midas needs to sell $151,360 of investments.
>
> Here is the plan:
>
> - Sell $80,000 of US Stock ETF (has a $3,200 unrealized loss -- selling this offsets gains)
> - Sell $71,360 of Bond ETF (has a $1,100 unrealized gain -- estimated tax impact: ~$250)
>
> Total estimated tax impact of this withdrawal: ~$250 (net, after using the loss to offset the gain)
>
> Trades will be placed tomorrow. Cash should be available for withdrawal in 3 business days.
>
> [Proceed with withdrawal] [Change amount] [Cancel]"

### Decision point

Approve the withdrawal plan or adjust the amount. Midas always shows the tax impact before you commit.

---

## Scenario 5: You Want to Change Risk Tolerance

### What you do

Open Settings and select "Safety Boundaries."

### What you see

Your current settings, each editable:

> **Your Current Safety Boundaries**
>
> Maximum drawdown before protection: **15%** [Change]
> Daily loss alert threshold: **3%** [Change]
> Minimum cash balance: **$100,000** [Change]
>
> **To change your investment style (more or less aggressive), go to Investment Preferences.**

### If you change the drawdown protection

> "You want to change your drawdown protection from 15% to 10%.
>
> What this means:
>
> - Midas will step in sooner during market declines (at a $500,000 loss instead of $750,000)
> - This may trigger more often -- a 10% drop happens roughly once every 1-2 years in normal markets
> - When triggered, Midas will reduce stock exposure by 50%, same as before
>
> Are you sure? [Confirm change] [Keep at 15%]"

### If you want to become more aggressive or conservative

You go to Investment Preferences and adjust the sliders (same screens as initial setup). Midas shows the impact:

> "You want to increase your stock allocation from 65% to 80%.
>
> What this means:
>
> - Higher expected returns (historically ~9-10% per year instead of ~7-8%)
> - Higher volatility (worst years could be -30% to -40% instead of -20% to -25%)
> - Midas will need to buy approximately $375,000 more in stock positions and sell bonds/cash to fund it
>
> This change will be implemented over 3-5 trading days.
>
> [Confirm new allocation] [Go back]"

---

## Scenario 6: You Want to Add More Capital

### What you do

Deposit additional money into your IBKR account (you do this through IBKR directly, not through Midas).

### What Midas sees

During its daily sync, Midas notices your cash balance has increased by more than expected.

### What you receive

> "New cash detected: $500,000 was deposited into your account.
>
> Midas will invest this according to your current portfolio allocations:
>
> - $225,000 into US Stocks
> - $100,000 into International Stocks
> - $100,000 into Bonds
> - $25,000 into Real Estate
> - $25,000 into Cash Reserve
> - $25,000 into Tactical Reserve
>
> Investment will be spread over 3-5 trading days.
>
> [Invest as planned] [Adjust how this money is invested] [Keep as cash for now]"

### Decision point

You can invest according to your existing plan, customize how this particular deposit is invested, or hold it as cash.

---

## Scenario 7: Tax Season -- What Reports Are Available

### What happens (around January-February each year)

Midas prepares a tax summary for the previous year.

### What you receive

> **Your 2025 Tax Summary is Ready**
>
> Here is a summary of the tax-relevant activity in your portfolio last year:
>
> **Gains and Losses**
>
> - Short-term capital gains (held < 1 year): $12,340
> - Long-term capital gains (held > 1 year): $45,670
> - Harvested losses applied: -$28,900
> - Net taxable gain: $29,110
>
> **Estimated tax impact:**
> At a 37% federal + 5% state rate: approximately $12,200
>
> **Without tax-loss harvesting, your tax bill would have been:**
> Approximately $24,100 -- Midas saved you an estimated $11,900 in taxes.
>
> **Dividends received:** $67,800
>
> - Qualified dividends (lower tax rate): $52,400
> - Non-qualified dividends (ordinary income rate): $15,400
>
> **Reports available for download:**
>
> - Tax summary (PDF) -- a clear summary for you
> - Detailed transaction log (CSV) -- every trade, for your accountant
> - Realized gains/losses report -- for Schedule D
> - Dividend income report -- for Schedule B
> - Cost basis report -- for your records
>
> **Important:** Your official 1099 tax forms come from Interactive Brokers, not from Midas. IBKR typically sends 1099s by mid-February. The Midas tax summary helps you understand your tax situation before the official forms arrive.
>
> [Download Tax Summary] [Download All Reports] [Send to my accountant]

### Decision point

Download the reports you need and share with your accountant. Midas makes it easy to email the full package directly to an email address.

---

## Summary: When Do You Need to Act?

| Situation                     | Urgency  | What Midas Does First                | What You Decide                    |
| ----------------------------- | -------- | ------------------------------------ | ---------------------------------- |
| Drawdown protection triggered | High     | Reduces risk automatically           | When and how to return to normal   |
| Brokerage disconnection       | Medium   | Stops trading, tries to reconnect    | Re-authorize if needed             |
| Strategy underperforming      | Low      | Explains why, shows options          | Whether to change strategy         |
| You want to withdraw cash     | You set  | Shows tax impact, proposes sell plan | Approve withdrawal                 |
| You want to change risk       | You set  | Shows impact of the change           | Confirm new settings               |
| You add more capital          | Low      | Proposes investment plan             | Approve or customize               |
| Tax season                    | Seasonal | Prepares reports automatically       | Download and share with accountant |
