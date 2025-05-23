# A/B Testing udacity course learning notes

In below, before analysing any A/B test result (or after conducting an A/A test), we can first check if the data split (unit of diversion split) follows the intended proportion. If the split significantly deviates from the intended proportion, it can be due to (1) an infrastructure setup problem, (2) a failure in capturing user behavior during experiment, (3) unintended filters, (4) something is causing cookies to reset, (5) etc.

<img src="image/checking_invariants.png" width="700"/>

<br>

In below, hypothesis testing (this example doesn't mention the Practical Significance)

<img src="image/pooled_standard_error.png" width="700"/>

<br>

In below, significance vs sensitivity. Ignore the "α low"

<img src="image/sensitivity_1.png" width="700"/>

<img src="image/sensitivity_2.png" width="700"/>

<br>

In below, the assumption is to hold all else equal.

Setting a Practical Significance Level is how we express a minimum threshold below which something is not worth doing.

Here "increased practical significance level" means we are no longer interested in detecting small changes. Larger changes are easier to detect, and require fewer page views.

<img src="image/sensitivity_3.png" width="700"/>

<br>

In below, statistical significance 0.0202 is compared with the practical significance 0.02.

`m` stands for `margin of error` or `confidence interval`.

<img src="image/hypo_test_1.png" width="700"/>

<br>

In below,

- Case 2
    - No statistically significant change from 0 since the confidence interval includes 0
    - Confident that there's not a practically significant change
- Case 3
    - Statistically significant. Confident that there was a positive change
    - Not practically significant. Confident that there was not a practically significant change
    - In other words, confident there is a change, but don't care about the magnitude of the change
- Case 4
    - The confidence interval bounds are outside of what's practically significant
- Case 5
    - The point estimate is beyond what's practically significant. This change is an effect you care about
    - But the confidence interval overlaps 0, so there might not be a change
- Case 6
    - Guess is that there is a practically significant positive change
    - But possible the change is not practically significant

<img src="image/confidence_interval_cases.png" width="700"/>

<br>
<br>

Click metric definitions

- Def #1 (Cookie probability): For each `time interval`, number of cookies that click divided by number of cookies
- Def #2 (Pageview probability): For each `time interval`, number of pageviews with a click divided by number of pageviews
- Def #3 (Rate): Number of clicks divided by number of pageviews

<img src="image/click_metric_impact.png" width="700"/>

<br>
<br>

Percentiles are more useful than mean or median. Now we need to choose a percentile as the metric

- Take a few very similar population groups, and look at their, say, 75th percentiles. If the 75th percentiles vary a lot, then 75th percentiles is not robust enough.
- Take a few different population groups, and look at their, say, 75th percentiles. If the 75th percentiles do not vary much, then 75th percentiles is not sensitive enough.
- We need to choose a percentile that is both robust and sensitive

<br>
<br>

Bootstrapping

- From: https://youtu.be/Xz0x-8-cgaQ
    - Each bootstrapped dataset is created using sampling with replacement, so that it has the same number of data points as the original sample.
- From: https://youtu.be/O_Fj4q8lgmc
    - Why use Bootstrapping
        - When we do not have a large sample and cannot assume the sampling distribution (i.e. the distribution of the sample's mean) is normal.
        - When there isn't a formula to calculate the standard error of the estimate (e.g. 80th percentile).
    - Instead of calculating the standard error and then the confidence interval from a formula, we can observe it from Bootstrapping.
    - Yes, Bootstrapping is impacted by outliers, but using a formula (if there is one) is also impacted by outliers.
- Example usage: to construct a confidence interval for a non-normal distribution dataset. https://statisticsbyjim.com/hypothesis-testing/bootstrapping
- From: https://stats.stackexchange.com/questions/26088
    - Resampling is not done to provide an estimate of the population distribution -- we take our sample itself as a model of the population. Rather, resampling is done to provide an estimate of the sampling distribution of the sample statistic in question.
    - We usually have a hard time calculating the actual quantities of interest from that sample. So, we have to estimate them, and this is why we draw lots of bootstrap samples.

<br>
<br>

Unit of diversion

A unit of diversion is how we define what an individual subject is in the experiment.

<img src="image/unit_of_diversion_1.png" width="700"/>

The unit of analysis is whatever the denominator of the metric is. E.g. for click through rate, there is clicks divided by page views, then page view would be the unit of analysis.

When the unit of diversion is also a page view (e.g. the case in an event base diversion), then the analytically computed variability is likely to be very close to the empirically computed variability. If, however, the unit of diversion is a cookie or a user id, then the variability of the same metric click through rate can be much higher. In those cases the analytically computed variability is likely to be an under-estimate, and should use an empirically computed variability given the unit of diversion.

This is because the independence assumption is no longer valid for computing the variability analytically. When doing event-based diversion every single event is a different random draw, and so the independence assumption is actually valid. When doing cookie or user ID based diversion, the independence assumption is no longer valid because groups of events are diverted, and they are correlated together. This will increase the variability greatly.

In the following example, to get the same standard error, we need far more page views, if we divert by cookies instead of by page views.

<img src="image/unit_of_diversion_2.png" width="700"/>

<br>
<br>

In the example below, it doesn't mention practical significance. The probability difference would be significant, if the confidence interval (margin of error), around the probability difference, doesn't include 0.

The focus is to show although for global-wide experiments, the confidence interval may be narrower than local experiments due to larger sample size, however the local experiments' probability difference may be narrower due to more uniformed population.

<img src="image/global_exp_vs_local_exp_1.png" width="700"/>

<img src="image/global_exp_vs_local_exp_2.png" width="700"/>

<br>
<br>

From: https://www.evanmiller.org/how-not-to-run-an-ab-test.html

Decide on a sample size in advance and don't conclude before the experiment is over. Restrain yourself from stopping an experiment before it has run its course. Size the experiment in advance to ensure that there will have enough power when the experiment is over.

<br>
<br>

From: https://youtu.be/LCZG4wlEFXE and https://youtu.be/-QwMdwqUq9c

Another example of the process: 

1. Calculate (1) Standard Error and (2) diff `d` between the rate of control group and the rate of experiment group
2. Calculate Margin of Error
3. Surround the diff `d` with the Margin of Error, as a Confidence Interval
4. See if the Confidence Interval overlaps 0 (or, further, see if it overlaps the Practical Significance)

Also mentions a way to do paired day-by-day Sign Test

<br>
<br>

Estimated Variance

<img src="image/estimated_variance.png" width="700"/>

<br>
<br>

Simpson's Paradox

The cause of the Simpson's Paradox in this example is that there are lots of Experienced Users in the Experiment Group, causing the overall Click-through-rate of the Experiment Group to be low.

<img src="image/simpsons_paradox.png" width="700"/>

In A/B testing, Simpson's Paradox may be caused by: (1) something wrong with setup, (2) the change affects the two subgroups of users differently.






