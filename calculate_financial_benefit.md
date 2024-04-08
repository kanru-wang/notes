## Calculate the financial benefit of a new retention model over an existing model

`Monthly $ difference in benefit (changing from existing model to new)`

=

`additional monthly profit (by changing from existing model to new) from customers who would churn if not treated`

x `reduction in churn prob for treated customers who would otherwise churn`

=

`$ amount of life time profit from each customer`

x `n number of customers that the company has capacity to treat per month`

x (`new_model's top n customers' precision` - `old model's top n customers' precision`)

x ((`churn rate of not treated customers` - `churn rate of treated customers`) / `churn rate of not treated customers`)

<br>
<br>

Also:

- `cumulative lift` = (`num positive in top n` / `n`) / (`num positive in all customers` / `num all customers`)
- `strike rate` = `precision of top n`
- `cumulative gains of top n` = `recall of top n`
