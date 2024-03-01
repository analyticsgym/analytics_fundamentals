# WORK IN-PROGRESS

------------------------------------------------------------------------

# Metrics Overview

#### Why use quantifiable metrics?

-   Metrics help organizations systematically measure, assess, and improve performance

#### Metric benefits and pitfalls

+------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
| Benefits +                                                                                           | Pitfalls -                                                                                           |
+======================================================================================================+======================================================================================================+
| -   Quantifiable way to track business health and spot opportunities for improvement/experimentation | -   Expecting a single metric to tell the full story                                                 |
| -   Drives organization alignment toward measurable target (often part of an OKR framework)          | -   Over complicated metrics not understood by target users                                          |
| -   Used for comparison across time periods and/or segments                                          | -   Reliant on vanity metrics that look good on paper but not tied to core business action           |
| -   Input to data-informed decision making process                                                   | -   Easy to game goal metrics                                                                        |
| -   Used to surface innovation opportunities                                                         | -   Missing big picture and obsessing over small metric movements                                    |
|                                                                                                      | -   Cherry picking metrics as a way to increase conviction for a previously help opinion             |
|                                                                                                      | -   Abandoning qual signals and over indexing on quant metrics due to ease of measurement            |
|                                                                                                      | -   Overly sensitive or overly stable metrics can limit actionability                                |
|                                                                                                      | -   Metric quality erodes over time due to limited or no data quality monitoring powering the metric |
|                                                                                                      | -   Some things are tricky to measure with metrics (i.e. love, happiness, delight, etc)              |
+------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+

#### Business metric examples

\<\<\<Next step bookmark\>\>\>

-   Financial
    -   Sales:
    -   Expenses:
    -   Margin:
-   Marketing
    -   Customer Acquisition Cost (CAC):
    -   Lifetime Value (LTV):
    -   Conversion Rate (CVR):
-   Product Usage & Growth
    -   Active Users:
    -   Retention Rate:
    -   ...
-   Customer Satisfaction
    -   Net Promoted Score
    -   Customer Satisfaction Score
    -   ...
-   Operational
    -   ...
-   Employee
    -   Turnover
    -   DEI

#### Creating metrics

-   pros and cons to different types of metrics

#### Assessing metric usefulness

#### Using metrics

#### QAing metrics

#### Common metric flaws and challenges

------------------------------------------------------------------------

# Common Metric Frameworks

-   AARRR (Pirtate metrics)
-   Google HEART

------------------------------------------------------------------------

# Defining Success Metrics for X new feature/experience

#### 1. What is being tested and why?

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

##### DAU / WAU

-   helps to shed light on if WAU is driven by folks coming back each day of week (ratio close to 1) or many users coming back once per week

##### DAU / MAU

-   higher ratio means folks are coming back frequently and there's more of a daily value prop
-   lower ratio suggests folks are coming less frequency during a month

##### WAU / MAU

-   if metric is close to 1 it suggest folks are coming back each week
-   if metric is low suggests that folks are not coming back multiple weeks in a month

##### MAU / QAU

-   helpful if a product is used on less frequent basis
-   scenarios where MAU could decline but QAU increases

#### Why look at active user ratios vs absolute counts

-   both are helpful to give a read on the engagement health of the business
-   various counts can indicate if the active user base is growing or not
-   active user ratios could assess the stickiness of the user base even if overall counts are declining (i.e. small active user base but those that are active are highly active)
-   helpful to look at ratio of active to inactivates as well (absolute counts could be increasing but share of active population could be declining)
-   counts could be driven up by large sign up volume and ratios can spotlight if folks are sticking around

### When metrics don't tie

-   is the difference large and material?
-   if so, investigate to understand why? there could be skeletons in the closest
-   for strategic high visibility work it's extra important that metrics being used to drive decisions/financials tie out as expected. From the start investigate and build high quality solution to minimize risk (\$ renewal forecast case study)

------------------------------------------------------------------------

# Backlog

-   how to handle scenarios when macro trend is pulling metrics up (i.e. COVID for online streamers and businesses)
