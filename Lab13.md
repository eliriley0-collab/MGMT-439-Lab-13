# Lab 13 — Portfolio Mandate Defense

AI-assisted draft, prepared after I reported that the instructor’s follow-up email permits AI help with the written sections. The email itself was not supplied in this chat.

## D — Common question

Which of these portfolios does the Meridian Foundation's mandate permit — and what must the CIO answer before anyone optimizes?

## R — Statement sorting

A passes every checkable rule. B fails three rules, and C fails one. Passing these checks does not establish that a portfolio satisfies the entire mandate.

| Statement | Type | Reason |
| --- | --- | --- |
| S1: Horizon seven years; annual rebalancing. | Hard constraint | This sets the planning horizon and rebalancing schedule. One set of weights cannot prove the schedule is followed. |
| S2: Objective: CPI + 4% a year, net of costs, over rolling five-year periods. | Objective | This is the return target, not a guaranteed result. |
| S3: Pays out $0.75 million every quarter; twelve months of payouts held in cash or daily-liquid government securities. | Hard constraint | Four payouts need $3 million, or 3% of the reserve, in eligible holdings. |
| S4: Benchmark: 60% global equities, 40% U.S. investment-grade bonds. | Preference / reference benchmark | It gives us a comparison point. It does not require every portfolio to hold exactly 60/40. |
| S5: Long-only: no borrowing, short sales or derivatives. | Hard constraint | Those activities are prohibited. For this holdings list, the checker tests nonnegative weights and full allocation. |
| S6: No single issuer above 8%. | Hard constraint | One company’s securities count together, with the supplied exemptions for cash, Treasuries and diversified funds. |
| S7: Annual turnover at most 25% without committee approval. | Hard constraint | Going above 25% needs approval. Checking it requires trading history and a turnover definition. |
| S8: At least 90% redeemable within five business days. | Hard constraint | We can add holdings with redemption times of five days or less and compare the total with 90%. |
| S9: “The reserve cannot have a calendar-year loss greater than 10%.” No measure, no breach rule. | Unverifiable promise as written | There is no defined loss measure or breach response, and weights cannot guarantee a future loss limit. |
| S10: No sector, ESG, tax or home-bias preference; nobody has said what the pilot should optimize. | Missing | There are no extra preferences in those areas, but the optimization criterion is still missing. |

S4 is closest to the preference category, but it is really a performance reference. I would not turn it into a required allocation without the CIO saying so.

## I / V — AI-assisted checker and verification

Run `python3 mandate.py` from this directory (`python mandate.py` also works if your Python command points to Python 3).

Inputs use USD millions and percentage points. Reserve minimum is computed as `4 * 0.75 / 100 * 100 = 3.0%`. The holdings table explicitly makes the 10-year Treasury reserve-eligible. Acme stock and bonds are aggregated by issuer. The calendar-year loss statement is never tested.

Execution evidence is in [verification.txt](verification.txt), including A–D and the negative-weight probe. The probe modifies an in-memory copy of A: Cash −2, global equity 64. The saved A inputs remain restored at Cash 2, global equity 60.

These checks do not establish the return objective, future losses, annual turnover, or rebalancing compliance. Those require definitions, time-series data or trading history beyond these static weights. Passing is conditional on the rules actually checked.

## E — AI-proposed attack

Candidate D: 100% Cash, every other holding 0%. It passes all five implemented checks. It exposes S2: there is no evidence this allocation can meet CPI + 4% net over rolling five-year periods. The CIO could reasonably reject it for lack of support for the stated return objective. This is an objection to suitability, not a claim that future cash returns are known. No optimization was performed.

### Judgment and sceptical colleague role

The objection holds because passing these checks gives us no reason to expect an all-cash portfolio to meet the return objective. S2 decides it, although this does not prove cash must miss the target in every future period.

Sceptical colleague role: I would cite S2 against D and ask for evidence that it can meet the return objective. S10 also leaves us without a chosen optimization criterion.

## One question for the CIO

One question that must be answered before optimization:

With S2 as the return target, should the pilot minimize expected volatility while targeting CPI + 4% net, or maximize expected net return subject to the agreed constraints?

The answer that would change your portfolio, and the resulting design change:

The answer changes the design. Minimizing volatility would favor the lowest modeled risk among portfolios expected to meet the target. Maximizing expected return could accept more risk within the approved limits. These are proposed choices for the CIO, not rules already in the mandate. S9 still needs its own definition before we can enforce it.

## Investment policy statement

An investment policy statement is the written agreement for how the client’s money should be managed. It states the goal, the limits and how results will be reviewed.

The objective tells us what we are trying to achieve. Constraints limit the choices we can use to get there. A benchmark helps compare results but does not automatically tell us what to optimize.

For Meridian, S2 is the return objective and S4 is the benchmark. S1 sets the horizon and rebalancing schedule. S3, S5, S6, S7 and S8 set the payout, trading, issuer, turnover and liquidity limits. S9 needs clarification before it can become a usable risk rule. S10 records the missing optimization criterion and the absence of extra preferences.

## Reflection draft

A portfolio can pass every checkable rule and still be wrong because the checker covers only part of the mandate. It does not show that the portfolio can meet the return objective, follow annual rebalancing, meet turnover limits or stay within a future loss limit. D shows the gap: passing the checker alone does not support choosing it for Meridian.

S4 is the statement I would question most in this AI-assisted reflection. The 60/40 benchmark can sound like required weights, but the statement only calls it a benchmark. It stays a comparison point unless the CIO makes it an allocation rule. S4 was selected with AI help rather than recorded as an unaided personal reaction.

## Code comprehension quiz

Topic reviewed: company issuer aggregation, implemented by `companies[issuer] += weight`. The student did not identify a least-understood line; the AI selected this topic.

- AI question: Why must the checker add Acme's bond and stock weights together, and what total does that produce for candidate B?
- Student answer: "idk total was 100 tho"
- AI explanation: 100% is the whole portfolio. B's Acme exposure is 5% bonds + 4% stock = 9%, exceeding the 8% company limit.
- Follow-up question: Does 3% Acme bonds + 4% Acme stock exceed the 8% issuer limit?
- Student answer: "no"
- Feedback: Correct; the combined 7% passes the issuer check.

## Project 2 — Apple

I want to evaluate Apple stock (AAPL) as one holding in a portfolio. For this project, I am using a hypothetical individual client with a $100,000 investment portfolio and a seven-year horizon. These are AI-assisted case assumptions selected for the project, not facts about my finances or a real client.

**Objective:** Seek annualized total returns above CPI by 3 percentage points, net of costs, over rolling five-year periods. This is a goal, not a guaranteed return. Evaluate whether Apple can help meet it as part of the portfolio; choosing Apple as the research subject does not require buying it.

**Hard constraints:**

- Start with $100,000 and fully allocate the portfolio; all weights must total 100%.
- Use long-only positions with no borrowing, short sales or derivatives.
- Keep at least 5% in cash or Treasury bills for unexpected withdrawals.
- Keep at least 95% redeemable within five business days.
- Limit Apple to 10% of the total portfolio. Apply the same 10% cap to each other individual company, combining its stock and bonds. Cash, U.S. Treasuries and diversified funds are exempt from the company cap.
- Review and rebalance annually over the seven-year horizon, subject to the other constraints. No scheduled payouts are assumed.

**One question for the client:** Should the portfolio minimize expected volatility while targeting CPI + 3% net, or maximize expected net return within these constraints?

**Answer that changes the design:** If the client chooses minimum volatility, the model would seek the lowest estimated risk among portfolios expected to meet the target. If the client chooses maximum expected return, the model could select more risk within the limits. Apple's weight could be anywhere from 0% to 10% depending on that answer and the analysis. If the target cannot be met within the constraints, I would report that instead of quietly changing the rules.

These assumptions define the classroom case. They would need confirmation before being used for a real client.

## GitHub checkout

Upload this file, mandate.py and verification.txt to the course repository. Paste the GitHub links for the Markdown and Python files into the Lab 13 quiz. No GitHub push or quiz submission has been made from this chat.
