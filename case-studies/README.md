# Improving Repeat FD Investments: Root Cause Analysis

*A product management practice case about understanding why customers do not make a second fixed deposit (FD) investment.*

**[View the complete mind map in Whimsical](https://whimsical.com/X6jYEXqktCqQYQioPWoXxC)**

> **About this case:** This is a learning exercise, not an analysis of actual Stable Money customer data. All customer counts, percentages, team responses and findings below are made up to show how I would approach the problem. The recommendation is a hypothesis to test, not a proven result.

## 1. The situation

Imagine a WealthTech platform that helps people invest in FDs, bonds and other financial products. Customers are coming to the platform and making their first investment. But relatively few return to make another investment.

**The question:** How can we increase repeat investments?

For this case, I am looking at **FDs only**, and at **people who have already completed their first FD investment**. Getting someone to sign up or complete their first KYC is a different problem.

My aim is not to guess a feature immediately. First, I want to understand whether a customer *needs* another FD, whether they *want* one, and what is stopping them if they do.

<!-- IMAGE: Add a screenshot of the case scenario and goal here, e.g. images/01-case-and-goal.png -->

## 2. Goal and starting assumptions

**Goal:** Increase the share of first-time FD investors who successfully make another FD investment within 12 months. We will choose a realistic improvement target once we understand the problem.

The following figures are **illustrative assumptions**:

| Starting point | Assumed number |
| --- | ---: |
| Registered users across the platform | 10,00,000 |
| People who made a first investment in any product | 1,00,000 |
| First-time FD investors (60% of those investors) | 60,000 |
| First-time bond investors (30%) | 30,000 |
| First-time investors in other products (10%) | 10,000 |
| FD investors who made another FD within 12 months | 6,000 |
| FD investors with no second FD recorded within 12 months | 54,000 |
| **Assumed 12-month repeat FD investment rate** | **10%** |

The 54,000 people are **not automatically inactive or unhappy**. Their first FD may still be running, they may have reached their savings goal, or they may prefer something else.

**How I would measure the rate**

**12-month repeat investment rate = (Number of first-time FD investors who complete at least one more FD within 12 months ÷ Number of first-time FD investors in that same group) × 100**

Using our assumed figures: **6,000 ÷ 60,000 × 100 = 10%.**

Only include customers who have had a **full 12 months since their first FD** when calculating this completed 12-month rate. Count each person only once, even if they invested several more times. For this exercise, another FD before the first matures *or* a successful renewal at maturity counts as a repeat investment. In a real product, I would also check how automatic renewals are recorded.

A second measure will matter later: **of customers whose FD has matured, how many invest again within 30 days?** That answers a different question from the overall 12-month rate.

<!-- IMAGE: Add your goal, assumptions and repeat investment formula here, e.g. images/02-goal-and-assumptions.png -->

## 3. Understanding the problem

Before looking for causes, these are the questions I would ask:

| Question | Why I am asking it |
| --- | --- |
| **How are we defining repeat investments?** Another FD before maturity, a renewal at maturity, or both? | These are different customer decisions. |
| **What is our current repeat investment rate?** How is it calculated, and over what period? | We need a fair starting point. |
| **Why is the current rate a problem?** Did it fall compared with the previous 12 months, or has it always been near 10%? | We should not assume 10% is bad without context. |
| **What do we want to improve, and by when?** More people investing again, more money invested, or both? | This tells us what success means. |
| **What do people do after their first investment?** Do they return, view FDs, start another investment, or stop midway? | This helps us see where interest may be lost. |
| **How long ago was their first FD?** Last week, three months ago, or a year ago? | Customers need a reasonable amount of time to invest again. |
| **Has their first FD matured?** If yes, did they reinvest or take the money out? | The need to reinvest can be very different at maturity. |

**A useful distinction:** Someone with an active 12-month FD who has not invested again is not the same as someone whose FD matured yesterday and who is looking for a new one.

<!-- IMAGE: Add the Understanding the Problem mind map here, e.g. images/03-understanding-the-problem.png -->

## 4. Who are the customers?

To make the exercise manageable, I grouped customers by age. These are **starting assumptions, not facts about people's risk appetite**.

| Group | Assumed behavior for this case | Priority |
| --- | --- | --- |
| **Group 1: Ages 18 to 30** | Lower savings available; may be careful with risk and invest smaller amounts. | 3 |
| **Group 2: Ages 30 to 50** | Balancing safety and returns, with family goals, expenses and some investable savings. | **1** |
| **Group 3: Ages 60+** | May have more capital; could care about regular income, access to money and bank trust. Risk preference needs to be checked, not assumed. | 2 |

**I chose Group 2 (30 to 50) for the exercise**, because it gives us a useful mix of savings needs, family goals and possible repeat investment opportunities. The priority is not backed by real data yet.

In actual research, I would make the ages non-overlapping, include the missing 51 to 59 group, and look beyond age at FD amount, tenure, investment goal and whether the customer has money available.

<!-- IMAGE: Add the customer groups mind map here, e.g. images/04-user-groups.png -->

## 5. What might stop someone from investing again?

I started by separating reasons **outside the app** from reasons **inside the app**.

| Outside the app | Inside the app |
| --- | --- |
| They need the money for family expenses or an emergency. | They did not notice their FD was about to mature. |
| Another bank offers a better rate or a more suitable FD. | They cannot easily find an FD that fits their needs. |
| Interest rates, taxes, policy or the economy have changed. | The investment steps are confusing or have KYC or payment issues. |
| They prefer another instrument, such as bonds or equities. | They cannot clearly see what to do after receiving their FD payout. |

This matters because not every reason can be solved with a new app feature. We should not persuade someone to lock away money they need for an emergency.

<!-- IMAGE: Add the internal and external causes diagram here, e.g. images/05-possible-causes.png -->

## 6. Scenario 1: The first FD is still running

Imagine a customer aged 30 to 50 who has invested once but has not opened another FD.

**A possible chain of reasoning:**

1. **Why haven't they invested again?** They have no spare money set aside for another FD.
2. **Why?** They put their planned savings into the first FD.
3. **Why does that matter?** They may only invest again when they save more money or when their first FD matures.

Other possibilities are worth checking: perhaps they chose bonds or equities, or another provider offered better rates, marketing or a better experience.

**What I take from this:** No second investment does not always mean the customer is unhappy. If they have no money available, making the app more attractive will not create more savings. I would not prioritize pushing another FD to this customer.

<!-- IMAGE: Add Scenario 1 here, e.g. images/06-scenario-1.png -->

## 7. Scenario 2: The FD has matured, but no new FD was made

Now imagine a customer aged 30 to 50 whose FD has matured. **For this scenario, assume they have money available and want to invest in another FD**, yet do not complete one.

I would first ask three simple questions:

- **Did they come back to the app after maturity?** If not, perhaps they missed a reminder.
- **Why didn't they choose another FD?** Perhaps the rates, bank or tenure did not fit their needs.
- **Did they start another investment but stop?** Perhaps KYC, payment or confusing steps got in the way.

### Following one possible cause with 5 Whys

This is *one possible explanation*, not a confirmed chain of events:

| Step | What might be happening? |
| --- | --- |
| **Why 1: Why didn't they reinvest?** | They took the matured FD money out instead. |
| **Why 2: Why did they take it out?** | They did not explore another FD on the platform. |
| **Why 3: Why didn't they explore one?** | They were unsure what to do after maturity. |
| **Why 4: Why were they unsure?** | They saw the payout but no clear next FD choice. |
| **Why 5: Why was there no clear next step?** | The experience appears to end when the first FD matures, instead of helping customers decide what comes next. |

**Possible root cause:** The path from FD maturity to the next suitable investment is not clear enough.

We would still need to rule out better rates elsewhere, the customer needing cash, or a genuine lack of investment options before accepting this explanation.

<!-- IMAGE: Add Scenario 2 and its 5 Whys here, e.g. images/07-scenario-2.png -->

## 8. What I would ask other teams

Before choosing a solution, I would ask for a small amount of focused evidence.

| Team | Questions I would ask |
| --- | --- |
| **Analytics** | How many people return after their first FD? How many with matured FDs invest again within 30 days? Where do interested customers stop: viewing, starting or paying? |
| **Marketing** | Did customers receive and notice the maturity reminder? Are we explaining the available FD choices clearly? |
| **Sales and Partnerships** | Are competing banks offering better rates or FD choices? Are customers asking for products we do not offer? |
| **Customer Support** | What do customers say when they do not reinvest? Are there complaints about payout, trust, KYC or payment? |
| **Product and Operations** | What does the customer see before and after maturity? Is starting a second FD easy and reliable? |

The purpose is to **check which explanation fits the customer**, rather than collecting data just because it is available.

<!-- IMAGE: Add the questions for other teams here, e.g. images/08-team-questions.png -->

## 9. Let's assume the teams come back with these answers

The numbers below are **made up for this practice case**. They are not survey results or company analytics.

**Scenario 1: First FD still running**

Out of 100 customers who did not invest again, **70 say they have no spare savings**. This suggests that many may simply not be ready for a second FD, even if the app works well.

**Scenario 2: FD has matured**

| Out of 100 customers whose FDs matured | People |
| --- | ---: |
| Made another FD investment | 40 |
| Did not reinvest because they needed cash or chose another bank | 35 |
| Wanted another FD but could not find a clear next step | 25 |
| **Total** | **100** |

This gives us a reason to investigate the **25 customers who wanted to invest but got stuck**. It does **not** mean a new feature will automatically convert all 25.

**My choice:** Focus on **Group 2, Scenario 2**: customers aged 30 to 50, with matured FDs, available money and an intention to reinvest.

Why? They already have a reason to invest. If the next step is genuinely unclear, that is something the product team may be able to improve.

If the evidence instead points mainly to poor rates or unsuitable FD choices, I would change this priority.

<!-- IMAGE: Add the assumed findings and chosen group here, e.g. images/09-assumed-findings.png -->

## 10. What I would do next

**Working recommendation:** Make it easier for customers who *want* another FD to understand their matured investment, see relevant choices and complete the next investment.

| When | Action | Why it could help |
| --- | --- | --- |
| **Before maturity** | Clearly show the payout date, expected amount and available next steps. | Customers know when a decision is coming. |
| **At maturity** | Show a few suitable FD options, including bank, rate, tenure and important terms. | Customers can decide whether another FD fits their needs. |
| **When investing again** | Make the next FD easy to start and complete, without repeating unnecessary steps. | Reduce avoidable confusion and abandonment. |
| **Communication** | Start with a clear in-app message; use optional push or email reminders. | Help customers find the information at the right time. |

I would always leave the option to **withdraw or do nothing**. Reinvesting should be the customer's choice.

### How I would test the idea

Try the improved experience with a small group of eligible customers and compare it with customers using the current experience.

- **Main measure:** Percentage of customers who make another FD within **30 days of maturity**.
- **Also check:** How many view an FD, start investing and finish; support complaints; payment failures; whether customers understood the terms and chose a suitable product.
- **Longer term:** Keep tracking the overall **12-month repeat FD investment rate** separately.

If more customers reinvest without worse customer outcomes, expand the experience. If the results do not improve, go back to the reasons: perhaps customers prefer better rates elsewhere, do not trust an issuer, or want a different way to save.

<!-- IMAGE: Add the Actionables mind map here, e.g. images/10-action-plan.png -->

## 11. Final takeaway

**My working root cause is an unclear journey from a matured FD to the next investment for customers who already want to reinvest.** It is a hypothesis supported only by the made-up findings in this case, not a proven diagnosis.

The lesson from this exercise is simple: **do not treat every customer who hasn't invested again as a lost customer.** First find out whether they have a reason and the means to invest. Then look at where those willing customers stop, and fix that specific problem.

The value of this case is the **approach to reaching a decision**, not claiming to have found the only correct answer.

---

**Complete visual analysis:** [Stable Money: Improving Repeat FD Investments, Root Cause Analysis (Whimsical)](https://whimsical.com/X6jYEXqktCqQYQioPWoXxC)

**Images:** I have left an image placeholder in each section so screenshots from the Whimsical board can be added later. Save the pictures in a case-studies/images folder and replace each placeholder comment with a normal Markdown image link.

[Back to the repository overview](../README.md)
