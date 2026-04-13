# Emergency Scenarios: When Things Go Seriously Wrong

**Who this is for**: You, the owner, when facing critical situations that go beyond normal intervention scenarios.

**The core idea**: These are the worst-case situations. They are unlikely on any given day, but over years of operation, some of them will happen. For each scenario, this flow describes what happens automatically, what you see, and what you need to do. Having a plan before the crisis is what separates a professional operation from a hobby.

---

## Scenario 1: Market Crash (2008/2020 Style -- 30%+ Drawdown)

### What is happening

The stock market is in freefall. Not a bad day -- a sustained crash. Think March 2020 (COVID: -34% in 23 trading days) or 2008 (Financial Crisis: -57% over 17 months). Your portfolio and your clients' portfolios are losing significant value rapidly.

### What Midas does automatically

**Phase 1: Drawdown protection triggers (first threshold crossed)**

Your drawdown protection was set at 15%. When the portfolio drops 15% from its peak, Midas automatically reduces equity exposure by 50%, moving proceeds to short-term bonds and cash. This happened for your portfolio and for any client whose individual drawdown threshold was crossed.

**Phase 2: Continued decline (beyond first threshold)**

If the market continues to fall after the defensive repositioning, Midas does NOT sell further. It holds the reduced position and monitors. The goal was to limit initial damage, not to time the bottom.

**Phase 3: Extreme volatility monitoring**

Midas watches for:

- Circuit breakers on exchanges (trading halts -- these happen automatically at the exchange level at -7%, -13%, and -20% daily declines)
- Brokerage system stability (high-volume days can overwhelm brokerage systems)
- Order execution quality (wide bid-ask spreads mean worse trade prices)

### What you receive

**First alert (drawdown protection trigger):**

> **URGENT: Drawdown Protection Activated**
>
> Your portfolio has fallen 15.3% from its peak.
> Current value: $4,740,000 (was $5,600,000 at peak)
>
> Midas has reduced stock exposure from 65% to 33%.
> Proceeds moved to short-term Treasury bonds and cash.
>
> Open Midas for details and options.

**If you have clients, a separate alert:**

> **ADVISOR ALERT: Client Drawdown Protections Triggered**
>
> 14 of 23 client portfolios have triggered their drawdown protections.
> Midas has automatically reduced equity exposure for each affected client.
>
> 9 clients with conservative portfolios have not triggered (bonds are holding up).
>
> Recommended action: Review client communications plan. Clients will be anxious. Proactive contact is strongly recommended.
>
> [View all client statuses] [Draft client communication]

### What you need to do

**For your own portfolio:**
The same decision as the regular circuit breaker scenario (stay defensive, gradually return, or return immediately). In a true crash, staying defensive is usually the prudent choice until the situation stabilizes.

**For your clients (this is the critical part):**

1. **Communicate proactively.** Do not wait for clients to call you in a panic. Midas helps you draft a communication:

> **Draft Client Communication**
>
> Midas has prepared a draft message for your review:
>
> "Dear [Client Name],
>
> The stock market has experienced a significant decline over the past [X] days. I want to reach out proactively to let you know what has happened with your portfolio and what steps we have taken.
>
> **Your portfolio:**
>
> - Current value: $[amount]
> - Change from peak: -[X]%
> - Your investments are safe and fully accounted for at Interactive Brokers
>
> **What we did:**
> Our risk management system automatically reduced your stock exposure when the decline reached your protection threshold. This limits further downside while keeping you positioned for recovery.
>
> **What happens next:**
> Markets have always recovered from crashes, though the timing is unpredictable. We will maintain the defensive position until conditions stabilize, then gradually return to your target allocation.
>
> I am available to discuss your portfolio at any time. Please do not hesitate to call or email.
>
> [Your Name]"
>
> [Send to all affected clients] [Customize per client] [Edit draft]

2. **Be available.** Clients will call. They will be worried. Your job is to be calm, factual, and reassuring. Midas provides talking points:

> **Client Talking Points:**
>
> - "Your investments are safe at Interactive Brokers. Nothing has been lost permanently unless we sell."
> - "We reduced your stock exposure automatically, which limits further losses."
> - "The market has recovered from every crash in history. The 2020 crash recovered fully in 5 months."
> - "Your portfolio is designed for this. Bonds and cash are providing stability."
> - "We are not making any panic decisions. We have a plan and we are following it."

3. **Document everything.** Midas automatically logs all trades, decisions, and communications during the crisis. This is important for compliance -- regulators will want to see that you acted reasonably and in clients' best interests.

### Decision points

- When to begin returning to normal allocation (for yourself and each client)
- Whether to contact each client individually or send a group communication
- Whether to adjust any client's strategy based on the new reality

---

## Scenario 2: Brokerage Goes Down During Market Hours

### What is happening

Interactive Brokers is experiencing a system-wide outage. You cannot see your accounts, and Midas cannot place trades. This happens occasionally -- even major brokerages have outages a few times a year.

### What Midas does automatically

1. Stops all trading activity immediately
2. Logs the outage start time
3. Monitors IBKR's status page for updates
4. Sends you an alert

### What you receive

> **SYSTEM ALERT: Brokerage Outage Detected**
>
> Interactive Brokers is not responding as of 11:45 AM Eastern.
> IBKR status page shows: "System disruption -- investigating"
>
> What this means:
>
> - Midas cannot place or monitor trades
> - All existing positions are safe (held at IBKR, not in Midas)
> - Any planned trades for today are paused
>
> What Midas is doing:
>
> - Monitoring IBKR status for updates
> - Will resume automatically when IBKR comes back online
> - Will reconcile all positions after reconnection
>
> What you should do:
>
> - No immediate action needed
> - If you have clients, consider sending a brief heads-up if the outage lasts more than 1 hour
> - Do NOT try to trade directly through IBKR's website (it is down too)

### If the outage happens during a critical moment (the market is crashing AND the brokerage is down)

This is the worst combination. Midas escalates the alert:

> **CRITICAL: Brokerage Down During Market Decline**
>
> The market is dropping and Midas cannot access Interactive Brokers.
>
> Current situation:
>
> - S&P 500 is down 4.2% today (based on last available data)
> - Midas cannot see your portfolio or place trades
> - Drawdown protection CANNOT execute until the brokerage is restored
>
> What you should know:
>
> - Your money is safe at IBKR regardless of whether we can access it
> - When IBKR comes back, Midas will immediately check your positions and execute any necessary protective trades
> - This situation, while stressful, is temporary
>
> If the outage lasts more than 2 hours during a significant decline, consider calling IBKR's client services directly: [phone number]

### Decision point

If the outage is prolonged (several hours) during a major market event, you may want to:

- Call IBKR directly to place orders by phone (their phone-based trading is sometimes available when electronic systems are down)
- Communicate with clients about the situation
- After the outage resolves, review whether any drawdown protections should have triggered and decide next steps

---

## Scenario 3: You Are Unavailable for Days

### What is happening

You are sick, traveling without internet, or otherwise unable to check on Midas for an extended period. This is the "hit by a bus" scenario -- what happens to your money and your clients' money if you cannot be reached?

### What Midas does automatically

Midas is designed to run without you. During your absence:

- Daily investment operations continue normally
- Rebalancing, tax-loss harvesting, and monitoring all run
- Drawdown protections are active and will trigger automatically
- Clients receive their regular reports
- System errors generate alerts (which you will see when you return)

### What changes when you are away

**Nothing, for routine operations.** This is the entire point of autonomous management.

**For situations requiring your judgment:**

- Client withdrawal requests will queue for your approval (the client receives: "Your request has been received and will be processed within 2 business days")
- New client onboarding will wait for your approval
- If drawdown protection triggers, Midas handles the defensive repositioning automatically but waits for your recovery decision

### What you should set up in advance

Midas has an "Away Mode" in settings:

> **Away Mode**
>
> If you will be unavailable for more than 2 days, configure your absence:
>
> Expected return date: ******\_\_\_******
>
> Auto-responses for client requests:
>
> - [ ] "I am currently unavailable and will respond by [return date]. For urgent matters, contact [backup contact]."
>
> Emergency contact (optional):
>
> - Name: ******\_\_\_******
> - Phone: ******\_\_\_******
> - This person can view (but not trade) client portfolios through a read-only login
>
> Escalation policy:
>
> - [ ] If drawdown protection triggers while I am away, send alerts to my emergency contact
> - [ ] If a system error persists for more than 4 hours, send alerts to my emergency contact

### Decision point (before you leave)

Set up Away Mode. Designate an emergency contact if possible. The system will function without you, but having someone who can at least see what is happening provides peace of mind.

---

## Scenario 4: Client Complains About Performance

### What is happening

A client is unhappy. Their portfolio is down, or it is up but not as much as the S&P 500, and they are questioning your value. This is an inevitability -- it will happen to every advisor.

### What Midas shows you when you open the client's account

An enhanced view with context:

> **Client Concern: [Client Name]**
>
> Their portfolio: +3.2% YTD
> S&P 500: +8.1% YTD
> Their benchmark (60/40): +5.4% YTD
>
> **Why the gap?**
>
> - Their Conservative portfolio (30% stocks) underperformed because stocks outperformed bonds significantly this year
> - Their portfolio did exactly what it was designed to do: moderate growth with lower risk
> - In the 2022 downturn, their portfolio fell -8% while the S&P fell -19%
>
> **Full picture:**
> | Period | Their Portfolio | S&P 500 | Their Benchmark |
> | ------------------- | --------------- | ------- | --------------- |
> | 2025 (full year) | +9.8% | +12.1% | +10.2% |
> | 2024 (full year) | +7.2% | +15.3% | +9.1% |
> | 2023 (full year) | +11.4% | +26.3% | +14.8% |
> | 2022 (bad year) | -8.1% | -19.4% | -12.3% |
> | Since inception | +21.8% | +34.2% | +22.1% |
> | Avg annual return | +6.8% | +9.4% | +7.0% |
>
> **Tax savings delivered:** $8,400 (tax-loss harvesting over their tenure)
> **Fees paid:** $6,200
> **Net value of advisory:** +$2,200 (tax savings minus fees)
>
> **Key point for the conversation:**
> Their portfolio is performing in line with its risk level. Comparing to the S&P 500 is comparing a sedan to a sports car -- different vehicles for different needs. They chose a conservative approach because they wanted stability. That stability protected them in down markets.

### What you do

Call the client. Use the data Midas provides to have an informed, honest conversation. Options:

1. **Reaffirm the strategy** -- their portfolio is doing exactly what it should. Show the 2022 comparison.
2. **Offer adjustment** -- if they truly want more growth, update their risk profile and shift to a more aggressive allocation. Make sure they understand the trade-off.
3. **Part ways** -- if they are fundamentally unhappy with the approach, it may be best for both of you if they move on. No hard feelings.

### Decision point

How to handle the relationship. Midas provides the data; you provide the judgment and empathy.

---

## Scenario 5: Regulatory Inquiry

### What is happening

You receive a letter or call from your state securities regulator or the SEC. They want to review your books, examine your trading practices, or ask about a specific client situation. This can happen for routine audits or specific complaints.

### What Midas provides

Midas maintains comprehensive records specifically for this purpose:

> **Compliance Records Available**
>
> [Generate Audit Package]
>
> This package includes:
>
> - Complete trade history for all clients (with timestamps and rationale for each trade)
> - Decision log (every investment decision the system made, with the data it used)
> - Risk management actions (every time a safety boundary was checked or triggered)
> - Client communications archive (every report, email, and message)
> - Fee calculation records (how each fee was computed)
> - Client onboarding records (KYC, risk questionnaires, signed agreements)
> - System uptime and error logs
>
> Date range: ******\_****** to ******\_******
> Client(s): [All] or [Select specific]
>
> Format: [PDF Bundle] [CSV Export] [Both]

### What you need to do

1. **Engage your compliance consultant or attorney.** Do not respond to a regulatory inquiry without professional guidance. Midas provides the data; a professional helps you present it.

2. **Generate the requested records.** Midas makes this easy -- everything is already logged and organized. The key advantage of an automated system is that every decision is documented with its reasoning. There are no gaps or "I don't remember" situations.

3. **Cooperate fully.** Regulators respond well to organized, complete records. The fact that Midas logs everything is a significant advantage in any inquiry.

### Decision point

When to engage counsel and which records to provide. Always err on the side of overcommunication with regulators.

---

## Scenario 6: Data Breach or Security Incident

### What is happening

You discover (or are notified) that there may have been unauthorized access to Midas, your brokerage account, or client data.

### What Midas does automatically (if it detects the breach)

1. **Immediately locks down.** All trading is suspended.
2. **Alerts you via every configured channel** (text, email, phone call if configured).
3. **Logs the incident** with as much forensic detail as possible (what was accessed, when, from where).

### What you receive

> **CRITICAL SECURITY ALERT**
>
> Midas has detected unusual activity:
>
> [Specific description, for example:]
>
> - An unrecognized IP address attempted to access your Midas dashboard
> - OR: Your brokerage API credentials may have been compromised
> - OR: Unusual login pattern detected
>
> What Midas did:
>
> - All trading has been suspended
> - Your brokerage API connection has been disconnected (as a precaution)
> - This incident has been logged
>
> IMMEDIATE ACTIONS REQUIRED:
>
> 1. Change your Midas password immediately
> 2. Change your Interactive Brokers password
> 3. Enable two-factor authentication everywhere if not already active
> 4. Review recent account activity at IBKR directly

### What you need to do

**For your own account:**

1. Change all passwords immediately
2. Review your IBKR account for unauthorized trades or withdrawals
3. Contact IBKR's security team if anything looks wrong
4. Review Midas's access logs for the timeline

**If client data may be affected:**

This is a serious legal situation. You likely have notification obligations:

> **Client Data Breach Protocol**
>
> If client personal information (SSN, financial data) may have been exposed, you are required by law to:
>
> 1. Notify affected clients within [timeframe per your state's breach notification law -- typically 30-60 days]
> 2. Notify your state's Attorney General office
> 3. Notify the SEC (if you are SEC-registered) via Form ADV amendment
> 4. Offer credit monitoring to affected clients
>
> Midas has prepared:
>
> - A list of all clients whose data is stored in the system
> - A log of exactly what data may have been accessed
> - A draft notification letter (consult your attorney before sending)
>
> **STRONGLY RECOMMENDED: Contact your attorney and compliance consultant immediately.**

### Decision point

This is not a scenario to handle alone. Your immediate priorities:

1. Stop the breach (change credentials, disconnect access)
2. Assess the damage (what was accessed?)
3. Engage professionals (attorney, compliance consultant, potentially a cybersecurity firm)
4. Notify affected parties (clients, regulators) per legal requirements
5. Fix the vulnerability

---

## Emergency Preparedness Checklist

Set these up BEFORE any emergency happens:

> **Your Emergency Readiness**
>
> - [ ] Emergency contact designated in Away Mode
> - [ ] Two-factor authentication enabled on Midas
> - [ ] Two-factor authentication enabled on IBKR
> - [ ] Attorney/compliance consultant identified (name and phone number)
> - [ ] E&O insurance active and current
> - [ ] Client communication templates reviewed and ready
> - [ ] Regular backups of all Midas data
> - [ ] IBKR phone trading number saved in your phone
> - [ ] Recovery plan documented (what to do if Midas system fails completely)

---

## Summary: Emergency Response Priorities

| Emergency           | First 5 Minutes        | First Hour                  | First Day                        |
| ------------------- | ---------------------- | --------------------------- | -------------------------------- |
| Market crash        | Review auto-defense    | Contact clients proactively | Decide recovery strategy         |
| Brokerage outage    | Verify it is IBKR-wide | Monitor IBKR status page    | Consider phone trading if needed |
| You are unavailable | (Away Mode handles it) | Emergency contact monitors  | System runs autonomously         |
| Client complaint    | Pull performance data  | Prepare honest comparison   | Have the conversation            |
| Regulatory inquiry  | Engage counsel         | Generate requested records  | Cooperate fully, organized       |
| Security breach     | Lock everything down   | Assess damage, change creds | Engage professionals, notify     |
