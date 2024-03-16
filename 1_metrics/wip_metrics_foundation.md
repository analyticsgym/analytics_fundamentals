------------------------------------------------------------------------
------------------------------------------------------------------------
------------------------------------------------------------------------
------------------------------------------------------------------------
------------------------------------------------------------------------
------------------------------------------------------------------------

# WORK IN-PROGRESS

------------------------------------------------------------------------
------------------------------------------------------------------------
------------------------------------------------------------------------
------------------------------------------------------------------------
------------------------------------------------------------------------
------------------------------------------------------------------------

# Metrics Overview

### Why use quantifiable metrics?

-   Metrics help organizations systematically measure, assess, and improve performance

### Metric benefits and pitfalls

##### Benefits +

-   Quantifiable way to track business health and spot opportunities for improvement/experimentation
-   Drives organization alignment toward measurable target (often part of an OKR framework)
-   Used for comparison across time periods and/or segments
-   Input to data-informed decision making process
-   Standardize way to track performance
-   Online metric improvement hypotheses typically can be tested via AB testing

##### Pitfalls -

-   Expecting a single metric to tell the full story
-   Over complicated metrics not understood by end users
-   Reliant on vanity metrics (e.g. always increasing metrics) that look good on paper but not tied to core business action
-   Easy to game goal metrics
-   Missing big picture and obsessing over small metric movements
-   Cherry picking metrics/time windows/segments as a way to prove a previously held opinion
-   Abandoning qual signals and over indexing on quant metrics due to ease of measurement
-   Overly sensitive or overly stable metrics that limit actionability
-   Not monitoring metric input data quality resulting in metric quality erosion over time
-   Trying to precisely measure hard to quantify experiences (i.e. love, happiness, delight, etc)

------------------------------------------------------------------------

# Business Metric Examples

##### Financial Metrics

-   **Sales**: total units or services sold (volume trends)
-   **Revenue**: income from all sources before deducting any costs (scales with business activity; nuances on how revenue recognition works for financial accounting)
-   **Cost of Goods Sold (COGS)**: direct variable costs of production (can scale up with more production)
-   **Gross Profit**: revenue minus COGS (assess efficiency of using resources)
-   **Gross Margin**: percentage of revenue that is gross profit (efficiency at converting revenue to profit)
-   **Burn Rate**: cash spent per time period (highlights cash flow sustainability)
-   **Runway**: the number of time periods (often months) a company can continue to operate at its current burn rate before running out of cash

##### Marketing Metrics

-   **Marketing Spend:** total spend on marketing across channels
-   **Customer Per Acquisition (CPA):** typical cost associated with acquiring a new customer
-   **Lifetime Value (LTV):** total profit one expects from customer relationship over time period
-   **Conversion Rate (CVR):** number of folks who purchased / number of folks who were exposed to offer
-   **LTV to CAC:** ratio of LTV to customer acquisition costs (used to assess marketing spend efficiency)

##### Product Usage & Growth

-   **Active Users:** number of unique users to interact with core value prop of product (often segmented as daily active users, weekly active users, monthly active users)
-   **Retention Rate:** percent of unique users who use the product period vs period (often segmented by user cohorts based on when the users joined the product)
-   **Feature Adoption:** percent of unique users who use a feature out of the total user base eligible to use the feature

##### Customer Satisfaction

-   **Net Promoter Score (NPS):** survey question on scale of 0 to 10 asking about willingness to recommend (NPS Score = % of 9 or 10 respondents - % of 6 or less respondents)
-   **Customer Satisfaction Score (CSAT):** measures how satisfied respondents are with a product and/or service (typically a survey question with a [Likert scale](https://en.wikipedia.org/wiki/Likert_scale) and response rate of top 1 or 2 answers used)

##### Operational Metrics

-   **Time to close new customer:** time duration between first contact with prospect till they become a paying customer
-   **Time to hire:** time duration between job posting and offer acceptance
-   **Employee turnover:** rate of folks leaving the company (number of employees leaving / average number of employees during time period)
-   **Employee DEI:** company representation across factors like race, gender, age, and more

------------------------------------------------------------------------

# Metric Frameworks

-   Next step bookmark
-   Collaboration with GPT: https://chat.openai.com/c/80cc8bb2-f1fd-44fd-9a56-e65da80dc361
-   AARRR (Pirtate metrics)
-   Google HEART
-   North Star Metric
-   Additional Funnel Metrics

------------------------------------------------------------------------

# Defining Success Metrics for X new feature/experience

##### 1. What is being tested and why?

-   What is the objective of the feature/experience? who is it for?
-   What would the high-level experience look like?

##### 2. Product and business goals

-   think through how does that objective tie to the overall company goals/mission
-   is the business looking for product market fit first or revenue or both out of the gate?
-   if it's a new product line, consider end to end funnel metrics vs if feature might be more targeted metrics

##### 3. Primary metric(s)

-   primary metrics should align to the core value prop of the new feature/experience/business and tie to overall company goals/mission
-   if the scale of the experience is large or it's a net new app: primary metrics might span (acquisition, activation, retention/engagement, referral, revenue)
-   simplicity scales (best metrics are straightforward based on name vs have complex logic that is hard for stakeholders to grasp/remember)
-   ideally, one to two primary metrics so there's decision simplicity and clear actions one could take to improve them
-   not easily game-able
-   sensitive enough to move (i.e. vs already being at a ceiling or floor or out of the control of X team)
-   measurable in the short-term and have proven link to company goals or there's a strong strategic assumption/hypothesis they do
-   ideally the primary metrics would work in an AB test construct
-   assuming metric A and B could both work, if A has been consistently used at the company and pressure tested then A is likely better
-   not to say new metrics shouldn't be used, but new net metrics for strategic tests/launches should have a clear value prop over established reliable metrics
-   based on context from #1, then we can list out several options then stack rank to get one to two options
-   depending on the stakeholder/scenario, stack ranking might be done with stakeholders or analyst delivers recommendation to stakeholder

##### 4. Guardrail/monitoring metrics

-   make sure other parts of the experience isn't eroding unexpectedly so it's clear the overall business is benefiting vs a small surface area getting a win but hurting the system overall
-   metrics that should be stable are stable
-   key segments might also be part of the guardrail
-   other upper and lower funnel pieces of the funnel that make sense and can be used for context/monitoring

##### 5. Time frame and users filters

-   does the metric need a time filter?
-   all users? or only paying users? or new vs existing users?

##### 6. Using the primary and guardrail metrics

-   ideally run an experiment
-   one or both primary metrics improve and guardrail metrics/segments stable then = good/launch
-   one or both primary metrics negative and guardrail metrics/segments stable then = try to understand why/iterate further or move on
-   one or both primary metrics improve and guardrail metrics/segments not stable then = try to understand why/iterate further or move on
-   one or both primary metrics negative and guardrail metrics/segments not stable then = don't move forward

------------------------------------------------------------------------

# Investigating Metric Change

1.  Understand the metric definition, filters applied, and time window
2.  Seasonal business context or known experience change that might explain the change
3.  Is the change meaningful enough to matter to the business or is it within typical fluctuation
4.  Data pipeline or experience bug that is skewing the metric
5.  Drivers analysis slicing and dicing data to uncover deviation root cause (new vs existing, device types, geo, power users vs non power users, paid vs free)

------------------------------------------------------------------------

# Example Business Scenarios

#### Is active user metric growth healthy or not?

1.  how is active defined and how is healthy growth being determined

-   is active a generic action or specific action

2.  segments driving the metric

-   are new users driving the metric? i.e. acquisition
-   or are existing users returning and why? i.e. incentive, seasonality, new experience release, etc

3.  acquisition quality assessment

-   \% of channels attribution (paid vs free)
-   LTV to CAC to assess sustainability
-   If paid then LTV to CAC needs to be considered to determine if the growth is healthy or not
-   If acquisition is primary organic then less emphasis on LTV to CAC

4.  engagement

-   if active is a generic catchall
-   dig deeper to understand if folks are having engagement tied to the core value prop of the service

5.  retention

-   if the active folks are having quality engagement depth then confirm if the engagement sustains overtime using retention signals

6.  qual feedback

-   NPS
-   surveys
-   interviews

#### Assessing the quality of organic traffic/active user growth

1.  analyze traffic source

-   which organic channels are driving the increase
-   determine if increase is due to viral event, mention by popular source, other short term

2.  retention metrics

-   are folks joining and then sticking around to glean value from the product
-   DAU, WAU, MAU
-   and ratio between them

3.  engagement depth

-   active users are good but highly engaged active users are even better
-   session duration, pages per sessions, actions taken per session

4.  cohort analysis

-   are new cohorts behaving different than past cohorts

5.  organic search

-   if organic is driving volume look for critical keyword vis to be stable or improving
-   make sure attributed search volume is not due to seasonal spike

6.  content longevity

-   if organic content marketing (SEO, social) is driving growth
-   look into how evergreen the content and cost to produce
-   does content require high turnover

7.  word of mouth and brand advocacy

-   are a passionate set of folks telling others and driving growth
-   investigate forums, social mentions, etc

Misc considerations

-   channel diversification is key to reduce risk
-   true test of sustainability is time

#### Explain usefulness of trended ratios between DAU, WAU, MAU, QAU

-   help explain the type of usage patterns folks have and gauge stickiness
-   when the ratios are close to 1 it suggests high stickiness during the metric frame
-   when the ratios are low it suggests less frequent usage during the metric frame

#### DAU / WAU

-   helps to shed light on if WAU is driven by folks coming back each day of week (ratio close to 1) or many users coming back once per week

#### DAU / MAU

-   higher ratio means folks are coming back frequently and there's more of a daily value prop
-   lower ratio suggests folks are coming less frequency during a month

#### WAU / MAU

-   if metric is close to 1 it suggest folks are coming back each week
-   if metric is low suggests that folks are not coming back multiple weeks in a month

#### MAU / QAU

-   helpful if a product is used on less frequent basis
-   scenarios where MAU could decline but QAU increases

#### Why look at active user ratios vs absolute counts

-   both are helpful to give a read on the engagement health of the business
-   various counts can indicate if the active user base is growing or not
-   active user ratios could assess the stickiness of the user base even if overall counts are declining (i.e. small active user base but those that are active are highly active)
-   helpful to look at ratio of active to inactivates as well (absolute counts could be increasing but share of active population could be declining)
-   counts could be driven up by large sign up volume and ratios can spotlight if folks are sticking around

#### When metrics don't tie

-   is the difference large and material?
-   if so, investigate to understand why? there could be skeletons in the closest
-   for strategic high visibility work it's extra important that metrics being used to drive decisions/financials tie out as expected. From the start investigate and build high quality solution to minimize risk (\$ renewal forecast case study)

------------------------------------------------------------------------

# Misc notes

-   how to handle scenarios when macro trend is pulling metrics up (i.e. COVID for online streamers and businesses)
-   how to handle scenario when comparing segment performance where one segment benefits from seasonality/marketing push more than another
-   leading vs lagging metrics in practice
-   vanity metrics can be context dependent (upper funnel metric like app installs can be a vanity metric for a mature business but a critical metric for a new business)
-   social media likes might be an OKR vanity metric; however, if likes correlate with downstream core actions then it could be a useful metric
-   hard to have a one size fits all solution to metrics and metric frameworks as it's highly dependent on the business objectives and the stage of the business
-   metric goals and incentives alignment (look out for path of least resistance to hit metric goals if career incentives are tied to metric goals) 
-   further learning resources???

