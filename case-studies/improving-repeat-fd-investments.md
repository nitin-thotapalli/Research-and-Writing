# Improving Repeat FD Investments (CLV): Root Cause Analysis

*Product Management Case Study | Fixed Deposits*

The link to the complete visual breakdown is available here : https://whimsical.com/thnitin-s/improving-repeat-fd-investments-root-cause-analysis-X6jYEXqktCqQYQioPWoXxC 

## 1. The situation

Imagine a WealthTech platform that helps people invest in FDs, bonds and other financial products. Customers are coming to the platform and making their first investment. But relatively few return to make another investment.

**The question: How can we increase repeat investments?**



## 2. Proposed solution

**Make it easier for customers who want to reinvest to move from a matured FD to their next suitable FD investment.**

The proposed experience has three parts:

1. **Before maturity:** Clearly communicate when the FD will mature, the expected payout and what choices the customer has.
2. **At maturity:** Show a few relevant FD options with understandable rates, tenures, bank names and important terms.
3. **When the customer decides to reinvest:** Provide a simple path to complete the next FD without unnecessary repeated steps.

This is **not** a recommendation to encourage everyone to invest again. Some people need their money, have already met their goal or prefer other investments. The intended customer is someone who **has funds available, wants another FD and may be getting stuck along the way**.

**Our decision:** Focus the proposed improvement on customers aged 30 to 50 whose FDs have matured. The reasoning behind this choice is explained below.

## 3. How we arrived at this recommendation

The analysis follows a simple sequence:

**Define the problem → Understand the customers → Look at possible reasons → Study two scenarios → Ask other teams → Make reasonable assumptions → Choose one problem → Test a solution.**

The purpose is to avoid building features before knowing **whether a customer has a reason to invest again and what is stopping them**.

## 4. Goal and starting assumptions

<img width="684" height="145" alt="image" src="https://github.com/user-attachments/assets/61bffcab-6661-4acc-bcf1-48e941827a62" />


### What are we trying to improve?

**Goal:** Increase the percentage of existing FD investors who complete at least one more FD investment within 12 months of their first investment.

For this exercise, we use a **sample repeat investment rate of 10%**. The improvement target would be set after checking the starting rate and the opportunity to improve it.

Why use a 12-month window? FD investments can remain active for several months. A short period could incorrectly label customers as lost when they simply have no reason to invest again yet. Twelve months is our **working measurement period**. In practice, the period should also reflect the tenures customers choose.

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



## 5. Understanding the problem

<img width="753" height="368" alt="image" src="https://github.com/user-attachments/assets/2e0dca92-5c4b-4ef9-90fa-2445ab75a09d" />


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



## 6. Who are we solving this for?

<img width="492" height="193" alt="image" src="https://github.com/user-attachments/assets/1742a046-408c-4a1f-9268-de117371b74f" />


For the exercise, we divided users into three broad age groups.

| Group | Working assumption | Priority |
| --- | --- | --- |
| **Group 2: Ages 30 to 50** | May balance family expenses, savings goals, safety and returns; some may have money available for another investment. | **1** |
| **Group 3: Ages 60+** | May have larger savings and care about income, bank trust or easy access to funds. Risk appetite varies by person. | 2 |
| **Group 1: Ages 18 to 30** | May have less spare money and invest smaller amounts. | 3 |

These groups help us organize the exercise, but age alone does not tell us someone's savings or risk preference. We would also use investment amount, tenure, goals and app activity to understand customers.

**Why select Group 2?** We are exploring customers who may have both the money and the intention to invest again. That makes this group useful for studying why a second FD does not happen. We would confirm the priority through customer and investment data.



## 7. What could stop a customer from investing again?

<img width="458" height="194" alt="image" src="https://github.com/user-attachments/assets/5353c071-0dff-4491-a995-c35ab147c993" />


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

 <img width="655" height="211" alt="image" src="https://github.com/user-attachments/assets/59ac5a85-2297-4cde-bf67-13442951f64f" />
  
Consider a customer from Group 2 who completed a first FD but has not invested again.

**Our line of questioning:**

1. **Why have they not invested again?** They may not have any extra savings set aside for another FD.
2. **Why might that be?** They may have already put their planned savings into the first FD.
3. **What does that mean?** They may invest again only when they save more money or when their current FD matures.

There are other possibilities: a different financial product, a better offer from another provider, or a different savings goal.

**What this scenario tells us:** The product may be working perfectly well, but the customer simply does not need another FD right now.

**Decision:** Do not prioritize pushing a second FD to customers who have no spare money or no investment need. This could create pressure without solving a real customer problem.


## 9. Scenario 2: The FD has matured, but no new FD was made

<img width="521" height="226" alt="image" src="https://github.com/user-attachments/assets/f6ffd0dc-7eda-4570-a761-1ae040228a5d" />

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

We would also check other explanations, including better rates elsewhere, trust concerns and customers who need their cash.



## 10. What we would ask other teams

<img width="500" height="248" alt="image" src="https://github.com/user-attachments/assets/77e8c1dd-9886-4589-90a3-b25fbed90fc1" />


Before recommending changes, we would request a few specific answers.

| Team | Questions | What the answers help us decide |
| --- | --- | --- |
| **Analytics** | How many customers return after maturity? How many view an FD, start investing and finish? How many reinvest within 30 days? | Whether the gap is in returning, choosing an FD or completing the investment. |
| **Marketing** | Do customers receive and notice maturity reminders? Are FD options explained clearly? | Whether customers know a new decision is available. |
| **Sales and Partnerships** | Are competitors offering more attractive rates or suitable FDs? Are there products customers request but cannot find? | Whether the platform's FD choices are competitive and relevant. |
| **Customer Support** | Why do customers say they did not invest again? Do they mention rates, trust, payout, KYC or payment problems? | What customers themselves say is stopping them. |
| **Product and Operations** | What does the customer see before and after maturity? Can they complete another FD without unnecessary steps or errors? | Whether the existing journey makes the second investment difficult. |

The goal is to get **just enough information to choose which problem is worth solving**, rather than collecting every possible metric.

  <img width="511" height="210" alt="image" src="https://github.com/user-attachments/assets/a2a3f691-7df1-4de5-89c2-78f36a45396d" />


## 11. Action plan

<img width="632" height="302" alt="image" src="https://github.com/user-attachments/assets/a42d4d07-e663-493e-b06f-1f5cdeb7f8c0" />


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



## 12. Tradeoffs

We are choosing where to focus, not claiming that one solution will work for every FD customer. These tradeoffs shape the proposed approach.

| Our choice | What we gain | What we give up or need to watch |
| --- | --- | --- |
| **Focus on matured FDs first** | Customers have reached a clear moment to decide what to do with their money. | We may miss people who want to open another FD before maturity. |
| **Show a few relevant FD options** | Comparing the next investment becomes easier. | Too few options may hide a better fit; suggestions must not prioritize rates over suitability. |
| **Use timely reminders** | Interested customers may notice their maturity date and options. | Too many messages can feel pushy. Reminders should be relevant and optional. |
| **Make the second investment simpler** | Remove avoidable steps for returning customers. | Keep necessary checks, clear terms and customer safeguards in place. |
| **Measure repeat investments** | We can see whether the new experience improves the intended behavior. | More investments alone do not show that customers made good financial decisions. |

## 13. Conclusion

**Our working root cause is an unclear path to a second FD for some customers who have reached maturity and already want to reinvest.**

The next step is to check the maturity journey and customer feedback, then test whether a clearer experience helps interested customers finish their next investment.

The important lesson is the approach: start with a broad business concern, separate customers who do not need another FD from those who do, identify the point where motivated customers get stuck, and test a small improvement before expanding it.

**A better repeat investment rate should come from helping customers make the right next decision, not simply getting them to return to the app more often.**
