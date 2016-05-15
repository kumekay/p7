A-B Testing
===========

Experiment Design
-----------------

### Metric Choice

> List which metrics you will use as invariant metrics and evaluation metrics here. (These should be the same metrics you chose in the "Choosing Invariant Metrics" and "Choosing Evaluation Metrics" quizzes.)
>
> For each metric, explain both why you did or did not use it as an invariant metric and why you did or did not use it as an evaluation metric. Also, state what results you will look for in your evaluation metrics in order to launch the experiment.

#### Used metrics

-	**Invariant:** Number of cookies, Number of clicks
-	**Evaluation:** Gross conversion, Retention, Net conversion

#### Selection reasoning

-	**Number of cookies:** Good invariant, used as unit of diverse.
-	**Number of user-ids:** We are using cookies as unit of diversion in this experiment, so this metric will not be robust invariant. Although it is possible to use user-ids as evaluation metric, it's not very good, because it's not normalized.
-	**Number of clicks:** Because this action happens right before our experiment this metric should be good invariant.
-	**Click-through-probability:** This metric is worthwhile as invariant. May be it even better than number of clicks, because it normalized for each group of users, but it's highly correlated with two other metrics that I already using, so I will not use it.
-	**Gross conversion:** This is important evaluation metric for our experiment, because it depends directly on number of enrollments.
-	**Retention:** Is a good evaluation metric, because number of paying users is may be affected and we have interest in this change.
-	**Net conversion:** Because we want to reduce the number of frustrated students and increase percent of paying users, this metric is obvious chose for evaluation.

#### Final decision making

As will be shown later **Retention** is hard to use, because to measure it we need too much page views (more than 4m), which will take too much time. So, in this experiment I will only use **Gross conversion** and **Net conversion** as evaluation metrics.

After the experiment, to make decision to launch the change

-	**Gross conversion:** Should significantly decrease (Number of trials should be lower)
-	**Net conversion:** Should not significantly decrease (Number of paying customers should not be lower)

### Measuring Standard Deviation

> List the standard deviation of each of your evaluation metrics. (These should be the answers from the "Calculating standard deviation" quiz.)
>
> For each of your evaluation metrics, indicate whether you think the analytic estimate would be comparable to the the empirical variability, or whether you expect them to be different (in which case it might be worth doing an empirical estimate if there is time). Briefly give your reasoning in each case.

-	**Gross conversion:** 0.0202
-	**Retention:** 0.0549
-	**Net conversion:** 0.0156

Empirical Standard deviation should be comparable to analytic estimate for Gross and Net conversion because unit of diversion and unit of analysis is the same - cookie. To calculate Retention we use user-id based number of enrollment, but analyze data on cookie based on cookie, so empirical estimate may differ from analytical.

### Sizing

#### Number of Samples vs. Power

> Indicate whether you will use the Bonferroni correction during your analysis phase, and give the number of pageviews you will need to power you experiment appropriately. (These should be the answers from the "Calculating Number of Pageviews" quiz.)

| Metric           | Value  | Std    | Std /5000 | Sample size | Probability | Pageviews |
|------------------|--------|--------|-----------|-------------|-------------|-----------|
| Gross conversion | 0.2063 | 0.0072 | 0.0202    | 25835       | 0.0800      | 645875    |
| Retention        | 0.5300 | 0.0194 | 0.0549    | 39115       | 0.0165      | 4741212   |
| Net conversion   | 0.1093 | 0.0055 | 0.0156    | 27413       | 0.0800      | 685325    |

I will not use Bonferroni correction, because we use just 2 metrics which are tied together (highly correlated) and we expect that both our metrics will have statistical significance, so it will be quite conservative to use Bonferroni correction.

#### Duration vs. Exposure

> Indicate what fraction of traffic you would divert to this experiment and, given this, how many days you would need to run the experiment. (These should be the answers from the "Choosing Duration and Exposure" quiz.)
>
> Give your reasoning for the fraction you chose to divert. How risky do you think this experiment would be for Udacity?

I'll launch this experiment on all traffic available. This experiment is not risky for users, they are not limited in their actions, also for business it should be quite safe Another moment, that it will take too much time to run this experiment on some fraction of traffic.

I'll not use Retention as metric, because it requires 4,7 millions pageviews to achieve level of statistical significance, and experiment will take 119 days. That's too long. Without this metric experiment can be finished in **18** days.

Experiment Analysis
-------------------

### Sanity Checks

> For each of your invariant metrics, give the 95% confidence interval for the value you expect to observe, the actual observed value, and whether the metric passes your sanity check. (These should be the answers from the "Sanity Checks" quiz.)
>
> For any sanity check that did not pass, explain your best guess as to what went wrong based on the day-by-day data. Do not proceed to the rest of the analysis unless all sanity checks pass.

|                 | Pageviews | Clicks |
|-----------------|-----------|--------|
| Control         | 345543    | 28378  |
| Experiment      | 344660    | 28325  |
| STD             | 0.0006    | 0.0021 |
| Margin of error | 0.0012    | 0.0041 |
| Lower bound     | 0.4988    | 0.4959 |
| Upper bound     | 0.5012    | 0.5041 |
| Observation     | 0.5006    | 0.5005 |
| Pass            | TRUE      | TRUE   |

### Result Analysis

#### Effect Size Tests

> For each of your evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Indicate whether each metric is statistically and practically significant. (These should be the answers from the "Effect Size Tests" quiz.)

|            | Pageviews | Clicks | Enrollments | Payments |
|------------|-----------|--------|-------------|----------|
| Control    | 212163    | 17293  | 3785        | 2033     |
| Experiment | 211362    | 17260  | 3423        | 1945     |

|                                                | Gross conversion | Net conversion |
|------------------------------------------------|------------------|----------------|
| Lower bound                                    | -0.0291          | -0.0116        |
| Upper bound                                    | -0.0120          | 0.0019         |
| d_min                                          | 0.01             | 0.0075         |
| Stat. significant ( 0 not in bounds)           | TRUE             | FALSE          |
| Practically significant ( d_min not in bounds) | TRUE             | FALSE          |

#### Sign Tests

> For each of your evaluation metrics, do a sign test using the day-by-day data, and report the p-value of the sign test and whether the result is statistically significant. (These should be the answers from the "Sign Tests" quiz.)

Total number of days: **23**

|                  | Days with positive change | P-value | Signicant (p < 0.025) |
|------------------|---------------------------|---------|-----------------------|
| Gross conversion | 4                         | 0.0026  | TRUE                  |
| Net conversion   | 10                        | 0.6776  | FALSE                 |

#### Summary

> State whether you used the Bonferroni correction, and explain why or why not. If there are any discrepancies between the effect size hypothesis tests and the sign tests, describe the discrepancy and why you think it arose.

All calculations for this project are presented in this spreadsheet https://docs.google.com/spreadsheets/d/17Gu5Zuhc_k5g28RHxch92jE3sBskZ-8pom95Y8wgY2w/edit?usp=sharing

I did not use Bonferroni correction because I have only two correlated metrics, and to make final decision to launch or not launch experiment, both our metrics should much expectations, so the risk to reject null hypothesis is still 5%. Bonferroni correction is designed for another type of errors, for example, it can help us if planned to launch experiment if one or another metric became statistically significant.

There are no discrepancies between effect size and sign tests.

### Recommendation

> Make a recommendation and briefly describe your reasoning.

I will not recommend to launch this experiment. **Gross conversion** significantly reduced, that is what we expected, but **Net conversion** didn't show both statistically and practically significant change, and more important that the confidence interval bounds are negative and the practical significance boundary (with minus sign) is included to these boundaries. It means, that there is some risk that launch of this change will decrease the net conversion metric (and revenue). Such change is not desirable for the company, and experiment is risky to launch.

Follow-Up Experiment
--------------------

> Give a high-level description of the follow up experiment you would run, what your hypothesis would be, what metrics you would want to measure, what your unit of diversion would be, and your reasoning for these choices.

Goals of any company are earn money by satisfying customer needs. I this experiment we tried to reduce distraction of users, who are going to enroll to trial but not going to spend a lot on time by studying. But we can work with other group of students, who auditing course for free, but spend more than 5 hours a week. If they decide to switch to non-free service, they can improve quality of learning and receive valuable certificate. For the company it can increase the revenue.

So, the null hypothesis is: Showing of "start trial screen" for students who have access to course materials and spend more than 5 hours a week will not increase retention.

Because we want to introduce this feature to user already signed to our coursed **unit of diversion** should be **user_id**. As **invariant metrics** we can use number of active **user_id's**, because it's our unit of diversion and **number of users clicking 'access course materials'**, because we measure it before the experiment stage.

As **evaluation metric** we can use overall **retention**, because we want to increase number of paying customers by this change

References
----------

-	http://www.had2know.com/academics/normal-distribution-table-z-scores.html
-	https://cran.r-project.org/web/packages/tigerstats/vignettes/qnorm.html
-	http://ncalculators.com/math-worksheets/calculate-standard-error.htm
-	https://en.wikipedia.org/wiki/Standard_deviation
-	http://www.had2know.com/academics/normal-distribution-table-z-scores.html
-	http://www.evanmiller.org/ab-testing/sample-size.html
-	http://graphpad.com/quickcalcs/binomial1.cfm
-	http://www.stat.berkeley.edu/~mgoldman/Section0402.pdf
-	http://www.utdallas.edu/~herve/Abdi-Bonferroni2007-pretty.pdf
