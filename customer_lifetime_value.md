## Customer Lifetime Value Estimation

- Customer Lifetime Value (CLV) can be estimated by Markov Chain Model.
- CLV is estimated for every customer category, instead of for every customer. Every customer belongs to a certain customer category.
- Need to decide an Initial Date and a Final Date that are 12 months apart. Notice that 12 months' data needs to be available after the Final Date.

    `Initial Date <----- 12 months -----> Final Date <----- 12 months -----> Recent`

- CLV is calculated using 2 components, a Revenue Matrix of size `[s, 1]`, and a Transition Matrix of size `[s, s]`. The number of states (categories) is denoted by `s`.
    - The state n years later would be the Transition Matrix self-multiplied `n` times.
    - Multiply the resulting `[s, s]` state matrix (n years later) with the Revenue Matrix `[s, 1]` to get a `[s, 1]` year-n probability-based Revenue Matrix.
    - Discount (divide) the year-n probability-based Revenue Matrix by `(1 + discount_rate) ** n` to get the present value of it.
    - Assume that we consider five years' customer value (not really "lifetime"), then sum all five probability-based Revenue Matrices to get one CLV lookup table (of size `[s, 1]`).
    - Join the CLV lookup table to each customer according to their category.
- Categorise customers and estimate a Transition Matrix in either way:
  - By individual’s product usage mix (e.g., a category for customers who only use Product B and D).
    - Rare product usage mixes are ignored.
    - The Transition Matrix is estimated by applying a crosstab on two snapshots of customer product usage mix, one on the Initial Date and the other on the Final Date.
  - By individual’s:

    1. average loan balance size (or purchase size) over a period.

        x
    
    2. total transaction count over a period.
       - If there are 3 bands of average loan balance size and 3 bands of total transaction count, there would be 3 x 3 = 9 total categories.
       - Need to make two measurements, on two 12-month periods, regarding to which of the 9 categories each customer belongs; i.e., the 12 months after the Initial Date, and the 12 months after the Final Date.
       - The Transition Matrix is estimated by applying a crosstab on the two 12-month measurements.
  - Common product usage mix strings, or bin edges (for categorising any transaction count or any loan balance) are determined based on the 12-month period between Initial Date and Final Date.
- The Revenue (or Profit) Matrix is estimated by:
  - For each customer, calculate the total revenue over the 12 months after the Initial Date.
  - Then for each category, calculate the average revenue per customer.
  - The revenue of each customer is not the customer’s actual revenue, but the average revenue of the category under which the customer falls. The benefits are: (1) conceptual simplicity for not adding 1st year’s actual revenue to future years’ average revenue, and (2) less overfitting.
- There will be a concept drift, if:
  - The relationship between categories and the average revenue of each category is outdated.
  - The Transition Matrix is outdated.
- What are saved during train and loaded during inference?
  - Two lists of bin edges (for categorising any transaction count and any loan balance), or a list of all the non-rare product-mix strings.
  - Transition Matrix, shape `[s, s]`.
  - Revenue Matrix, shape `[s, 1]`.
  - Ordered segment labels, shape `[s, 1]`, to turn the product of Transition Matrix and Revenue Matrix into a dataframe.
- Training / Fitting process
  - Create two history dataframes and two snapshot dataframes:
    - [History Dataframe 1] / [History Dataframe 2] is one row per customer per month; include any customers who appeared in the 12 months period between [Initial Date and Final Date] / [Final Date and Recent].
    - [Snapshot Dataframe 1] / [Snapshot Dataframe 2] is one row per customer, on the [Initial Date] / [Final Date].
    - For the Product Usage Mix model, we do not need a History Dataframe 2 (or to ensure it has 12 months), because we only need a Snapshot Dataframe 2.
  - History dataframes aggregate by ID and then:
    - For the Product Usage Mix model, calculate "total revenue per customer" and "number of rows" (only needed for History Dataframe 1).
    - For the 9 Category model, calculate "total revenue per customer" (only needed for History Dataframe 1), and also "average loan balance size", "total transaction count" and "number of rows" (for both History Dataframe 1 and 2).
    - Filter to only customers who have 12 rows (i.e., exists for the full 12 months, so the summation is meaningful).
  - Join the aggregated dataframes to the snapshot dataframes of the same period, on customer ID, resulting in Combined Dataframe 1, or Combined Dataframe 2 as well.
  - Define categories:
    - For the Product Usage Mix model, fitting is to find all common product usage mixes; fitting is only on the [Snapshot] / [Combined] Dataframe 1. Transforming is to add a category column to each of the [Snapshot] / [Combined] Dataframe 1 and 2.
    - For the 9 Category model, fitting is to find the bin edges of "average loan balance size" and "total transaction count"; fitting is only on Combined Dataframe 1. Transforming is to add a category column to each of the Combined Dataframe 1 and 2.
  - Join [Snapshot] / [Combined] Dataframe 1 and 2 on customer ID.
  - Calculate the Transition Matrix and Revenue Matrix.
- Inference process
  - No need to have inference code that calculates the sum of revenue per customer, nor the average revenue of each category.
  - For the Product Usage Mix model, get the most recent month's customer data, `c` number of unique customers result in `c` rows. For the 9 Category model, get the last 12 months' customer data, `c` number of unique customers result in `12c` rows.
  - (After aggregation and calculation if needed) Assign a category column to the customer dataframe.
  - Calculate the CLV lookup table (of size `[s, 1]`), given `discount_rate` and the estimation horizon.
  - Join the customer dataframe and the CLV lookup table by each customer's category.
