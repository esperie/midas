# Owner Setup Flow: Your First 60 Days with Midas

**Who this is for**: You, the owner, setting up Midas for the first time to manage your $5M.

**What happens at the end**: Your money is being invested automatically, according to your preferences, with safety guardrails you chose.

---

## Step 1: Installation and First Launch (Day 1, ~30 minutes)

### What you do

You open Midas for the first time. The system welcomes you and walks you through a guided setup. There is no dashboard yet -- just a focused setup wizard, one step at a time.

### What you see

A clean welcome screen:

> "Welcome to Midas. Let's get your investment system running. This setup takes about 30 minutes. You will:
>
> 1. Connect your brokerage account
> 2. Tell the system how you want to invest
> 3. Set your safety boundaries
> 4. Start a 30-day test run (no real money moves yet)
>
> Ready? Let's start."

### Decision point

None yet -- just press "Begin Setup."

---

## Step 2: Connecting Your Brokerage Account (Day 1, ~10 minutes)

### What you do

You connect Midas to your Interactive Brokers (IBKR) account. This is similar to connecting a bank account to a budgeting app -- you log in to IBKR through a secure window, and give Midas permission to view your accounts and place trades on your behalf.

### What you see

A screen that explains what is happening:

> "Midas needs to connect to your brokerage so it can see your holdings and make trades. You will log in to Interactive Brokers directly -- Midas never sees your brokerage password.
>
> Midas will be able to:
>
> - View your account balances and positions
> - Place buy and sell orders
> - Access market data
>
> Midas will NOT be able to:
>
> - Withdraw money from your account
> - Transfer money to other accounts
> - Change your personal information"

You click "Connect Interactive Brokers," log in through IBKR's own login page, and approve the connection.

### What happens next

Midas pulls in your current account information. You see a summary:

> **Account Connected**
>
> Account: [your account number, partially masked]
> Cash balance: $5,000,000.00
> Current holdings: None (or whatever you currently hold)
> Account type: Individual Margin
>
> Everything looks right? [Confirm] [Something is wrong]

### Decision point

Confirm that the account information looks correct. If you have existing holdings, Midas will ask you about them in the next step.

### If something goes wrong

If the connection fails, Midas shows a clear message:

> "We could not connect to Interactive Brokers. This usually means:
>
> - Your IBKR login credentials were incorrect
> - Your account does not have API access enabled (here is how to enable it: [link])
> - IBKR is temporarily unavailable
>
> Try again, or contact support."

---

## Step 3: Defining Your Investment Preferences (Day 1, ~15 minutes)

### What you do

You answer a series of plain-language questions about how you want your money invested. This is NOT a technical questionnaire about "alpha" and "beta" -- it is a conversation about your goals and comfort level.

### What you see

**Screen 1: Your Goals**

> "What is this money for?"
>
> - [ ] Long-term growth (10+ years, building wealth)
> - [ ] Medium-term (5-10 years, saving for something specific)
> - [ ] Income generation (I want regular cash payouts)
> - [ ] A mix (some growth, some income)
>
> "Do you have other sources of income or savings beyond this $5M?"
>
> - [ ] Yes, this is part of my total wealth
> - [ ] No, this is most of my wealth

**Screen 2: What to Invest In**

> "Midas invests primarily in diversified funds that track the broad market (similar to what Vanguard or Fidelity would recommend). Beyond that core, you can customize:
>
> **How much to keep in cash** (money that is not invested, available for withdrawals)
> Suggested: 3-5% ($150K-$250K)
> Your choice: [slider from 1% to 20%]
>
> **International exposure** (how much to invest in companies outside the US)
> Suggested: 20-30% of stock holdings
> Your choice: [slider from 0% to 50%]
>
> **Bond allocation** (more bonds = more stability, less growth potential)
> Suggested based on your goals: 15-25%
> Your choice: [slider from 0% to 60%]
>
> **Any sectors or types of companies you want to avoid?**
>
> - [ ] Fossil fuels / oil & gas
> - [ ] Weapons / defense contractors
> - [ ] Tobacco / alcohol
> - [ ] Gambling
> - [ ] None -- invest in everything
>
> **Anything you want to emphasize?**
>
> - [ ] Technology companies
> - [ ] Sustainable / ESG-focused companies
> - [ ] High-dividend stocks
> - [ ] Real estate investment trusts (REITs)
> - [ ] No preference -- just maximize returns"

**Screen 3: Tax Optimization**

> "Midas can save you money on taxes by selling investments that have lost value (harvesting tax losses) and immediately buying a similar investment to maintain your exposure. This is a well-established strategy used by most professional advisors.
>
> Based on your tax bracket, this could save you an estimated $25,000-$75,000 per year on a $5M portfolio.
>
> Enable tax-loss harvesting?
> [Yes, save me money on taxes] [No, keep things simple]"

### Decision points

- How much cash to keep available
- How much to invest internationally
- How much to put in bonds vs stocks
- Whether to exclude certain types of companies
- Whether to enable tax-loss harvesting

### What Midas does with your answers

After you answer, Midas shows you a proposed portfolio:

> **Your Proposed Portfolio**
>
> Based on your preferences, here is how your $5M would be invested:
>
> | Category            | Allocation | Amount     | What this is                    |
> | ------------------- | ---------- | ---------- | ------------------------------- |
> | US Stocks           | 45%        | $2,250,000 | Broad US market fund (like VTI) |
> | International       | 20%        | $1,000,000 | Companies outside the US        |
> | Bonds               | 20%        | $1,000,000 | Government and corporate bonds  |
> | Real Estate (REITs) | 5%         | $250,000   | Real estate investment trusts   |
> | Cash                | 5%         | $250,000   | Available for withdrawals       |
> | Tactical Reserve    | 5%         | $250,000   | Cash held to invest during dips |
>
> This portfolio is designed for long-term growth with moderate stability. Over the last 20 years, a portfolio like this would have averaged 7-9% annual returns, with a worst-case year of about -25%.
>
> [This looks right] [I want to adjust]

---

## Step 4: Setting Your Safety Boundaries (Day 1, ~5 minutes)

### What you do

You tell Midas when to hit the brakes. These are your personal circuit breakers -- the rules that override everything else.

### What you see

> **Safety Boundaries**
>
> These are your non-negotiable limits. If any of these are reached, Midas will automatically reduce risk and notify you immediately.
>
> **Maximum loss before automatic protection kicks in**
> "If my portfolio drops more than \_\_\_% from its highest value, start moving to safety."
>
> - [ ] 10% ($500,000 loss) -- very conservative, will trigger in most corrections
> - [ ] 15% ($750,000 loss) -- moderate, will trigger in significant downturns
> - [ ] 20% ($1,000,000 loss) -- allows for normal market volatility
> - [ ] 25% ($1,250,000 loss) -- tolerates larger drawdowns for potential recovery
> - [ ] Custom: \_\_\_\_%
>
> **What "move to safety" means:**
> When this limit is hit, Midas will sell some stocks and move the money to bonds and cash. It will NOT sell everything -- it will reduce stock exposure by 50% to limit further losses while keeping you in the market for recovery.
>
> **Maximum single-day loss before Midas alerts you**
> "Send me an urgent alert if the portfolio drops more than \_\_\_% in a single day."
>
> Suggested: 3% ($150,000)
> Your choice: [slider from 1% to 10%]
>
> **Minimum cash balance**
> "Never let my cash balance drop below $**\_\_**"
>
> Suggested: $100,000
> Your choice: [text field]
>
> **How do you want to be alerted?**
>
> - [ ] Text message (recommended for urgent alerts)
> - [ ] Email
> - [ ] Both
>
> Phone number: ******\_\_\_******
> Email: ******\_\_\_******

### Decision points

- What loss level triggers automatic protection
- What daily loss triggers an alert
- How much cash to always keep available
- How you want to receive alerts

### What Midas shows after you set boundaries

> **Your Safety Net**
>
> Here is what happens in different scenarios:
>
> | Scenario             | What Midas Does                         | What You Do                                |
> | -------------------- | --------------------------------------- | ------------------------------------------ |
> | Normal market day    | Invests and rebalances as needed        | Nothing -- check dashboard when you want   |
> | Bad day (-3%)        | Sends you an alert, continues investing | Read the alert, no action needed           |
> | Significant drop     | Reduces risk automatically, alerts you  | Review the situation, decide next steps    |
> | Market crash (-20%+) | Moves to defensive position, alerts you | You decide when to resume normal investing |
> | System error         | Stops trading, alerts you immediately   | You acknowledge and Midas resumes          |
>
> [These boundaries feel right] [I want to adjust]

---

## Step 5: The 30-Day Paper Trading Period (Days 2-31)

### What you do

Nothing. This is the "prove it works" period.

Midas runs exactly as it would with real money, but no actual trades are placed. Every morning it decides what it would buy and sell, tracks the results, and shows you how you would have done. This is like a dress rehearsal before the real performance.

### What you see on Day 2 (first morning)

A notification:

> "Paper trading has started. Midas is now simulating investments with your $5M. No real money will move during the 30-day test period.
>
> Check your dashboard anytime to see how things are going. I'll send you a weekly summary every Sunday."

### What the dashboard looks like during paper trading

There is a clear banner at the top:

> **PAPER TRADING MODE -- No real money is being invested**
> Day 7 of 30 | Test ends: [date]

Below that:

> **Simulated Performance**
>
> Starting value: $5,000,000
> Current simulated value: $5,023,450 (+0.47%)
> If you had done nothing (cash): $5,000,000
> S&P 500 over same period: +0.52%
>
> **What Midas would have done this week:**
>
> - Bought $250,000 of US Total Stock Market ETF
> - Bought $100,000 of International Stock ETF
> - Bought $100,000 of Bond ETF
> - Kept $250,000 in cash as planned
>
> **Trades simulated this week: 4**
> **Risk alerts: 0**
> **System health: All good**

### Weekly summary (sent every Sunday)

> **Week 1 Summary**
>
> Your simulated portfolio is up 0.47% ($23,450).
> The broad market (S&P 500) is up 0.52% over the same period.
>
> The system ran smoothly all week:
>
> - 4 simulated trades placed successfully
> - All risk checks passed
> - No alerts triggered
> - System uptime: 100%
>
> 23 days remaining in the paper trading period.

### What you see at the end of 30 days

A comprehensive summary:

> **Paper Trading Complete -- 30-Day Results**
>
> Starting value: $5,000,000
> Ending simulated value: $5,087,200 (+1.74%)
> S&P 500 over same period: +1.82%
> Bond index over same period: +0.31%
>
> **System Reliability**
>
> - Days the system ran correctly: 30/30
> - Simulated trades placed: 12
> - Simulated trades that would have executed successfully: 12/12
> - Risk checks triggered: 0
> - Errors encountered: 0
>
> **What this means:**
> The system performed as expected. Your portfolio slightly underperformed the stock market alone but provided more stability through diversification. All safety systems worked correctly.
>
> **Ready to go live?**
> [Yes, start investing my real money] [Extend paper trading for 30 more days] [I want to change my settings first]

### Decision point

This is the most important decision in the entire setup. You are choosing to let the system manage real money. Midas will not rush you.

---

## Step 6: Going Live -- First Real Investment (Day 32+)

### What you do

You press the "Go Live" button. Midas asks for a final confirmation.

### What you see

> **Going Live**
>
> You are about to let Midas invest your real money. Here is exactly what will happen:
>
> **Today and tomorrow:**
> Midas will begin buying investments according to your portfolio plan. It will NOT invest all $5M at once. It will spread the initial investment over 5-10 trading days to avoid market impact and get better prices.
>
> **Day 1 estimated activity:**
>
> - Buy ~$450,000 of US Total Stock Market ETF
> - Buy ~$200,000 of International Stock ETF
> - Buy ~$200,000 of Bond ETF
> - Remaining $4,150,000 stays in cash until subsequent days
>
> **Your safety boundaries are active:**
>
> - Automatic protection at -15% drawdown
> - Daily loss alert at -3%
> - Minimum cash: $100,000
> - Alerts via text and email
>
> **You can stop at any time.** If you press "Pause" on the dashboard, Midas will stop placing new trades immediately. Your existing investments stay in place until you decide what to do.
>
> Type "GO LIVE" to confirm: [____________]

### Decision point

Type "GO LIVE" to confirm. This is intentionally not just a button click -- typing requires deliberate action.

### What happens after you go live

You get a notification after the first real trades:

> "Your first real investments have been placed.
>
> Today's activity:
>
> - Bought 1,245 shares of VTI (US Total Stock Market) at $361.45 -- $450,005.25
> - Bought 3,890 shares of VXUS (International Stock) at $51.41 -- $199,983.90
> - Bought 2,564 shares of BND (Bond Fund) at $78.01 -- $199,993.64
>
> Total invested today: $849,982.79
> Remaining to invest: $4,150,017.21 (will be invested over the next 4-9 trading days)
>
> Your portfolio is now live. I'll send you a summary every evening after the market closes."

### What the dashboard looks like now

The paper trading banner is gone. In its place:

> **LIVE** | Portfolio value: $5,001,234.56 | Today: +$1,234.56 (+0.02%)

The dashboard shows:

- Total portfolio value (updated throughout the day during market hours)
- Today's gain or loss in dollars and percentage
- How much has been invested vs. how much is still in cash (during the initial deployment)
- A chart showing portfolio value over time (starts today)
- Next scheduled action ("Investing another ~$500K tomorrow")

---

## What Success Looks Like

After this setup process, you should be able to:

1. Not touch Midas for days or weeks at a time
2. Receive occasional notifications about what the system is doing
3. Check the dashboard whenever you are curious (but you do not need to)
4. Feel confident that safety boundaries will protect you in a downturn
5. See clear, honest performance reports comparing your results to benchmarks

The system is now running autonomously. The next flow (Daily Operations) describes what happens from here.
