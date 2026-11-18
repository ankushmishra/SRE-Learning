# Day 17 - Risk and error budgets

## Session goal

Calculate an error budget from a stated SLO and write a small learning policy
for how the lab should react when the budget is close to or beyond its limit.

Suggested time: 30 minutes reading, 15 minutes fundamentals, 60 minutes lab,
and 15 minutes completion check.

## 1. Fundamentals

Reliability has costs, and the appropriate target depends on user needs and
service risk. An error budget is the amount of unreliability allowed by an SLO
over its measurement window. It only makes sense when the SLI, denominator, and
window are explicit.

For a 99% success target over 1,000 requests, the allowed failures are 1% of
1,000, or 10. Eight failures leave two failures in the example budget. Twelve
failures exceed it by two. This arithmetic is a learning example, not a Google
production policy.

## 2. Practical lab

1. Reuse the Day 16 example: 1,000 eligible probes, 99% success.
2. Calculate the failure budget.
3. Compare an observed count of 8 failures, then 12 failures.
4. Write a sample lab rule: when the budget is exceeded, pause optional changes
   and focus on improving the lab's measurement or recovery steps.
5. Label the rule as your own exercise policy, not a universal SRE threshold.

Save the calculation and policy under `~/sre-followon-lab/notes/day17-budget.md`.

## 3. Completion check and notes

The budget is 10 failures. Eight failures leave 2; twelve exceed it by 2.
Explain why the denominator and window must be named, and what your lab policy
would do after an over-budget result.

- SLO and window:
- Allowed failures:
- Result for 8 failures:
- Result for 12 failures:
- My lab policy:
- Three things learned:
  1.
  2.
  3.

## Done when

- [ ] The error budget calculation is correct.
- [ ] I stated the SLI denominator and measurement window.
- [ ] My response rule is clearly labeled as a lab policy.

## Reading

[Google SRE Book - Embracing Risk](https://sre.google/sre-book/embracing-risk/): Managing Risk, Motivation for Error Budgets, and Forming Your Error Budget.

