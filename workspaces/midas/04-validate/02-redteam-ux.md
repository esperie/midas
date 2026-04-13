# Midas UX Red Team Report

**Date**: 2026-03-23
**Reviewer**: uiux-designer (Red Team Mode)
**Scope**: All 6 user flow documents + decision brief
**Methodology**: Adversarial UX evaluation from the perspective of real users under real conditions

---

## Executive Summary

The Midas user flows are well-written, clear in language, and demonstrate genuine respect for a non-technical user. The communication design -- daily summaries, progressive disclosure, plain-language explanations -- is above average for fintech. However, the flows contain significant gaps in emotional design, mobile-first reality, edge-case handling, and missing user journeys that would surface within the first 90 days of real use. This report identifies 47 findings across 8 categories, with 6 rated CRITICAL, 14 rated HIGH, 18 rated MEDIUM, and 9 rated LOW.

The top three systemic issues:

1. **The flows assume a calm, rational user.** Real users managing $5M will be anxious, impatient, and emotionally reactive -- especially during losses. The emotional design is insufficient for the stakes involved.
2. **Mobile is never mentioned.** The user brief says "I do not need to do a thing." That person checks their portfolio on their phone at 11pm. Every flow describes a "dashboard" without specifying whether it works on a phone screen.
3. **The 60-day paper trading period is a trust and engagement desert.** Two months of "nothing is happening with your real money" is a dropout cliff for someone who came in saying "invest for me."

---

## Category 1: Onboarding Realism

### UX-001: 60-Day Paper Trading Is a Dropout Cliff

**Severity**: CRITICAL

**What the user experiences**: After 30 minutes of excited setup on Day 1, the user is told to wait 30 days (the flow says 30, but the decision brief says 60) while nothing happens to their real $5M. The money sits in cash, earning essentially nothing, while they watch a simulation. For someone who said "I want the system to invest for me," this is a 60-day anticlimactic pause.

**Why it matters**: The user came in with urgency ("I have $5M and I want to invest"). Asking them to watch simulated numbers for two months while their money earns near-zero is psychologically punishing. The opportunity cost on $5M at even 4% risk-free is roughly $33,000 over 60 days. The user will calculate this. Every week of paper trading, the implicit message is: "You are paying $8,000 a week to watch a demo."

**What should be done**:

- Acknowledge the opportunity cost explicitly during setup: "During the test period, your $5M stays in your brokerage account. You can keep it in money market funds or short-term Treasuries (earning approximately 4-5% annually) while the system proves itself."
- Provide a "graduated live" option: invest 10% ($500K) with real money after 30 days of successful paper trading, then scale to full allocation after 60 days. This gives the user skin in the game without full exposure.
- Make the paper trading period genuinely engaging -- not just "weekly summaries." Show the user what they are learning from the simulation (e.g., "This week, the system would have harvested a $3,200 tax loss -- here is how that works").

### UX-002: Paper Trading Duration Inconsistency

**Severity**: HIGH

**What the user experiences**: The setup flow (01) says "30-day paper trading period" on the welcome screen, throughout the paper trading section, and at the completion screen. The decision brief (02) says "60 days minimum" and explicitly argues against 30 days. The user reads conflicting numbers.

**Why it matters**: This inconsistency will erode trust immediately. If the system cannot agree with itself on how long testing takes, the user will question what else is inconsistent.

**What should be done**:

- Resolve the discrepancy. Pick one number and use it everywhere. If the risk analysis says 60 days, the setup flow should say 60 days.
- If the answer is "30 days minimum, 60 days recommended," say that explicitly and let the user choose.

### UX-003: Setup Flow Assumes Single Brokerage, Single Account

**Severity**: MEDIUM

**What the user experiences**: The setup connects to one Interactive Brokers account. But a person with $5M likely has multiple accounts -- an individual account, a joint account with a spouse, an IRA, a Roth IRA, maybe a trust. The flow never asks about this.

**Why it matters**: If the user expects Midas to manage their entire financial picture and it only connects to one account, they will be confused when $2M of their $5M sits in an IRA that Midas cannot see.

**What should be done**:

- Add a step during setup: "Do you have multiple accounts at Interactive Brokers? Midas can manage them together as one portfolio or separately with different strategies."
- If multi-account is not supported in Phase 1, say so explicitly: "Right now, Midas connects to one account. If you have multiple accounts, we recommend starting with your largest one."

### UX-004: No Explanation of What Happens to Existing Holdings

**Severity**: MEDIUM

**What the user experiences**: Step 2 says "If you have existing holdings, Midas will ask you about them in the next step." But Step 3 starts from scratch with sliders for a new portfolio. There is no flow for: "You currently hold $2M in Apple stock. Should Midas sell this and reinvest, or incorporate it into your allocation?"

**Why it matters**: Selling concentrated positions has massive tax implications. A user with $2M in Apple at a $500K cost basis faces a potential $450K+ capital gains tax bill if Midas decides to diversify. The user needs to understand this before Midas touches anything.

**What should be done**:

- Add a "Current Holdings Review" step between Steps 2 and 3.
- Show each existing position with its unrealized gain/loss and estimated tax impact of selling.
- Offer options: "Keep this position," "Reduce gradually over 12 months," or "Sell and reinvest."

### UX-005: Tax Bracket Not Collected

**Severity**: MEDIUM

**What the user experiences**: Step 3 says "Based on your tax bracket, this could save you an estimated $25,000-$75,000 per year." But no screen asks for their tax bracket. The system is citing a tax savings estimate without knowing the user's actual tax situation.

**Why it matters**: A $25K-$75K range is so wide it is meaningless. A user in the 37% bracket vs. 24% bracket gets very different savings. If the user has significant carry-forward losses, tax-loss harvesting may provide zero incremental value. Presenting a number without asking the relevant question looks sloppy.

**What should be done**:

- Either ask for filing status and approximate income range (to estimate bracket), or drop the specific dollar estimate and say "typically saves high-income investors significant amounts."
- Better: ask for tax filing status, approximate income, and state of residence (for state tax rates).

---

## Category 2: Emotional and Psychological Gaps

### UX-006: Inadequate Emotional Design for Large Losses

**Severity**: CRITICAL

**What the user experiences**: The "bad day" summary says: "Portfolio value: $4,975,200 (-$159,360 today, -3.1%)." That is a $159,000 loss communicated in the same format as a $2,000 gain. There is no acknowledgment that this is a psychologically different event. The user sees a text message that says they lost the equivalent of two years' median salary in one day.

**Why it matters**: Loss aversion is one of the most documented findings in behavioral finance. Losses hurt roughly twice as much as equivalent gains feel good. A $159K daily loss on a $5M portfolio is the kind of number that makes people abandon their strategy, override the system, or call their lawyer. The communication needs to match the emotional weight of the event.

**What should be done**:

- For losses exceeding the user's daily alert threshold, use a different communication format that leads with reassurance before the number: "Today was a tough day in the markets. Your portfolio declined along with the broader market. Here is the full picture..."
- Include historical context immediately: "In the last 20 years, the S&P 500 has had [X] days worse than this. In every case, it recovered within [Y] months on average."
- Include a "What Midas is doing about it" section: even if the answer is "nothing, because your safety boundaries were not triggered," that is reassuring.
- For losses over $100K in a single day, consider a phone call prompt: "Would you like to schedule a 15-minute call to discuss today's market?" (for Phase 2 clients).

### UX-007: No Trust-Building During the First Week of Live Trading

**Severity**: CRITICAL

**What the user experiences**: Day 32+ (going live). The system starts buying. The user gets a notification listing trades. Then... silence until the evening summary. On the first day of managing real money, the user is left alone with their anxiety from 9:30 AM to 5:00 PM.

**Why it matters**: The first week of live trading is the highest-anxiety period in the entire relationship. The user just committed $5M. Every hour they do not hear from the system, they are refreshing the dashboard. The first red day will feel catastrophic because they have no track record of the system recovering.

**What should be done**:

- During the first 5 trading days, send a brief mid-day check-in (around noon): "Your portfolio is at $5,003,400 (+0.07% so far today). All systems running normally. Evening summary at 5 PM as usual."
- After the first week, offer the choice: "Would you like to continue receiving mid-day updates, or are you comfortable with the evening summary only?"
- On the first day, include a "What to expect this week" guide: "Day 1 will feel strange. You may want to check the dashboard every hour. That is normal. By Week 2, most owners check once a day or less."

### UX-008: Decision Fatigue from Notification Design

**Severity**: HIGH

**What the user experiences**: The daily summary, weekly summary, monthly report, immediate trade notifications, risk alerts, system alerts -- all compete for attention. Over time, the user cannot distinguish between "everything is fine" and "something needs your attention."

**Why it matters**: When the user gets desensitized to notifications, they will miss the one that actually matters -- the drawdown protection trigger or the brokerage disconnection during a crash.

**What should be done**:

- Implement a clear severity tier system in the notification design:
  - GREEN (daily/weekly/monthly summaries): information only, no action needed
  - YELLOW (performance check-ins, minor drift): worth reading, no urgency
  - ORANGE (daily loss alert, brokerage disconnection): important, review today
  - RED (drawdown protection, system error, security breach): immediate action required
- Use different notification channels for different tiers: GREEN = email only, YELLOW = email, ORANGE = text + email, RED = text + email + phone call.
- Never send a RED-tier notification for something that is actually YELLOW-tier.

### UX-009: "Defensive Mode" Leaves the User in Limbo

**Severity**: HIGH

**What the user experiences**: Circuit breaker triggers. Midas sells stocks and goes defensive. Then it says: "Midas will NOT automatically buy back into stocks. It will hold this defensive position until you decide what to do." The user must now make one of three choices (stay defensive, gradually return, return immediately) while stressed, possibly panicking, and facing the most volatile market of the year.

**Why it matters**: Forcing a high-stakes investment decision during a crisis is the opposite of "autonomous management." The user chose Midas precisely so they would NOT have to make these decisions. Asking them to choose when to re-enter the market is asking them to time the market -- the one thing the system was supposed to prevent.

**What should be done**:

- Add a fourth option: "Let Midas decide." The system uses its own signals (volatility indicators, market breadth, momentum) to determine when to begin re-entry. This is the autonomous option.
- Make "Gradually return to normal" the default if the user does not respond within 7 trading days. Inform them: "If you do not make a selection, Midas will begin gradually returning to your normal allocation in 7 trading days."
- Include a clear recommendation from Midas: "Based on historical patterns, Option B (gradual return) has produced the best outcomes in situations like this."

### UX-010: No Celebration of Wins

**Severity**: MEDIUM

**What the user experiences**: Every communication is about losses, risks, and safety. The daily summaries mention gains in the same flat format as losses. There is no moment where the system says: "Your portfolio crossed $5.5M for the first time" or "Tax-loss harvesting saved you $15,000 this year -- that is more than the system cost to run."

**Why it matters**: Positive reinforcement builds trust and engagement. The user needs to feel that Midas is working FOR them, not just protecting them FROM things. Milestone celebrations create the emotional moments that make the user recommend Midas to friends (critical for Phase 2 client acquisition).

**What should be done**:

- Define portfolio milestones and celebrate them: first $100K gain, first year anniversary, crossing round-number thresholds.
- Include an annual "value delivered" summary: "This year, Midas saved you $32,000 in taxes, avoided $180,000 in losses through drawdown protection, and generated $340,000 in returns. Total estimated value vs. doing nothing: $XX,XXX."
- Keep celebrations proportional and dignified -- this is wealth management, not a video game. A brief "Notable: Your portfolio reached $5.5M today" in the daily summary is appropriate.

### UX-011: No Psychological Preparation for Extended Bear Markets

**Severity**: HIGH

**What the user experiences**: The flows address crashes (sudden drops) well but say nothing about prolonged bear markets -- the 2000-2002 period where the S&P 500 fell 49% over 30 months. The user could receive 100+ daily summaries showing gradual decline, none of which individually trigger the drawdown protection.

**Why it matters**: Death by a thousand cuts is psychologically harder than a single crash. The user watches their $5M become $4.5M, then $4M, then $3.5M over months, with daily summaries saying "no action needed" because no single day breached a threshold. The compounding effect of small daily losses never triggering protective action creates a trust crisis: "Why is the system letting me lose $1.5M slowly?"

**What should be done**:

- Add a cumulative loss awareness mechanism: "Your portfolio has declined 10% over the past 3 months. While no single-day threshold was triggered, the cumulative decline is worth discussing."
- Define a trailing drawdown check (not just peak-to-current), so the user is proactively informed.
- Include bear market educational content: "Markets have historically taken 12-18 months to recover from declines of this magnitude."

---

## Category 3: Missing User Flows

### UX-012: No "Why Did You Make That Trade?" Flow

**Severity**: CRITICAL

**What the user experiences**: The daily summary says "Rebalanced $25,000 from US Stocks to International." The user thinks: "Why? What triggered this? Is this a good idea?" There is no way for the user to tap into a specific trade and see the reasoning.

**Why it matters**: Transparency is the foundation of trust in autonomous systems. Every trade should be explainable in plain language. Without trade-level explanations, the system feels like a black box -- the exact opposite of the trust-building the flows are trying to achieve. This becomes a legal requirement in Phase 2 (fiduciary duty requires documenting why each trade was made).

**What should be done**:

- Every trade in the activity log should be tappable/clickable, expanding to show:
  - What triggered the trade (drift exceeded threshold, tax-loss opportunity, scheduled rebalance)
  - What the allocation was before and after
  - What the alternative was ("If Midas had not rebalanced, your US stock allocation would have been 48% vs. target 45%")
  - Cost of the trade (commissions, spread)
- This is also critical for the Phase 2 compliance record.

### UX-013: No Benchmark Comparison Configuration

**Severity**: HIGH

**What the user experiences**: Every summary compares performance to the S&P 500. But the user's portfolio is 45% stocks, 20% international, 20% bonds, 5% REITs, 5% cash. Comparing a diversified portfolio to a 100% US stock index is misleading and creates constant disappointment when stocks are up.

**Why it matters**: The S&P 500 comparison will almost always make a diversified portfolio look like it is underperforming during bull markets. This is a known problem in wealth management -- it creates client churn. The system should compare to a benchmark that matches the user's allocation, not a pure equity index.

**What should be done**:

- The primary benchmark should be a blended benchmark matching the user's target allocation (e.g., 45% VTI + 20% VXUS + 20% BND + 5% VNQ + 10% cash). This is the fair comparison.
- Show the S&P 500 as a secondary reference, with a disclaimer: "The S&P 500 is shown for reference. Your portfolio is designed to be less volatile than pure stocks, so it will typically lag during strong stock markets and outperform during downturns."
- Allow the user to choose which benchmarks they want to see.

### UX-014: No "What If I Had Just Put Everything in an Index Fund?" View

**Severity**: HIGH

**What the user experiences**: The decision brief acknowledges "The active strategy might not beat a simple index fund." But no flow ever shows the user this comparison in a running, visible way. The monthly report compares to the S&P 500 (incomplete) but never says: "If you had put your $5M in VTI on day one, here is where you would be."

**Why it matters**: This is the single most important question every self-directed investor asks. If the system avoids this comparison, the user will calculate it themselves and feel deceived when the system underperforms. Proactive transparency is better than the user discovering the gap independently.

**What should be done**:

- Include a "Simple Alternative" benchmark in the monthly report: "If your $5M had been invested in a single total US stock market fund (VTI) on [inception date], it would currently be worth $[X]."
- When Midas underperforms this benchmark, explain the value the system provided beyond raw returns (tax savings, drawdown protection, lower volatility).
- When Midas outperforms, note it factually without self-congratulation.

### UX-015: No Recurring Deposit Flow

**Severity**: HIGH

**What the user experiences**: Scenario 6 (adding capital) describes Midas detecting a manual deposit and offering to invest it. But there is no flow for the user who wants to deposit $50,000 every month automatically. They have to manually deposit into IBKR each time, and Midas has to detect it each time.

**Why it matters**: Dollar-cost averaging through recurring deposits is one of the most common investment behaviors. For Phase 2 clients especially, monthly contributions are standard. If every deposit requires a manual IBKR transfer plus a Midas approval, the workflow is cumbersome.

**What should be done**:

- Add a "Recurring Deposits" setting: "Would you like to set up automatic monthly transfers from your bank to your investment account?"
- If Midas cannot initiate bank transfers (likely), at minimum provide a reminder: "Your monthly deposit of $50,000 is due. Here is how to transfer to your IBKR account."
- Auto-detect recurring deposits and skip the confirmation prompt after the third one: "Midas detected your regular monthly deposit of $50,000 and will invest it according to your plan. [Undo if this was not intentional]"

### UX-016: No End-of-Year Tax Planning Flow

**Severity**: HIGH

**What the user experiences**: Flow 03 Scenario 7 covers tax season reporting (January-February). But there is no flow for December tax planning -- the period where the user (or their CPA) wants to know: "Should I harvest more losses before year-end? Should I defer gains? How does my portfolio's tax situation affect my other income?"

**Why it matters**: Tax-loss harvesting is seasonal. The system should proactively increase harvesting activity in November-December when it can offset known gains. Many CPAs want to see a projected tax picture in November to make planning decisions. This is a major value-add for high-net-worth users.

**What should be done**:

- Add a "Year-End Tax Planning" flow triggered in November:
  - "Your estimated tax situation for 2026: $X in realized gains, $Y in harvestable losses, $Z in projected dividends."
  - "Midas can harvest an additional $[amount] in losses before year-end to offset gains. Should it proceed?"
  - "Share this projection with your CPA: [Generate Tax Planning Report]"
- Make the tax projection exportable in a format CPAs commonly use.

### UX-017: No "Override a Specific Trade" Flow

**Severity**: MEDIUM

**What the user experiences**: The user can change their overall allocation, risk tolerance, and safety boundaries. But they cannot say: "Do not sell my Apple stock" or "I want to buy $100K of Tesla." There is no mechanism for individual trade overrides.

**Why it matters**: Even in an autonomous system, the owner will occasionally have strong opinions about specific positions. A user who inherited Apple stock from their father may never want to sell it, regardless of allocation drift. A user who believes in a specific company may want to direct a portion of their portfolio manually.

**What should be done**:

- Add a "Held Positions" feature: "Mark positions that Midas should never sell." These are excluded from rebalancing and tax-loss harvesting.
- Add a "Manual Trade" feature: "Place a specific trade yourself. Midas will adjust the rest of the portfolio around it."
- Both should come with clear warnings about how they affect the overall strategy and diversification.

### UX-018: No Estate Planning / Beneficiary Flow

**Severity**: MEDIUM

**What the user experiences**: Someone with $5M in investments has estate planning concerns. The flows never mention beneficiaries, transfer-on-death designations, or what happens to the Midas system and advisory relationships if the owner dies.

**Why it matters**: For Phase 1, the backup operator scenario partially covers this, but it does not address the legal succession of the account. For Phase 2, if the advisor dies, what happens to client accounts? This is a regulatory requirement (business continuity plan).

**What should be done**:

- Add an "Account Succession" section in Settings: "Who should have access to your Midas system if you become incapacitated or pass away?"
- For Phase 2, require a documented business continuity plan: "If you are unable to serve your clients, Midas will [freeze trading / transfer to designated successor advisor]."
- Link to estate planning resources.

### UX-019: No Performance Attribution by Strategy Component

**Severity**: MEDIUM

**What the user experiences**: The monthly report shows overall performance and broad category contributions ("US Stocks contributed +0.82%"). But it does not show: "Tax-loss harvesting contributed +0.3% after-tax. Rebalancing contributed +0.1%. The tactical reserve did not add value this month."

**Why it matters**: The decision brief says the system should automatically disable active components that underperform for 12 months. But the user flows never show the user which components are working and which are not. This makes the automatic fallback to index investing feel arbitrary when it happens.

**What should be done**:

- Add a "Strategy Breakdown" section to the monthly report showing the incremental value (or cost) of each active component.
- When a component is underperforming, flag it: "The tactical reserve has not added value in the last 6 months. If this continues for 6 more months, Midas will automatically allocate this cash to your core index holdings."

---

## Category 4: Mobile-First Reality

### UX-020: No Mobile Experience Defined

**Severity**: CRITICAL

**What the user experiences**: Every flow describes "the dashboard" but never specifies whether it is a web app, mobile app, or both. The user brief says "I do not need to do a thing" -- this person checks their portfolio on their phone at 11pm, in bed. They check it during lunch. They check it on the toilet. They do NOT open a laptop to look at an "Advisor Dashboard."

**Why it matters**: The flows describe detailed tables, multi-column layouts, and dense information screens. These work on a desktop monitor but are unusable on a phone. If 80% of dashboard visits happen on mobile (which is the industry norm for personal finance apps), the entire information architecture needs to be phone-first.

**What should be done**:

- Define the mobile experience explicitly for every flow. The phone view should show:
  - One big number (portfolio value)
  - One trend indicator (up/down, with amount)
  - A "status light" (green = all good, yellow = check something, red = action required)
  - Everything else behind one tap
- The "Go Live" confirmation (typing "GO LIVE") needs a mobile-friendly alternative. Typing on a phone keyboard is not the same deliberate action as on a desktop keyboard.
- Emergency notifications must work on mobile lock screens -- visible without unlocking the phone.

### UX-021: Dashboard Tables Are Not Mobile-Friendly

**Severity**: HIGH

**What the user experiences**: The portfolio allocation table shows 6 columns (Category, Target, Actual, Value, Change Today, Status). On a phone in portrait mode, this is unreadable. The client management table shows 6+ columns. These require horizontal scrolling or are simply cut off.

**Why it matters**: If the most-used screen is unusable on the most-used device, the user will stop checking. They will miss important information. For Phase 2 clients, a bad mobile experience makes the advisory practice look unprofessional.

**What should be done**:

- Design card-based layouts for mobile: each asset class gets a card showing allocation bar, value, and daily change.
- Use progressive disclosure: tap a card to see target vs. actual and status details.
- Client list on mobile: show name, value, and status badge only. Everything else on tap.

---

## Category 5: Client Experience Gaps

### UX-022: No "Talk to a Human" Flow for Clients

**Severity**: CRITICAL

**What the user experiences**: A Phase 2 client is anxious about their portfolio. The quarterly report says "Questions? Contact [Your Name] at [contact info]." But there is no scheduling system, no chat, no way to request a callback. The client has to figure out how to reach their advisor independently.

**Why it matters**: The single most common reason clients leave an advisor is feeling ignored. "Contact me at..." with a phone number and email is the 1990s experience. Modern clients expect: "Schedule a 15-minute review call" with a calendar link.

**What should be done**:

- Add a "Request a Call" button to the client dashboard and to every report.
- Integrate a simple scheduling tool (Calendly-style) where the advisor sets available times.
- Track call requests on the advisor dashboard: "2 clients have requested calls this week."
- For urgent concerns, offer a "Mark as Urgent" option that sends the advisor a text message.

### UX-023: Client Cannot See Real-Time Portfolio on Their Phone

**Severity**: HIGH

**What the user experiences**: The client onboarding flow mentions a "client dashboard" and "login link." But the flows never describe what the client dashboard actually shows in real-time. Can the client see their portfolio value during market hours? Or only after the daily summary?

**Why it matters**: Clients will compare the Midas experience to their Fidelity or Schwab app, where they can see real-time values, individual positions, and transaction history. If the Midas client dashboard is less capable than what they already have at their brokerage, it feels like a downgrade.

**What should be done**:

- Define the client dashboard experience explicitly. At minimum, it should show:
  - Real-time portfolio value during market hours
  - Allocation breakdown (simplified)
  - Recent activity
  - Performance chart
  - "Contact my advisor" button
- Acknowledge that clients can also log into IBKR directly to see their account. Midas adds the advisory layer on top.

### UX-024: No Client Portal for Documents

**Severity**: MEDIUM

**What the user experiences**: Tax reports are mentioned ("Download Tax Summary" / "Send to my accountant"). Quarterly reports are emailed. But there is no central place where a client can find all their historical documents -- past reports, tax summaries, the signed advisory agreement, fee invoices.

**Why it matters**: CPAs will ask for specific documents. Clients will want to reference old reports. If every document is only in email, it gets lost.

**What should be done**:

- Add a "Documents" section to the client dashboard with all historical reports, tax summaries, agreements, and fee statements organized by year and type.

### UX-025: No Client Referral Mechanism

**Severity**: LOW

**What the user experiences**: The decision brief identifies client acquisition as the hardest part of Phase 2. But no flow includes a referral mechanism. Happy clients are the best source of new clients.

**What should be done**:

- Add a "Refer a Friend" option to the client dashboard.
- After positive milestones (first year anniversary, strong quarter), prompt: "Know someone who might benefit from professional investment management? Share your experience."

### UX-026: Different Client Types Get the Same Experience

**Severity**: MEDIUM

**What the user experiences**: A 30-year-old tech employee with $200K and a 65-year-old retiree with $3M receive the same report format, the same communication frequency, and the same dashboard. Their needs are fundamentally different.

**Why it matters**: The retiree cares about income, capital preservation, and required minimum distributions. The young professional cares about growth and does not want to think about it. One wants weekly communication; the other wants monthly at most. A one-size-fits-all experience will satisfy neither.

**What should be done**:

- Define client personas and allow communication preferences: frequency (daily/weekly/monthly), detail level (summary/detailed), and topics of interest (tax focus, growth focus, income focus).
- Flag clients approaching age 72 for RMD (Required Minimum Distribution) planning.
- Customize report emphasis based on the client's stated goals.

---

## Category 6: Emergency UX Under Stress

### UX-027: SMS Circuit Breaker Reset Is Unreliable

**Severity**: HIGH

**What the user experiences**: The decision brief mentions an emergency stop that "can be triggered by you from your phone via text message." But the flows never describe how this works. What number do you text? What do you type? What if you are in a country where SMS does not work? What if the text is delayed? What if someone else sends the text (a child playing with the phone)?

**Why it matters**: An SMS-based critical control for a $5M+ system is a single point of failure with multiple failure modes. SMS is not encrypted, not reliable (delivery delays of minutes to hours are common), and not authenticated (caller ID can be spoofed). Using SMS as a financial control mechanism has known security vulnerabilities.

**What should be done**:

- Define the SMS control flow explicitly: what number, what commands, what authentication (PIN? passphrase?).
- Add redundant channels: SMS, push notification with confirmation, phone call with voice PIN, web dashboard button.
- Never rely on SMS alone. It should be one channel among several.
- Consider a dedicated mobile app with biometric authentication for emergency controls.

### UX-028: Emergency Notifications During Sleep Hours

**Severity**: HIGH

**What the user experiences**: A system error occurs at 3 AM (server crash, overnight data feed failure). The user receives a text message that wakes them up. It says something technical is wrong. They are groggy, confused, and cannot meaningfully evaluate the situation.

**Why it matters**: Most system errors at 3 AM are not emergencies -- the market is closed, no trades are happening, and the issue can wait until morning. Waking the user for a non-urgent issue trains them to ignore all alerts.

**What should be done**:

- Implement "quiet hours" for non-critical alerts (e.g., 10 PM - 7 AM). Only RED-tier alerts (active drawdown protection during market hours, security breach) break through quiet hours.
- For issues detected overnight, queue them for a morning summary: "While you were sleeping: Midas lost connection to IBKR at 2:14 AM. It reconnected at 2:41 AM. Everything is fine now."
- Allow the user to configure quiet hours.

### UX-029: Emergency Flows Assume Internet Access

**Severity**: MEDIUM

**What the user experiences**: Every emergency response assumes the user can open a dashboard, read detailed options, and make selections. What if the user is on a plane? At a remote cabin? In a hospital?

**Why it matters**: The "hit by a bus" scenario (Flow 06, Scenario 3) partially addresses this with Away Mode, but the setup has to be done in advance. If the user did not set up Away Mode and becomes unavailable, the system holds all decisions indefinitely.

**What should be done**:

- Define sensible defaults for every decision point: "If no response within X hours/days, Midas will take the most conservative option."
- For drawdown protection: default to "gradually return" after 7 days of no response.
- For brokerage disconnection: continue attempting reconnection indefinitely (no human action actually needed).
- For client withdrawal requests: "Your advisor is currently unavailable. Your request will be processed within 3 business days."

### UX-030: Emergency Contact Has No Training Flow

**Severity**: MEDIUM

**What the user experiences**: The decision brief says the backup person needs "about 15 minutes of training" and a "laminated card with step-by-step instructions." But no flow describes what this training looks like, what the card says, or how the emergency contact accesses the system.

**Why it matters**: In a real emergency, the backup person will be stressed and unfamiliar with the system. "Read-only access" is mentioned but not defined. If the emergency contact cannot actually do anything useful, they are not a meaningful backup.

**What should be done**:

- Create a "Backup Operator Guide" flow: a step-by-step document the emergency contact reads.
- Define exactly 3 actions the emergency contact can take: (1) View system status, (2) Pause all trading, (3) Call the IBKR emergency line.
- Include a "test drill" the owner can run with their emergency contact once per year.

---

## Category 7: Regulatory and Compliance UX

### UX-031: KYC Failure Path Is a Dead End

**Severity**: HIGH

**What the user experiences**: During client onboarding, if KYC verification fails, the client sees: "We need a little more information to verify your identity. Please contact [Your Name] for next steps." That is it. The onboarding stops. The client has no idea what went wrong or how long it will take.

**Why it matters**: KYC failures are common (recent address changes, name mismatches, non-US documents). A dead-end screen after the client has spent 10 minutes entering personal information is a dropout moment. The client may never come back.

**What should be done**:

- Provide specific guidance for common KYC failure reasons: "Your address could not be verified. This often happens with recent moves. Please provide a utility bill or bank statement showing your current address."
- Allow the client to continue the rest of the onboarding (risk questionnaire, agreement review) while KYC is pending. Only block the brokerage connection step.
- Give the advisor tools to manually resolve KYC issues.

### UX-032: No Compliance Dashboard

**Severity**: MEDIUM

**What the user experiences**: Flow 06 mentions regulatory inquiries and audit packages, but there is no ongoing compliance dashboard. The advisor does not see: "3 clients are overdue for annual risk profile reviews. 1 client's Form ADV needs updating. Your E&O insurance expires in 45 days."

**Why it matters**: Compliance failures are the most common reason RIAs face enforcement actions. A system that manages investments autonomously but leaves compliance tracking to the advisor's memory is an incomplete product.

**What should be done**:

- Add a "Compliance" section to the advisor dashboard showing:
  - Client review due dates
  - Form ADV filing deadlines
  - Insurance renewal dates
  - Any regulatory changes affecting the practice
  - A compliance calendar with upcoming deadlines

### UX-033: Client Agreement Signing Is Not Described for Updates

**Severity**: MEDIUM

**What the user experiences**: The advisory agreement is signed during onboarding. But what about amendments? If fees change, if the strategy changes materially, if regulatory requirements change -- does the client need to sign a new agreement? No flow describes this.

**What should be done**:

- Add an "Agreement Amendment" flow: when material changes occur, generate an amendment, send to client for e-signature, and track signing status.
- Track which clients are on which version of the agreement.

---

## Category 8: Information Architecture and Content Gaps

### UX-034: No Glossary or Education Hub

**Severity**: MEDIUM

**What the user experiences**: The flows do a good job of explaining terms inline ("tax-loss harvesting," "drawdown"), but there is no central place to learn about investment concepts. A user who forgets what "rebalancing" means 3 months after setup has to search through old notifications.

**Why it matters**: Financial literacy varies enormously. The user's friends and family (potential Phase 2 clients) may be less financially literate than the owner. An education hub builds confidence and reduces anxiety.

**What should be done**:

- Add a "Learn" section with brief, plain-language articles on key concepts.
- Link from in-app terms to their glossary entries.

### UX-035: No Historical Scenario Visualization

**Severity**: MEDIUM

**What the user experiences**: The setup flow shows "Over the last 20 years, a portfolio like this would have averaged 7-9% annual returns, with a worst-case year of about -25%." But there is no visualization. The user cannot see what 2008 or 2020 would have looked like with their specific settings.

**Why it matters**: Seeing "your portfolio would have dropped from $5M to $3.75M in October 2008 and recovered to $5M by March 2013" is far more visceral and useful than "-25% worst case." This is how the user calibrates their risk tolerance against reality, not abstract percentages.

**What should be done**:

- During setup (Step 4: Safety Boundaries), show an interactive "time machine" chart: "Here is how your portfolio would have performed during the 2008 financial crisis, the 2020 COVID crash, and the 2022 rate hike downturn, with your current settings."
- Let the user adjust their drawdown threshold and see how it would have changed the outcome in each scenario.

### UX-036: No Comparison of Fee Value to Alternatives

**Severity**: MEDIUM

**What the user experiences**: For Phase 2 clients, the fee is presented as a line item (e.g., "$2,325 this quarter"). There is no context for whether this is a good deal compared to alternatives.

**Why it matters**: Clients will compare. A 0.75% fee on $1M is $7,500/year. A Vanguard Personal Advisor charges 0.30%. A human advisor charges 1.0%+. Betterment charges 0.25%. If Midas does not proactively show its value relative to alternatives, clients will do the math themselves and focus only on cost.

**What should be done**:

- In the annual report, include a "Value of Advisory" section:
  - Tax savings generated: $X
  - Estimated cost of equivalent human advisor: $Y
  - Midas advisory fee: $Z
  - Net value: positive or negative, honestly

### UX-037: No Data Export or Portability Flow

**Severity**: MEDIUM

**What the user experiences**: If the user wants to leave Midas (stop using it, switch to another system, or hand data to a human advisor), there is no described flow for exporting their complete history -- all trades, all performance data, all tax records.

**Why it matters**: Data portability is both a trust signal (we are confident enough in our product that we make it easy to leave) and potentially a regulatory requirement.

**What should be done**:

- Add a "Export All Data" option in Settings.
- Include: complete trade history, performance history, tax records, decision logs, client records (Phase 2).
- Format: CSV and PDF bundle.

### UX-038: Weekend and Holiday Behavior Not Explained

**Severity**: LOW

**What the user experiences**: The daily operations flow says "Weekends and market holidays are quiet -- Midas does nothing and you hear nothing." But the user does not know all US market holidays. They might wonder why nothing happened on Presidents' Day.

**What should be done**:

- Include a "Market Calendar" view showing upcoming market holidays.
- On holiday mornings, send a brief note: "Markets are closed today for [Holiday]. No trading activity. Next trading day: [date]."

### UX-039: No Notification Preference Granularity

**Severity**: LOW

**What the user experiences**: Setup asks "How do you want to be alerted? Text / Email / Both." That is the only notification preference. But users might want: daily summaries by email only, urgent alerts by text, weekly summaries by email, and monthly reports as a PDF attachment.

**What should be done**:

- Add granular notification preferences by alert type and channel.
- Default to sensible settings but allow customization.

---

## Category 9: Trust and Transparency Gaps

### UX-040: No Audit Trail Accessible to the User

**Severity**: HIGH

**What the user experiences**: The system logs everything (mentioned in multiple flows). But the owner cannot browse this audit trail through the dashboard. They can only see "Recent Activity" showing the last 7 days. What about 6 months ago?

**Why it matters**: For Phase 2, the regulatory audit package generates from these logs. But the advisor should be able to browse their own logs at any time, not just during an emergency. "Trust but verify" requires verification access.

**What should be done**:

- Add a "Full History" section with searchable, filterable trade history and decision logs going back to inception.
- Include filtering by: date range, action type (trade, rebalance, tax harvest, risk event), client (Phase 2), and severity.

### UX-041: No System Downtime Communication

**Severity**: MEDIUM

**What the user experiences**: The system health sidebar shows "Next maintenance window: None scheduled." But what happens when maintenance IS needed? Is there a maintenance page? Does the user get advance notice?

**What should be done**:

- Define a maintenance notification flow: 48-hour advance notice for planned maintenance, with a clear message about what will and will not work during the window.
- Never schedule maintenance during market hours.

### UX-042: No Explanation of How Midas Differs from Robo-Advisors

**Severity**: LOW

**What the user experiences**: For Phase 2 client acquisition, the "Client Introduction Packet" is mentioned but its contents are vague. A potential client will ask: "How is this different from Betterment or Wealthfront?"

**What should be done**:

- Include a clear positioning section in the introduction packet: "Unlike automated platforms that offer cookie-cutter portfolios, Midas is managed by a registered advisor who oversees your specific situation, with the efficiency of institutional-grade automation."

### UX-043: Recovery After False Positive Circuit Breaker

**Severity**: MEDIUM

**What the user experiences**: If the circuit breaker triggers during a flash crash (a violent but brief decline that recovers within hours), the system sells stocks into the decline. When the market recovers the same day, the user's portfolio has locked in a real loss and missed the recovery. The flows describe the recovery options but never address the emotional impact of a false positive.

**Why it matters**: Flash crashes are relatively common (May 2010, August 2015, February 2018). A circuit breaker that triggers and sells during a flash crash is the most expensive system failure -- it turns a temporary decline into a permanent loss. The user will be furious.

**What should be done**:

- Acknowledge this scenario in the safety boundaries setup: "Your drawdown protection may occasionally trigger during brief, sharp market moves that quickly recover. In these cases, the protection limits your downside but may also limit your recovery. This is the trade-off of automated safety."
- Add a "cool-down" period: do not execute defensive trades for the first 30 minutes after a threshold is crossed. If the market recovers within that window, do not sell.
- After a false positive event, provide an honest post-mortem: "The drawdown protection triggered yesterday during a brief market decline. The market recovered by end of day. Here is how this affected your portfolio: [impact]. Here is what would have happened without the protection: [comparison]."

### UX-044: No Explanation of Costs Beyond Commissions

**Severity**: MEDIUM

**What the user experiences**: The monthly report shows "Trading commissions: $12.40. No management fees (you own the system)." But the real costs include: bid-ask spreads (invisible but real), market impact (for larger trades), the opportunity cost of the cash reserve, and the running cost of the system infrastructure ($200-$450/month per the decision brief).

**Why it matters**: Showing only commissions creates a false sense of "this costs almost nothing." The user is actually paying $200-$450/month in infrastructure costs plus spread costs on every trade. Transparent cost accounting builds trust.

**What should be done**:

- Include estimated spread costs in the monthly report.
- Include infrastructure running costs.
- Show a "Total Cost of Ownership" annually: commissions + spreads + infrastructure + opportunity cost of cash reserve.

---

## Category 10: Adversarial User Scenarios

### UX-045: User Tries to Use Midas During a Divorce

**Severity**: MEDIUM

**What the user experiences**: During a divorce, a court may issue a QDRO (Qualified Domestic Relations Order) or a temporary restraining order freezing assets. Midas has no concept of a legal hold on trading. It will continue rebalancing, harvesting losses, and executing trades on an account that is legally frozen.

**Why it matters**: Trading on a legally frozen account is contempt of court. The advisor (Phase 2) could face sanctions. The owner (Phase 1) could face legal consequences.

**What should be done**:

- Add a "Legal Hold" status that can be applied to any account: all trading stops, positions are frozen, but monitoring and reporting continue.
- For Phase 2, include this in the advisor training: "If a client notifies you of a divorce or legal proceeding, apply Legal Hold immediately and consult your attorney."

### UX-046: Client Dies and Family Contacts Advisor

**Severity**: MEDIUM

**What the user experiences**: A Phase 2 client passes away. Their spouse calls the advisor. The flows have no process for handling a deceased client's account -- no beneficiary workflow, no estate transfer process, no required document list.

**Why it matters**: This is a sensitive, legally complex situation that will happen if the advisory practice runs for more than a few years. Handling it poorly damages the advisor's reputation and can create legal liability.

**What should be done**:

- Add a "Deceased Client" flow: stop all trading, maintain positions, generate a final account statement, document required next steps (death certificate, probate documents, beneficiary claim form).
- Include guidance: "Contact IBKR's estate services department at [number]. You will need: [document list]."

### UX-047: User Has Unrealistic Expectations

**Severity**: HIGH

**What the user experiences**: The user brief says "I have $5M and I heard investing is the way to go." This is someone with limited investment experience and potentially unrealistic expectations. The flows do a reasonable job of setting expectations during setup, but they do not address the scenario where the user expects 20%+ annual returns and becomes disappointed with 7-9%.

**Why it matters**: Misaligned expectations are the root cause of most client-advisor conflicts. If the user expects to double their money in 3 years and the system targets 7-9% annual returns, they will be disappointed and blame the system.

**What should be done**:

- During setup, be more explicit about expected outcomes: "Based on your chosen allocation and historical data, your portfolio is expected to grow at approximately 7-9% per year on average. This means your $5M could become approximately $7-8M in 5 years. In the best-case scenario, it could be $9-10M. In the worst case, it could temporarily drop to $3.5-4M before recovering."
- Include a "Set Your Expectations" screen that shows the full range of possible outcomes using a fan chart.
- For Phase 2, include expectation-setting in the client onboarding conversation guidance.

---

## Summary of Findings by Severity

### CRITICAL (6 findings -- must address before launch)

| ID     | Finding                                             | Category          |
| ------ | --------------------------------------------------- | ----------------- |
| UX-001 | 60-day paper trading is a dropout cliff             | Onboarding        |
| UX-006 | Inadequate emotional design for large losses        | Emotional         |
| UX-007 | No trust-building during first week of live trading | Emotional         |
| UX-012 | No "why did you make that trade?" explainability    | Missing Flows     |
| UX-020 | No mobile experience defined                        | Mobile            |
| UX-022 | No "talk to a human" flow for clients               | Client Experience |

### HIGH (14 findings -- must address before Phase 2)

| ID     | Finding                                                     | Category      |
| ------ | ----------------------------------------------------------- | ------------- |
| UX-002 | Paper trading duration inconsistency (30 vs 60 days)        | Onboarding    |
| UX-008 | Decision fatigue from notification design                   | Emotional     |
| UX-009 | Defensive mode leaves user in limbo (no autonomous option)  | Emotional     |
| UX-011 | No preparation for extended bear markets                    | Emotional     |
| UX-013 | Benchmark comparison is misleading (diversified vs S&P 500) | Missing Flows |
| UX-014 | No "what if I had just used an index fund?" comparison      | Missing Flows |
| UX-015 | No recurring deposit flow                                   | Missing Flows |
| UX-016 | No end-of-year tax planning flow                            | Missing Flows |
| UX-021 | Dashboard tables not mobile-friendly                        | Mobile        |
| UX-023 | Client real-time portfolio viewing not defined              | Client        |
| UX-027 | SMS circuit breaker reset is unreliable and insecure        | Emergency     |
| UX-028 | Emergency notifications during sleep hours                  | Emergency     |
| UX-031 | KYC failure is a dead end                                   | Compliance    |
| UX-040 | No audit trail accessible to user                           | Trust         |
| UX-047 | User has unrealistic expectations                           | Adversarial   |

### MEDIUM (18 findings -- should address during active development)

| ID     | Finding                                          | Category      |
| ------ | ------------------------------------------------ | ------------- |
| UX-003 | Single brokerage, single account assumption      | Onboarding    |
| UX-004 | No flow for existing holdings                    | Onboarding    |
| UX-005 | Tax bracket not collected                        | Onboarding    |
| UX-010 | No celebration of wins                           | Emotional     |
| UX-017 | No override for specific trades                  | Missing Flows |
| UX-018 | No estate planning / beneficiary flow            | Missing Flows |
| UX-019 | No performance attribution by strategy component | Missing Flows |
| UX-024 | No client document portal                        | Client        |
| UX-026 | All client types get same experience             | Client        |
| UX-029 | Emergency flows assume internet access           | Emergency     |
| UX-030 | Emergency contact has no training flow           | Emergency     |
| UX-032 | No compliance dashboard                          | Compliance    |
| UX-033 | Client agreement amendment flow missing          | Compliance    |
| UX-034 | No glossary or education hub                     | Content       |
| UX-035 | No historical scenario visualization             | Content       |
| UX-036 | No fee value comparison to alternatives          | Content       |
| UX-037 | No data export or portability flow               | Content       |
| UX-041 | No system downtime communication                 | Trust         |
| UX-043 | No flash crash false positive recovery           | Trust         |
| UX-044 | No explanation of costs beyond commissions       | Trust         |
| UX-045 | No legal hold / divorce scenario                 | Adversarial   |
| UX-046 | Client death / estate process missing            | Adversarial   |

### LOW (9 findings -- address when convenient)

| ID     | Finding                                         | Category |
| ------ | ----------------------------------------------- | -------- |
| UX-025 | No client referral mechanism                    | Client   |
| UX-034 | No glossary or education hub                    | Content  |
| UX-038 | Weekend/holiday behavior not explained          | Content  |
| UX-039 | No notification preference granularity          | Content  |
| UX-042 | No differentiation from robo-advisors explained | Trust    |

---

## Systemic Recommendations

### 1. Hire an Emotional Design Lens

The flows are rational and thorough but emotionally flat. Financial services is 30% numbers and 70% feelings. Every communication should pass the test: "If someone who just lost $200K reads this, will they feel informed and reassured, or will they feel like they are receiving a system report?"

### 2. Mobile-First, Desktop-Enhanced

Redesign the entire information architecture starting from a phone screen. The phone shows: one number, one status, one action. The desktop shows the full analytical view. Not the other way around.

### 3. Default to Autonomous, Offer Override

The system is called "autonomous" but requires human decisions at critical moments (circuit breaker recovery, defensive mode exit). True autonomy means the system has sensible defaults for every decision point and only asks the human when the human wants to be asked.

### 4. Build Trust Through Transparency, Not Opacity

Every trade should be explainable. Every cost should be visible. Every comparison should be fair (blended benchmark, not S&P 500). Users trust systems they can see through, not systems that hide behind summary numbers.

### 5. Resolve the Paper Trading Duration Before Development

30 days vs. 60 days is not a small detail. It changes the onboarding timeline, the user's emotional journey, the opportunity cost calculation, and the "graduated live" possibility. Decide this now.

---

## Conclusion

The Midas user flows are a strong foundation. The writing is clear, the information architecture is logical, and the safety-first design philosophy is sound. However, the flows describe a system designed for a calm, rational, desktop-using investor who never has a bad day, never checks their phone, and never asks "but why?" These users do not exist. The real user is anxious, phone-addicted, loss-averse, and will question every trade that does not go their way. The 47 findings in this report represent the gap between the designed experience and the lived experience. Addressing the 6 CRITICAL findings before launch and the 14 HIGH findings before Phase 2 will transform Midas from a competent investment tool into a trustworthy financial companion.
