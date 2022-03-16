## Rule of Three

If a certain event did not occur in a sample of `n`, the interval `[0, 3/n]` is a 95% confidence interval for the rate of occurrences in the population.

- https://en.wikipedia.org/wiki/Rule_of_three_(statistics)
- https://towardsdatascience.com/the-rule-of-three-calculating-the-probability-of-events-that-have-not-yet-occurred-106144dc2c39 (this one has a typo: `p` should be `>= 3/n`)
- https://jnd18.github.io/rule-of-3/

Notation:

- `p`: probability of something is true / occurs
- `1-p`: probability of something is false / doesn't occur
- `Pr(X=0)`: probability of seeing number of true / occurrence equals to 0, given `p`; we want it to be no more than 0.05

Derivation:

- `Pr(X=0) = ...probability mass function... = (1 - p) ** n <= 0.05`
- So `nln(1 - p) <= ln(0.05) approximately -3`
- For `p` close to 0, `ln(1 - p)` is approximately `-p`. Then we get `p >= 3/n`

Remarks:

- We allow `Pr(X=0)` up to 0.05, and want to find the 95% confidence interval of `p`.
- If `n` is small, we can allow a large `p` to exhaust this 0.05 allowance.
- If `n` is large, we can only allow a small `p` to exhaust this 0.05 allowance.
- The higher the `p`, the lower the `Pr(X=0)`, i.e. less likely we will get `X=0`.
- If `n` is fixed, if we have an allowance greater than 0.05, `p` is allowed to be smaller than `3/n` , thus a narrower CI for `p`.
- If `n` is fixed, if we have an allowance less than 0.05, `p` is allowed to be larger than `3/n`, thus a wider CI for `p`.
