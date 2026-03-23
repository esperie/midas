# Midas Decision Brief

**Date**: 2026-03-23
**Purpose**: Help you decide whether to proceed with building Midas
**Reading time**: 5 minutes

---

## 1. What We Are Building

Midas is a system that invests your $5 million for you, automatically, without needing your daily attention. It buys diversified investments, watches for danger, saves you money on taxes, and can stop itself if something goes wrong. Once it proves itself on your own money, it can be expanded to manage investments for other people -- which becomes a business that earns fees on the money you manage.

---

## 2. How It Protects Your Money

This is the question that matters most, so here is exactly how the system keeps your money safe.

**It does not put all your eggs in one basket.** The system invests across many different types of assets -- US stocks, international stocks, bonds, real estate -- and enforces strict limits. No single investment can be more than 10% of your portfolio. No single industry can be more than 25%.

**It responds to drops gradually, not all at once.** If markets fall, the system reduces your exposure to stocks in stages -- 20% reduction at a 10% drop, 40% at 15%, 60% at 20%. This is designed to prevent the worst outcome: panic-selling everything at the bottom of a temporary dip and missing the recovery. Based on historical testing, this approach would have limited losses during the 2008 financial crisis to roughly 20-25% instead of the 45% that a simple "buy and hold" investor suffered.

**It has an emergency stop button.** If something goes seriously wrong -- a 25% loss, the system losing contact with the brokerage, data going stale, or anything out of the ordinary -- everything halts automatically. No more trades until a human (you or your designated backup) reviews the situation and gives the all-clear. This emergency stop can be triggered by the system automatically, by you from your phone via text message, or by a trusted backup person if you are unavailable.

**Even if the entire system goes offline, a safety net remains.** Standing safety orders sit at the brokerage itself, ready to sell if prices fall catastrophically. These work even if the Midas server is destroyed, your internet goes down, or the software crashes. They are the last line of defense.

**Nothing can override the safety limits.** The system uses a governance framework where each component has strict boundaries on what it is allowed to do. The part that researches strategies cannot place trades. The part that places trades cannot change its own limits. The risk monitor can stop everything but cannot start trades. These limits are enforced by the software architecture, not by hoping everything works correctly.

---

## 3. What It Costs

### To Build

**Phase 1 (your personal investment tool)**: Minimal out-of-pocket cost. The software is built using autonomous development sessions. Your main cost is time during the 60-day testing period before real money is deployed.

**Phase 2 (managing other people's money)**: $35,000-$95,000 in legal fees, insurance, and compliance costs. This is the cost of registering as an investment advisor (legally required) and getting liability insurance.

**Phase 3 (self-service platform)**: $65,000-$195,000 for institutional compliance (security audits, infrastructure hardening).

### To Run

|                                       | Monthly Cost   |
| ------------------------------------- | -------------- |
| Phase 1 (just your money)             | $200-$450      |
| Phase 2 (10-100 clients)              | $1,000-$2,700  |
| Phase 3 (100+ clients, full platform) | $3,000-$11,000 |

### Revenue (When Managing Other People's Money)

At a 0.50% annual fee (which is competitive for the level of service Midas provides):

- Managing $10 million (5-10 clients): $50,000/year
- Managing $50 million (20-50 clients): $200,000/year
- Managing $100 million: $350,000/year

The business becomes self-sustaining at roughly $20-30 million in managed assets.

### Value Even Without Clients

Tax optimization alone (finding investment losses to offset your tax bill, then immediately reinvesting) is estimated to save $10,000-$50,000 per year on a $5 million portfolio. This is money you save regardless of whether you ever take on clients.

---

## 4. What You Need to Decide

Here are the five most critical decisions, with recommendations.

**Decision 1: How much risk are you comfortable with?**

If your portfolio drops from $5 million to $3.75 million (a 25% loss), how do you feel? Sick to your stomach, or "markets go up and markets go down"? Your answer sets the most important parameter in the system -- when the automatic safety mechanisms kick in.

_Recommendation_: Set the maximum acceptable loss at 25%. This limits downside while avoiding the expensive mistake of the system selling too early during temporary dips.

**Decision 2: Are you serious about managing other people's money?**

Managing other people's money is a completely different undertaking from managing your own. It requires legal registration, liability insurance, compliance procedures, and fiduciary duty -- meaning you are legally obligated to act in their best interest, not yours. Violations carry personal criminal liability.

If you are serious, the legal process takes 3-6 months and costs $20,000-$50,000. Starting that process now (while the software is being built and tested) means you are ready when the software is ready. Waiting means adding 3-6 months to your timeline.

_Recommendation_: Build Phase 1 first (your personal tool). While it runs and proves itself over 6-12 months, decide whether the business opportunity justifies the regulatory investment. If you have 10 specific people who would give you money to manage, start the registration process now.

**Decision 3: How long should we test before using real money?**

The system will run in "practice mode" first -- making all the same decisions it would make with real money, but not actually executing any trades. This lets us verify that the safety mechanisms work, the strategy performs as expected, and the system handles unusual market conditions.

_Recommendation_: 60 days minimum. The original plan called for 30, but our risk analysis concluded that 30 days may not include enough market ups and downs to truly test the safety features. The cost of waiting an extra month is small compared to the cost of discovering a problem with real money.

**Decision 4: Do you have someone who can act as your backup?**

If the emergency stop triggers at 2 AM on a Saturday while you are on a flight, someone needs to be able to respond. This person does not need to understand investing -- they need a sealed envelope with login credentials and a laminated card with step-by-step instructions for three scenarios: reset the system, keep it halted, or call a specific phone number.

_Recommendation_: Designate someone you trust. A spouse, business partner, or close friend. They need about 15 minutes of training.

**Decision 5: What do you want the system to invest in?**

The system needs a starting point -- a target mix of investments. A common starting allocation for someone with a long time horizon and moderate risk tolerance:

- 40% US stocks (broad market)
- 15% International stocks
- 5% Emerging markets
- 15% Bonds
- 5% Real estate
- 10-20% Systematic strategy tilts
- 0-10% Cash reserve for opportunities

_Recommendation_: Start with a standard diversified allocation. The system can adjust over time as you see how it performs and as your preferences become clearer.

---

## 5. What Happens Next (If You Approve)

1. **You answer the five questions above.** This gives us the parameters to configure the system.

2. **We build the core system** (3 focused development sessions). At the end, you have a working system connected to your brokerage that can generate investment decisions, check them against safety rules, and execute trades.

3. **60 days of practice mode.** The system runs in full simulation -- making real decisions based on real market data, but not executing actual trades. You watch, we validate, and we tune the safety mechanisms.

4. **Go live.** After successful testing, the system begins managing your $5 million. You receive alerts on significant events and can view performance at any time. Your involvement drops to quarterly reviews unless the emergency stop triggers.

5. **6-12 months of real-money track record.** This is the proof period. Does the system perform as expected? Are the safety mechanisms working? Is the tax optimization delivering value?

6. **Decision point: pursue the business?** With a proven track record and (if you started early) regulatory registration complete, you can begin managing money for your first clients.

---

## 6. The Honest Risks

**The system will lose money sometimes.** Every investment strategy has losing periods. During the 2008 financial crisis, even diversified portfolios lost 30-45%. Midas's safety mechanisms are designed to limit losses to roughly 20-25% in the worst scenarios, but loss is inevitable. The question is not "will I ever lose money" but "will I lose less than I would without the system, and will I recover faster?"

**The active strategy might not beat a simple index fund.** Research consistently shows that most actively managed investments underperform simple index funds over long periods. Midas's strategy is designed to add value primarily through tax savings and risk management (where evidence is strong), not through predicting which stocks will go up (where evidence is weak). If the active components underperform for 12 months, the system automatically turns them off and falls back to simple index investing with tax optimization.

**Managing other people's money is a legal minefield.** If the system makes a mistake with a client's money, you are personally liable. "The computer did it" is not a legal defense. This is why the plan insists on liability insurance, compliance procedures, and a proven track record before accepting any client.

**You are the single point of failure.** Right now, you are the only person who can build, operate, and fix this system. If something happens to you, the system keeps running (it is designed to), but nobody can fix it if something goes wrong. The backup operator can hit the emergency stop but cannot diagnose problems. For Phase 2 (managing other people's money), this risk becomes untenable and must be addressed through documentation and potentially a technical partner.

**The technology is not the hard part.** Building the software is a solvable engineering problem. The hard part of the investment management business is earning people's trust with their money, navigating complex regulations, and demonstrating consistent performance over years. Phase 1 (your personal tool) is a technology project. Phase 2 and beyond is a business, and 80% of that challenge is non-technical.

---

## Summary

| Dimension                          | Assessment                                                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Personal investment tool (Phase 1) | Strong case. Tax savings alone justify the build cost. Full control and transparency.                              |
| Business viability (Phase 2+)      | Depends on finding specific clients and navigating regulation. Start the legal process early if serious.           |
| Risk management                    | Thorough. 88 risk items identified, all with concrete mitigations. Four layers of protection.                      |
| Build cost                         | Modest for Phase 1. $35K-$95K for Phase 2 (regulatory). $65K-$195K for Phase 3 (institutional).                    |
| Running cost                       | $200-$450/month for Phase 1. Self-sustaining at ~$20-30M AUM in Phase 2.                                           |
| Timeline to first deployment       | 3 development sessions + 60 days testing.                                                                          |
| Biggest risk                       | Drawdown protection whipsawing (selling at the bottom, missing the recovery). Mitigated by tiered response design. |

**The bottom line**: Building Phase 1 is a reasonable project with tangible value (tax savings, automation, control). The business case for Phase 2+ is credible but unproven -- it depends on whether you can find clients, navigate regulation, and demonstrate a track record. The plan is designed to let you prove Phase 1 before committing to Phase 2.

---

_To proceed, answer the five decisions in Section 4 and approve this plan. Implementation begins with Session 1: Core Infrastructure._
