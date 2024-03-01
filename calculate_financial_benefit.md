## Calculate the financial benefit of a new retention model over an existing model

`Monthly $ difference in benefit (changing from existing model to new)` =

`$ amount of profit per customer per year`

x `n number of customers per month that the company has capacity to treat`

x (`new_model's top n customers' precision` - `old model's top n customers' precision`)

x (`% of n customers who originally would churn but now stay after treatment` **+** `% of n customers who originally would stay but now churn after treatment`)

<br>

It is hard to calculate from historical statistics

`% of n customers who originally would churn but now stay after treatment` **+** `% of n customers who originally would stay but now churn after treatment`,

but it is equal to

`% of n customers who originally would churn but now stay after treatment` **-** `% of n customers who originally would stay but now churn after treatment`,

if `% of n customers who originally would stay but now churn after treatment` is small.

The later is easy to derive from historical statistics.

<br>
<br>

Also:

- `cumulative lift` = (`num positive in top n` / `n`) / (`num positive in all customers` / `num all customers`)
- `strike rate` = `precision of top n`
- `cumulative gains of top n` = `recall of top n`
