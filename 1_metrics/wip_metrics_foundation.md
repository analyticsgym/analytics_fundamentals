## ------------------------------------------------------------------------

# WORK IN-PROGRESS

## ------------------------------------------------------------------------

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

### AARRR (Pirate metrics)
-   **Overview**
    -   [popularized by Dave McClure (2007)](https://www.slideshare.net/dmc500hats/startup-metrics-for-pirates-long-version)
    -   a systematic way to optimize the customer funnel
    -   Dave suggested startups foucs on 5 to 10 conversion steps to measure and iterate on via AB tests (based on cost, conversion, and business revenue model)
    -   Dave recommends 80% effort deployed on existing feature optimization and 20% on new feature dev
    -   funnel metrics segmented by audience, channel, campaign theme, landing page & CTA, copy & graphics

1.  **Acquisition:** how users find out about the product
    -   channel optimization/prioritization based on volume, cost, efficiency
    -   example funnel metrics
        -   site visits
        -   engaged prospect rate (pages viewed \>= x, time spent \>= y, clicks \>= z)
2.  **Activation:** first happy experience and account creation
    -   getting folks to key feature usage and actions that signal feature usage
    -   example funnel metrics
        -   account completion rate
        -   key feature usage rate
3.  **Retention:** repeat usage of the product
    -   marketing, content, features, processes, and offers to pull folks back to engage with the product
    -   example funnel metrics
        -   reactivation campaign CTR
        -   percent days engaged \>= x during first 30 days
4.  **Referral:** users like the product and tell others
    -   encourage users to refer after they have a happy experience
    -   example funnel metrics
        -   percent users who send invites \>= x
        -   percent users who have referral activate \>= x
5.  **Revenue:** users complete monetization actions
    -   aligns to how the business model captures value from customers
    -   example funnel metrics
        -   percent users who to pay
        -   percent users who break-even

### Google HEART
-   **Overview**
    -   proposed by Googlers Kerry Rodden, Hilary Hutchinson, and Xin Fu in [2010 paper](https://research.google/pubs/measuring-the-user-experience-on-a-large-scale-user-centered-metrics-for-web-applications/)
    -   set of user-centered metrics to measure progress toward user experience goals
    -   metrics tracked for a given time period and/or cut by user segments/cohorts
    -   some metric categories may not be applicable for certain products/business use cases

1.  **Happiness**
    -   attitudinal metrics
    -   i.e. CSAT, NPS, and ease of use
2.  **Engagement**
    -   behavioral metrics on level of involvement with the product
    -   i.e. frequency, depth, breadth of interaction
    -   averages per user and rate of actives to do action are often good starting points
    -   engagement metrics that predict future product retention or revenue tend to be prioritized
3.  **Adoption**
    -   new users to start using a product
    -   expressed as absolute count, proportion of total users, proportion of active users, proportion of new/existing users, etc
    -   defining "using" is product/feature/business case specific
    -   i.e. accounts created last 7 days, number of users to turn on a feature, etc
4.  **Retention**
    -   percent of users who use the product during period X and then use it again during period Y
    -   new products tend to have evolving retention metrics vs mature products tend to have more stable retention metrics outside of seasonal/macro events
    -   cohort comparison is common here (i.e. cohorts based on when users first joined the product or adopted the product/feature)
    -   i.e. percent of 7 day actives this week who were also active last week, percent of cohort users who use the product month 2, month 3, etc
5.  **Task success**
    -   is the product UX/design intuitive and efficient at getting users to desired objective
    -   task metrics help pinpoint areas of opportunity to reduce friction and improve the user experience
    -   behavioral metrics assessing efficiency, effectiveness, error rates
    -   efficiency examples: time to complete task, clicks to complete task, steps complete per time window x
    -   effectiveness examples: step completion rate, rate of users who stick to optimal path, rate of users complete task without assistance
    -   error rates examples: page error rate, abandonment rate by sub task location, issues reported rate

### North Star Metric Framework
-   **Overview**
    -   inspired by Sean Ellis and the growth hacking movement
    -   a single metric to guide strategy, inform decision making, and set internal focus
    -   ideally as the north star metric improves business results improvement follows
    -   [Amplitude's North Star Playbook](https://amplitude.com/books/north-star/about-the-north-star-framework)
-   **Framework key elements**
    -   lagging business results/value: often revenue, continued paying customers, etc
    -   north star metric (NSM): captures the core value prop of the product and is a leading indicator of business value (i.e. critical rate, count, ratio, etc)
    -   input metrics: 3 to 5 sub-metrics that influence the north star metric
    -   work: activities that hope to move input metrics
-   **Example for subscription business**
    -   business result: renewing subscription revenue
    -   NSM: core value prop action that correlates with future sub revenue
    -   inputs: supporting tasks/actions that improve customer volume and frequency of core value prop action
    -   work: hypotheses to move and improve the input metrics
-  **NSM checklist**
    -   aligns to customer value exchange with the product
    -   represents the company's vision and product strategy
    -   leading indicator of critical business results
    -   measurable via product analytics tracking
    -   produces actionable set of sub-metrics teams can run experiments on
    -   simple to explain and understand for technical and non-technical team members
-   **Inputs**
    -   starting point ideas: breadth, depth, frequency, efficiency
    -   not too sensitive or too broad
    -   teams can generate large volume of actionable ideas to move the inputs

### Meta Product Market Fit
-   **Overview**
    -   [Meta Analytics PMF Playbook](https://medium.com/@AnalyticsAtMeta/analytics-and-product-market-fit-11efaea403cd)
    -   product market fit (PMF): the value a product delivers for a specific market segment
    -   in other words, are we building something people want? are folks using the product? do folks come back to use it?
    -   valuable products with high product marketing fit: keep people coming back, sustainability acquire new users, and have long stretches of usage
-   **PMF measurement criteria**
    -   stable retention: do folks keep coming back after using the product?
    -   sustainable growth: is there a healthy approach to acquire, retain, and resurrect folks over time?
    -   deep engagement: do folks have long usage duration on core value prop actions?
-   **Stable retention**
    -   active: defined based on core value prop of product
    -   volume of active cohort users in period N+1 / Volume of active cohort users in period N
-   **Sustainable growth**
    -   Meta often waits to ramp up upper funnel acquisition till after products can sustainability add and retain folks
    -   growth accounting states model used to dissect active user growth ([Duolingo example](https://blog.duolingo.com/growth-model-duolingo/))
    -   i.e. is active user growth being fueled by new users or existing user frequency?
-   **Deep engagement**
    -   time spent, days engaged out of the last 28 days, purchases/revenue per user

### Growth Equation
-   **Overview**
    -   part growth framework + part metrics framework
    -   framework for how consumer products grow active users / customer base
    -   inspired by [Chamath Palihapitiya](https://youtu.be/raIUQP71SBU) (Facebook VP of Growth: 2007 - 2011) and [Andy Johns](https://review.firstround.com/indispensable-growth-frameworks-from-my-years-at-facebook-twitter-and-wealthfront/) (early growth employee at Facebook, Twitter, Quora, Wealthfront exec, former VC)
-   **Chamath's description [^wip_metrics_foundation-1]**
    -   acquisition: get people to the front door
    -   activation: get them to an "Aha" moment as quickly as possible
    -   engagement: deliver core value as often as possible
-   **Andy Johns's description (from Andy's work with Chamath at early days Facebook)**
    -   top of funnel (traffic, conversion rate) x
    -   magic moment (create emotional response) x
    -   core product value (solves real problems) =
    -   sustainable growth
-   **Facebook early days example**
    -   grow active users (active user growth makes the social network more valuable, provides evidence for additional funding/IPO, monetization opportunities, etc)
    -   active users = active new users + active existing users
    -   hypothesized set of key metrics and bets aligned to each growth equation pillar
    -   acquisition (top of funnel sign ups)
        -   universitys adopted
        -   expansion to high schools and non-students
        -   international expansion / localization
    -   activation (7 friends in 10 days)
        -   user onboarding optimized for profile completion and connecting with friends
    -   engagement (time spent & frequency)
        -   news feed innovation and time spent
        -   platform integrations and expanding engagement sources (i.e. game developers building games for FB; FarmVille)
        -   "Like" button as a mechanic for how users interact with content
        -   positioning profile content as a "Timeline" to more easily see a users history and life events
        -   Facebook mobile app and Instagram acquisition to capitalize on shift to mobile
-   **Notes on operationalizing the growth equation framework**
    -   build a culture of measuring, testing, and try new things
    -   requires teams to be unemotionally detached from what is getting built
    -   don't overly rely on gut as most folks can't predict correctly
    -   growth equation specifics differ by company type (core elements persist: acquisition, activation, engagement)

[^wip_metrics_foundation-1]: Virality wasn't a core focus initially (how existing users recruit other users); Chamath put the emphasis on acquisition, activation, and engagement vs K factor (virality)

------------------------------------------------------------------------

# Best Practices for Defining Metrics

1.  **Keep it simple**
    -   use the simplest metric logic to achieve the desired measurement objective
    -   ideally new metrics should be easy to understand and explain
2.  **Self-documenting metric names**
    -   aim for metric names that describe the metric for tech and non-tech stakeholders
    -   "last_28_days_trailing_total_sales" vs "total_sales" to represent the last 28 days of a trailing total
3.  **Actionable**
    -   source of inspiration for ways to improve the product experience or business
    -   hypothesized cause and effect relationship (i.e. doing X will lead to Y metric improvement)
4.  **Sensitive enough to move but not volatile**
    -   tends to require a balance between broad/high-level metrics and granular/detailed metrics
    -   actionable lagging metric or overaly broad metric might not be sensitive enough to move in the near term
    -   volatile metrics can lead to hard to explain fluctuations
5.  **Analyze historical metric trend, fluctuations, and underlying data quality**
    -   how does seasonality and user mix impact the metric?
    -   has the underlying data passed QAed for x new metric?

------------------------------------------------------------------------

# Investigating Metric Change

1.  **Determine if the metric change is meaningful to the business**
    - if the change is within the expected fluctuation range, it may not require further investigation
    - however, if the change is small but unexpected, it might be worth investigating as a signal for a larger issue (business/product/data issue)

2.  **Understand the metric definition, filters being used, and the time window**
    - check if the metric logic, filters, date ranges, and time zone conversions are consistent

3.  **Consider any seasonal, business, or product changes that could influence the metric**
    - metric changes during specific events may be expected (i.e. holiday shopping for an ecommerce product)

4.  **Check for any data pipeline or experience bugs that might be distorting the metric**
    - recommended to start with the business context and then move to data/experience QA

5.  **Conduct a drivers analysis to identify the root cause of the deviation**
    - use segmentation analysis to understand if a specific segment is driving the change
    - e.g. compare new users to existing ones, device types, geographic locations, power users versus non-power users, and paid versus free users
    - i.e. new users during a particular period are taking certain actions at a lower rate than in previous periods

------------------------------------------------------------------------

NEXT STEP BOOKMARK

# When Metrics Don't Tie Between Sources
-   IS THIS NEEDED?
-   is the difference large and material?
-   if so, investigate to understand why? there could be skeletons in the closest
-   for strategic high visibility work it's extra important that metrics being used to drive decisions/financials tie out as expected. From the start investigate and build high quality solution to minimize risk (\$ renewal forecast case study)

------------------------------------------------------------------------

# Defining Success Metrics for a New Product Experience

1.  Context Collection

-   what problem/need/pain is the new product experience solving?
-   what would the high-level experience look like?

2.  Connecting the New Product Experience to Company Objectives

-   is the new product experience expected to deliver on a key company objective (i.e. establish product market fit, increase acquisition, improve engagement depth or retention, drive average revenue per user or LTV higher, etc)?

3.  Primary metric(s)

-   align primary metric(s) to the core value prop the new experience is delivering and to the company objective being targeted

4.  Guardrail/monitoring metrics

-   make sure other parts of the experience isn't eroding unexpectedly so it's clear the overall business is benefiting vs a small surface area getting a win but hurting the system overall
-   metrics that should be stable are stable
-   key segments might also be part of the guardrail
-   other upper and lower funnel pieces of the funnel that make sense and can be used for context/monitoring

5.  Time frame and users filters

-   does the metric need a time filter?
-   all users? or only paying users? or new vs existing users?

6.  Using the primary and guardrail metrics

-   ideally run an experiment
-   one or both primary metrics improve and guardrail metrics/segments stable then = likely good outcome
-   one or both primary metrics improve and guardrail metrics/segments negative = try to understand why/iterate further
-   one or both primary metrics negative and guardrail metrics/segments stable then = try to understand why/iterate further or move on
-   one or both primary metrics negative and guardrail metrics/segments negative = likely bad outcome; iterate or move on

------------------------------------------------------------------------

# Is active user metric growth healthy or not?

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

# Assessing the quality of organic traffic/active user growth

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

Misc considerations - channel diversification is key to reduce risk - true test of sustainability is time

# Explain usefulness of trended ratios between DAU, WAU, MAU, QAU

-   help explain the type of usage patterns folks have and gauge stickiness
-   when the ratios are close to 1 it suggests high stickiness during the metric frame
-   when the ratios are low it suggests less frequent usage during the metric frame

### DAU / WAU

-   helps to shed light on if WAU is driven by folks coming back each day of week (ratio close to 1) or many users coming back once per week

### DAU / MAU

-   higher ratio means folks are coming back frequently and there's more of a daily value prop
-   lower ratio suggests folks are coming less frequency during a month

### WAU / MAU

-   if metric is close to 1 it suggest folks are coming back each week
-   if metric is low suggests that folks are not coming back multiple weeks in a month

### MAU / QAU

-   helpful if a product is used on less frequent basis
-   scenarios where MAU could decline but QAU increases

# Why look at active user ratios vs absolute counts

-   both are helpful to give a read on the engagement health of the business
-   various counts can indicate if the active user base is growing or not
-   active user ratios could assess the stickiness of the user base even if overall counts are declining (i.e. small active user base but those that are active are highly active)
-   helpful to look at ratio of active to inactivates as well (absolute counts could be increasing but share of active population could be declining)
-   counts could be driven up by large sign up volume and ratios can spotlight if folks are sticking around

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
-   product X has 100% user turnover week to week but 7 day actives is still growing? How might this be?
-   change aversion
-   Twyman's law
-   Outliers
-   Scaling upper funnel acquisition and/or rising macro tide can hide churn problem
-   Study Andrew Chen's blog and deck on [growth accounting](https://andrewchen.com/wp-content/uploads/2018/11/a16z_growth_deck.pdf) and other topics related to metrics/analytics
-   What to do if a product isn't retaining users?
-   What to do if a product isn't getting folks to a activate?
-   The path of least resistance to hit metric goals and unintended consequences
