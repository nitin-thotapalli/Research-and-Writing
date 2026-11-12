# Improving Repeat FD Investments: Root Cause Analysis

*Product Management Case Study | Fixed Deposits*

## 1. The situation

Imagine a WealthTech platform that helps people invest in FDs, bonds and other financial products. Customers are coming to the platform and making their first investment. But relatively few return to make another investment.

**The question: How can we increase repeat investments?**

<!-- IMAGE: Add a screenshot of the case scenario here, e.g. images/01-case-and-goal.png -->

## 2. Proposed solution

**Make it easier for customers who want to reinvest to move from a matured FD to their next suitable FD investment.**

The proposed experience has three parts:

1. **Before maturity:** Clearly communicate when the FD will mature, the expected payout and what choices the customer has.
2. **At maturity:** Show a few relevant FD options with understandable rates, tenures, bank names and important terms.
3. **When the customer decides to reinvest:** Provide a simple path to complete the next FD without unnecessary repeated steps.

This is **not** a recommendation to encourage everyone to invest again. Some people need their money, have already met their goal or prefer other investments. The intended customer is someone who **has funds available, wants another FD and may be getting stuck along the way**.

**Our decision:** Focus the proposed improvement on customers aged 30 to 50 whose FDs have matured. This choice comes from the reasoning below, not from verified company research.

> **Evidence note:** This is an independent case study. The company figures, customer behaviors and team findings below are **illustrative assumptions**, not actual Stable Money data or measured product results. The solution is a proposal to validate, not a claim of proven impact.

## 3. How we arrived at this recommendation

The analysis follows a simple sequence:

**Define the problem → Understand the customers → Look at possible reasons → Study two scenarios → Ask other teams → Make reasonable assumptions → Choose one problem → Test a solution.**

The purpose is to avoid building features before knowing **whether a customer has a reason to invest again and what is stopping them**.

The complete visual breakdown is available in the [Whimsical mind map](https://whimsical.com/X6jYEXqktCqQYioPWoXxC).

## 4. Goal and starting assumptions

### What are we trying to improve?

**Goal:** Increase the percentage of existing FD investors who complete at least one more FD investment within 12 months of their first investment.

We begin with an **assumed rate of 10%**. The actual target should be set only after checking the real starting rate and how much improvement is realistically possible.

Why use a 12-month window? FD investments can remain active for several months. A short period could incorrectly label customers as lost when they simply have no reason to invest again yet. Twelve months is a **working choice for this case**, not proof that it is the best period for every FD tenure.

### Numbers used in this exercise

| Assumption | Example number | Why we use it |
| --- | ---: | --- |
| Total registered users | 10,00,000 | To picture the size of the platform, not as the repeat-rate denominator. |
| People who completed a first investment in any product | 1,00,000 | To narrow the analysis to customers who have invested at least once. |
| First-time FD investors | 60,000 | Our focus; assumed to be 60% of first-time investors. |
| First-time bond investors | 30,000 | Shows the platform has other products, which are outside this case. |
| First-time investors in other products | 10,000 | Completes our illustrative product split. |
| FD investors who make another FD within 12 months | 6,000 | The assumed number of repeat FD investors. |
| FD investors with no recorded second FD within 12 months | 54,000 | People to understand, **not automatically inactive customers**. |

These product groups are assumed to be separate for this simple example. In a real analysis, someone might invest in more than one product, so the data would need to account for that.

### How is repeat investment measured?

**12-month repeat FD investment rate = (Customers who complete another FD within 12 months ÷ Customers who completed their first FD) × 100**

Using our assumed figures:

**(6,000 ÷ 60,000) × 100 = 10%**

For a fair comparison, count each customer once and include only first-time FD investors who have had a **full 12 months** to invest again. Another FD before maturity and reinvestment at maturity both count in this practice case, but should also be studied separately.

Later, because we focus on maturity, we will also measure the **share of customers who reinvest within 30 days after their FD matures**. This is a more direct way to test the proposed solution.

<!-- IMAGE: Add the goal, assumptions and formula here, e.g. images/02-goal-and-assumptions.png -->

## 5. Understanding the problem

Before looking for a root cause, we need answers to seven questions.

| Question | Why it matters |
| --- | --- |
| **What counts as investing again?** Another FD while the first is running, renewal at maturity, or both? | These are different decisions and may have different reasons behind them. |
| **What is the current repeat investment rate?** How was it calculated, for whom and over what period? | We must know what the 10% actually represents. |
| **Why is the rate considered a problem?** Has it fallen compared with previous periods, or has it always been similar? | A low number alone does not prove that something has gone wrong. |
| **What outcome do we want, and by when?** More customers investing again, more total money invested, or both? | We need a clear business goal rather than a vague demand for more activity. |
| **What do customers do after their first FD?** Return to the app, look at another FD, start investing, or stop midway? | This shows where interested customers might be getting stuck. |
| **How long has it been since their first FD?** One week, three months or a year? | Someone who invested recently may not have had a meaningful chance to invest again. |
| **Has their first FD matured?** If so, did they reinvest or withdraw? | Maturity is a natural time when the customer has a fresh decision to make. |

The most important lesson here is that **not making a second FD is not the same as leaving the platform**. A customer with a 12-month FD may be satisfied and waiting for maturity.

<!-- IMAGE: Add the Understanding the Problem mind map here, e.g. images/03-understanding-the-problem.png -->

## 6. Who are we solving this for?

For the exercise, we divided users into three broad age groups.

| Group | Working assumption | Priority |
| --- | --- | --- |
| **Group 1: Ages 18 to 30** | May have less spare money and invest smaller amounts. | 3 |
| **Group 2: Ages 30 to 50** | May balance family expenses, savings goals, safety and returns; some may have money available for another investment. | **1** |
| **Group 3: Ages 60+** | May have larger savings and care about income, bank trust or easy access to funds. Risk appetite varies by person. | 2 |

These are **hypotheses for discussion, not facts about age groups**. They do not cover every age or guarantee how someone behaves. Real customer grouping would also use FD amount, tenure, savings needs, investment goals and app activity.

**Why select Group 2?** For this case, we assume some customers in this group have both a reason and the ability to invest again. That makes it useful for investigating why a second investment is not happening. This priority would need to be checked with real data.

<!-- IMAGE: Add the customer groups diagram here, e.g. images/04-user-groups.png -->

## 7. What could stop a customer from investing again?

We separated possible causes into two broad categories.

| Outside the app | Inside the app |
| --- | --- |
| The customer needs money for family expenses or an emergency. | They do not notice that their FD is about to mature. |
| Another bank has a better rate, tenure or offer. | They cannot easily find another FD that suits their needs. |
| Rates, taxes, policies or economic conditions have changed. | Investment steps are confusing, repetitive or fail during payment or verification. |
| They want to put savings into bonds, equities or another product. | The next step is unclear after the FD is paid out. |

**Why make this distinction?** Some reasons reflect a customer's genuine financial decision. Others may be issues with the product experience that we can improve.

The aim is not to eliminate every reason for not investing. It is to identify **where the platform is losing customers who actually want a second FD**.

<!-- IMAGE: Add the internal and external causes mind map here, e.g. images/05-possible-causes.png -->

## 8. Scenario 1: The first FD is still running

Consider a customer from Group 2 who completed a first FD but has not invested again.

**Our line of questioning:**

1. **Why have they not invested again?** They may not have any extra savings set aside for another FD.
2. **Why might that be?** They may have already put their planned savings into the first FD.
3. **What does that mean?** They may invest again only when they save more money or when their current FD matures.

There are other possibilities: a different financial product, a better offer from another provider, or a different savings goal.

**What this scenario tells us:** The product may be working perfectly well, but the customer simply does not need another FD right now.

**Decision:** Do not prioritize pushing a second FD to customers who have no spare money or no investment need. This could create pressure without solving a real customer problem.

<!-- IMAGE: Add Scenario 1 here, e.g. images/06-scenario-1.png -->

## 9. Scenario 2: The FD has matured, but no new FD was made

Now consider a different Group 2 customer. Their FD has matured. For this exercise, **assume they have money available and want another FD**, but have not completed one.

We start with three simple questions:

1. **Did they return to the app after maturity?** If not, perhaps they never noticed a useful reminder or next step.
2. **Why did they not choose another FD?** Perhaps the rate, tenure, bank or available choices did not fit their needs.
3. **Did they start another FD but stop?** Perhaps the steps were confusing or there was a KYC or payment issue.

### Going deeper with 5 Whys

To explore one *possible* explanation, we follow the unclear-next-step path.

| Question | Working explanation |
| --- | --- |
| **Why 1: Why was there no second FD?** | The customer took their matured FD money out without making another investment. |
| **Why 2: Why did they leave without another FD?** | They did not explore another FD on our platform. |
| **Why 3: Why did they not explore one?** | They were unsure what to do next after maturity. |
| **Why 4: Why were they unsure?** | They could see the payout but not a clear next investment choice. |
| **Why 5: Why was there no clear next step?** | The experience may be built mainly around making the first FD rather than helping with the decision after maturity. |

**Possible root cause:** A customer who wants another FD may not have a clear, easy route from maturity to their next investment.

This chain is a **hypothesis**, not a confirmed customer explanation. Better rates elsewhere, trust concerns and the need for cash must still be considered.

<!-- IMAGE: Add Scenario 2 and its 5 Whys here, e.g. images/07-scenario-2.png -->

## 10. What we would ask other teams

Before recommending changes, we would request a few specific answers.

| Team | Questions | What the answers help us decide |
| --- | --- | --- |
| **Analytics** | How many customers return after maturity? How many view an FD, start investing and finish? How many reinvest within 30 days? | Whether the gap is in returning, choosing an FD or completing the investment. |
| **Marketing** | Do customers receive and notice maturity reminders? Are FD options explained clearly? | Whether customers know a new decision is available. |
| **Sales and Partnerships** | Are competitors offering more attractive rates or suitable FDs? Are there products customers request but cannot find? | Whether the platform's FD choices are competitive and relevant. |
| **Customer Support** | Why do customers say they did not invest again? Do they mention rates, trust, payout, KYC or payment problems? | What customers themselves say is stopping them. |
| **Product and Operations** | What does the customer see before and after maturity? Can they complete another FD without unnecessary steps or errors? | Whether the existing journey makes the second investment difficult. |

The goal is to get **just enough information to choose which problem is worth solving**, rather than collecting every possible metric.

<!-- IMAGE: Add the team questions diagram here, e.g. images/08-team-questions.png -->

## 11. Illustrative findings and our decision

For this case study, let's assume the teams return the following **fictional findings**.

### Scenario 1: First FD still running

Out of 100 customers who have not invested again, **70 say they have no spare savings**.

**Interpretation:** For many of these customers, another FD may not be a relevant next action. Product reminders alone are unlikely to change that.

### Scenario 2: FD has matured

| What happened among 100 customers whose FDs matured? | Customers |
| --- | ---: |
| Made another FD investment | 40 |
| Did not reinvest because they needed cash or chose another bank | 35 |
| Wanted another FD but could not find a clear next step | 25 |
| **Total** | **100** |

These 100 customers are a **separate illustrative group** from the 100 in Scenario 1. This is not observed company data, and these figures cannot establish a cause by themselves.

**Our decision:** Focus on the **25 customers who wanted another FD but struggled to find the next step**.

**Why this group?** They already have investment intent. If their problem is genuinely an unclear experience, the product team may be able to remove that obstacle. We would still verify their behavior and speak with customers before building anything.

If customers instead tell us that the main issue is uncompetitive rates, then improving the screen alone will not solve the problem. We would reconsider the choice.

<!-- IMAGE: Add the hypothetical findings and chosen group here, e.g. images/09-assumed-findings.png -->

## 12. Action plan

Our proposed solution is a clearer FD maturity and reinvestment journey for customers who want another suitable FD.

| Step | Proposed action | Expected benefit |
| --- | --- | --- |
| **Before maturity** | Show maturity date, expected payout and the choices available after maturity. | Customers know when they need to make a decision. |
| **At maturity** | Present a few suitable FDs with rates, tenure, bank and key terms. | Customers can compare options without starting their search from scratch. |
| **During reinvestment** | Make it easy to start another FD, reducing repeated or unnecessary steps where possible. | Fewer interested customers abandon the process. |
| **Communication** | Use clear messages in the app, with optional push notifications or email reminders. | Reach customers when the information is useful, without excessive messaging. |

Customers should always be able to withdraw their money or choose not to reinvest. The aim is to help them make an informed decision, not to push an unsuitable product.

### How we would test the solution

Start with a small group of eligible customers whose FDs are approaching maturity.

- **Group A:** Continues with the current experience.
- **Group B:** Receives the clearer maturity information and simpler route to another FD.
- **Main measure:** Percentage of eligible customers who complete a new FD within **30 days after maturity**.
- **Additional checks:** How many view options, start and finish an investment; complaints, payment failures and whether people understood the terms.
- **Longer-term measure:** Track the overall **12-month repeat FD investment rate** separately.

Only expand the improved experience if it helps customers make suitable investments without causing new problems.

### What if the test does not work?

Return to the evidence. Customers may want better rates, have trust concerns, prefer another bank or need access to their money. If the reasons are different from our original assumption, **change the approach rather than sending more reminders**.

<!-- IMAGE: Add the Actionables mind map here, e.g. images/10-action-plan.png -->

## 13. Conclusion

**Our working root cause is an unclear path to a second FD for some customers who have reached maturity and already want to reinvest.**

That conclusion is supported by the **hypothetical conditions of this case study**, not by verified customer research. It would need validation before it becomes a real product decision.

The important lesson is the approach: start with a broad business concern, separate customers who do not need another FD from those who do, identify the point where motivated customers get stuck, and test a small improvement before expanding it.

**A better repeat investment rate should come from helping customers make the right next decision, not simply getting them to return to the app more often.**

---

**[Explore the complete RCA mind map in Whimsical](https://whimsical.com/X6jYEXqktCqQYioPWoXxC)**

*Images can be added under `case-studies/images/` by replacing each image comment with a Markdown image link.*

[Back to Case Studies](README.md) · [Repository overview](../README.md)
