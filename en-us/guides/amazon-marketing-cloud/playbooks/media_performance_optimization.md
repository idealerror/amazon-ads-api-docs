---
title: Amazon Marketing Cloud - media performance optimization
description: media performance optimization playbook
type: guide
interface: api
---
# Media performance optimization

Amazon Marketing Cloud (AMC) allows its users the unique opportunity to compile media spend and performance inputs at each funnel level to track the diminishing rate of returns at greater media spends.
This approach gives advertisers the novel opportunity to take direct action on their media budget by using a holistic view of their advertising attribution down to the purchase data.

## Introduction

This playbook guides you through optimizing ad spend distribution by maximizing a specified KPI. It uses historical performance data to create diminishing returns curves, which project expected outcomes at various budget levels allowing you to visualize performance expectations and identify the point where additional investment yields diminishing returns.

The playbook is designed for an illustrative use case. Further customization for your use cases will lead to a better outcome.

## What is media performance optimization?

Media performance optimization (MPO) is a toolkit that helps advertisers make informed decisions about the budget levers in Amazon Ads by utilizing historical advertising performance to project an expected Return on Ad Spend (ROAS) given a custom spend distribution. The tool also helps in this process by calculating the optimal distribution of spend using a specified KPI that the advertiser wishes to maximize.

This tool does so by utilizing historical advertiser performance data to build diminishing returns curves of performance at historical budget levels. These curves can be leveraged to project expected performance at a custom budget levels. establishing expectations for performance using the same campaigns that ran historically. Advertisers can visualize the point at which the benefits gained from an investment begin to decrease compared to the amount of resources invested.

The formation of these curves plotting uses a non-linear least squares curve fitting model, analyzing at least six months of historical data with an emphasis on recent data to be relevant to the current market condition. The model produces a series of coefficients that can be used to draw a custom curve for each unique combination of media tactics.

The budget optimizer uses Constrained Non-linear Optimization to recommend budget distribution across multiple inputs, satisfying a system of inequalities and equalities based on ABC coefficients and fixed budgets.

![MPO1](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_1.png)

The playbook will answer the following questions:

1. How to analyze historical data to identify the overall cost and key performance indicator (KPI) relationship and how it changes at different cost levels.
2. How to utilize these cost/ KPI relationships to create diminishing returns curve that represent the data’s trend.
3. How to project custom spend levels across historical tactics to estimated expected return on investment (ROI) at different KPI levels.
4. How to determine the optimal distribution of spend to maximize returns for a given KPI.

![MPO2](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_2.png)

### Prerequisites

To maximize success with the Media Performance Optimizer tool, it’s recommended that you understand the following concepts:

#### Advertising

- Ability to define dimensional inputs that would be a lever within the media planning / buying process.
- Ability to define clear media goals and objectives (e.g., increasing revenue by x% or achieving minimum sales of y$.
- Mapping of categorical variables to clusters to reduce the overall dimensionality of the curves.
- Defining KPIs to build your optimization model around.
- Data driven decision making ability regarding the timing of historical data and determining the time decay component.

#### Technical

- Python - A large portion of the work takes place in Python, so proficiency is essential.
- Curve Fitting Metrics - Understanding metrics such as Adjusted R² and Mean Absolute Percentage Error (MAPE) is helpful for assessing the quality of diminishing returns curves
- AMC concepts, specifically data aggregation thresholds, privacy limitations, and other restrictions listed in the AMC documentation.
- AMC SQL knowledge (link only available for AMC UI users). The processes outlined here relies on altering SQL code to match your purposes.
- Basic AWS Knowledge of Amazon SageMaker. You will need to know how to set up an Amazon SageMaker Notebook in accordance with your company's standards.

### Costs

- Cost assumptions are based on standard pricing for S3, Lambda, and Sagemaker.
- Monthly costs per advertiser have been reported at less than $1, but may vary depending on data load, architecture, frequency, etc.

### Recommendations

It is recommended to have "Amazon Marketing Cloud Insights on AWS"  or a similar custom solution deployed. The AMC insights on AWS provides several solutions:

- Workflow manager: service to create and schedule AMC query executions
- Data Lake: service to transform signals from the source bucket into parquet and catalog in Athena. This allows for signals to pulled via Athena.

### Assumptions

The MPO’s data-driven component uses historical data within the past year to project the cost / return ratio at various spend levels. A time decay component is included to weight the curves 10% more heavily towards more recent data. The 10% figure was chosen as a starting point to emphasize recent data without over-indexing, and can be adjusted by the user.

> [WARNING] The model is not a forecast or a prediction of future returns, as would involve a separate practice for time series analysis.

### Key differentiators: Performance projection model vs predictive forecast model

The table below outlines the key differentiators between the performance projection model (covered in this playbook) and a traditional predictive forecast model.

| -                            | Performance Projection                                                                                                                                                                           | Forecast                                                                                                                         | Applicability to AMC                                                                                                                                                                                                                                                                               |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Curve Fitting Model Function | Assumes a specific function to fit the data\. User\-performed "curve fitting exercise" outside the time\-decay model is preferred\.                                                              | Leverages an ensemble approach of various machine learning methodologies to minimize error between predicted and actual values\. | Specific function allows faster processing for hundreds of curves simultaneously, offering parameter tuning to maximize curve fit\.                                                                                                                                                                |
| Historical Data Lookback     | Requires at least 6 months to capture enough weekly data to build a curve                                                                                                                        | Requires at least 2 years to capture trend and seasonality of time data component                                                | AMC policy limits queries 13\.5 months, making 2\-year historical lookback unfeasible without a data lake\.                                                                                                                                                                                        |
| Model Validity               | Utilizes R2 and MAPE to assess the overall fit quality of the curves the data\.  \(note, a specific LMfit function was chosen for curve fitting, advertisers can adjust to find the best fit\.\) | Uses training and testing data within a time series point, measuring difference between forecasted and actual trends\.           | When using R2 and MAPE, it’s important to set a threshold for when return distribution variance that does not reflect the overall shape of the curve\.                                                                                                                                            |
| Time Series Forecasting      | Assumes seasonality will not significant impact return on spend                                                                                                                                  | Assumes stationarity with the data to account for changing trends and seasonality                                                | When using the performance optimizer, it's important to consider that seasonality will be difficult to reflect with less than a year’s data\. When building a data lake, incorporate data from the same time a year ago and adjust the time decay to weight towards prior year's same timeframe\. |
| Compute Power                | Relatively low since it's a curve fit on existing data rather than requesting new data                                                                                                           | Gets higher with the quality of forecast model you want to run                                                                   | Considering MPO needs to build a model for every combination of media tactic and KPI, minimizing compute capacity is crucial as the model will run dozens of times for each combination\.                                                                                                          |

### Playbook steps

The playbook is structured in the following order:

1. Data preparation
2. Diminishing returns model execution
3. Optimization model execution
4. Budget planning and visualization

## Data preparation

When configuring your optimizer tool, the most important step is selecting the inputs and dimensions you want to build your performance optimizer around. Preparing your data properly will help ensure you have the best-shaped curve to plan against.

### Defining model dimensions

The script below requires you to make several decisions regarding the dimensional breakdown for building curves. These decisions will be based on:

- the overall number of unique categorical variables in these dimensions
- the customization in these inputs in informing a media plan.

At a high-level view, we will use an example script that focuses on the following dimensions:

- Ad product type
- Advertiser
- Campaign

<details class="details-bar">
<summary>Click to see code: data preparation  </summary>

```
with traffic_and_conversion_events as (
    select
        impression_date as dim_date,
        advertiser,
        case
            when ad_product_type is null then 'dsp'
            else ad_product_type
        end as ad_product_type,
        campaign,
        0 as impressions,
        0 as clicks,
        purchases,
        units_sold,
        product_sales,
        add_to_cart,
        detail_page_view,
        winning_bid_cost / 100000 as winning_bid_cost,
        0 as cost
    from
        amazon_attributed_events_by_traffic_time
    union
    all
    select
        event_date as dim_date,
        advertiser,
        case
            when ad_product_type is null then 'dsp'
            else ad_product_type
        end as ad_product_type,
        campaign,
        impressions as impressions,
        clicks,
        0 as purchases,
        0 as units_sold,
        0 as product_sales,
        0 as add_to_cart,
        0 as detail_page_view,
        0 as winning_bid_cost,
        spend / 100000000 as cost
    from
        sponsored_ads_traffic
    union
    all
    select
        impression_date as dim_date,
        advertiser,
        'dsp' as ad_product_type,
        campaign,
        impressions,
        0 as clicks,
        0 as purchases,
        0 as units_sold,
        0 as product_sales,
        0 as add_to_cart,
        0 as detail_page_view,
        winning_bid_cost / 100000 as winning_bid_cost,
        total_cost / 100000 as cost
    from
        dsp_impressions
)
select
    extract(
        week
        from
            dim_date
    ) as week,
    min(dim_date) as dim_date,
    advertiser,
    ad_product_type,
    campaign,
    sum(impressions) as impressions,
    sum(clicks) as clicks,
    sum(purchases) as purchases,
    sum(units_sold) as units_sold,
    sum(product_sales) as product_sales,
    sum(add_to_cart) as add_to_cart,
    sum(detail_page_view) as detail_page_view,
    avg(winning_bid_cost) as winning_bid_cost,
    sum(cost) as cost
from
    traffic_and_conversion_events
where dim_date is not null
group by
    week,
    ad_product_type,
    advertiser,
    campaign
```

</details>

The query above searches the `amazon_attributed_events_traffic_time`, `sponsored_ads` and `dsp_impressions` tables to compile all campaign activity that took place each week over the past 6 months. Additionally, the query pulls the following impression and engagement metrics:

- Impressions
- Clicks
- Purchases
- Units sold
- Product sales
- Add to cart
- Detail page view
- Cost

For each engagement metric, the diminishing returns curves will plot the overall volume of each on a 2-dimensional plane with the corresponding cost. Then, it will use a least squares model to draw the actual curve across each individual combination of ad product type, advertiser, and campaign on each engagement metric. This can include both endemic and non-endemic engagement metrics, as long as they can be directly attributed to advertising activity available on AMC.

#### User input and decisions

 **Applying additional dimensions**:

Customers can decide which dimensions to apply to the diminishing returns model. Consider the following caveats:

- Having enough data to draw a useful curve.
  - The hard cutoff point: Total spend of $50 and at least 3 weeks of data. (The usefulness of the curve is related to the overall quality metrics of the curve.)
  - Volume of categorical values in the dimensions: Interface output typically includes a dropdown for user to select categorical values.

    > [NOTE] Large number of values (e.g., hundreds of campaigns) may be user-unfriendly
    >

 **Applying additional engagement metrics**

- Customers can expand their plane of reference for engagement metrics to include items tracked within AMC such as new to brand purchases and brand halo purchases. Consider the following caveats:
  - Data must be available to track within AMC.
  - Metrics need to be measurable at the event level.
  - Aggregate measures (e.g., click-through rate, customer lifetime value, ROAS) cannot be used.

#### First party inputs

The media performance optimizer tool allows customers to incorporate their own event level data into the model as a response variable to incorporate into the model. Considering the degrees of separation from on-Amazon advertising activity, it will create for an interesting analysis to measure the correlation between spend and customer response over time.

#### SQL output

Structurally, the SQL queries are relatively simple. They typically do not require complex calculations or functions, but focus on collecting Amazon DSP and sponsored ads impression and conversion events. The query referenced above will provide an output similar to the table below:

(using Sandbox data)

| week | dim\_date  | advertiser            | ad\_product\_type   | campaign                                             | impressions | clicks | purchases | units\_sold | product\_sales | add\_to\_cart | detail\_page\_view | winning\_bid\_cost | cost       |
| ---- | ---------- | --------------------- | ------------------- | ---------------------------------------------------- | ----------- | ------ | --------- | ----------- | -------------- | ------------- | ------------------ | ------------------ | ---------- |
| 45   | 11/6/2023  | Iris                  | sponsored\_display  | Iris\_test\_campaign\_2311016356135                  | 2418        | 460    | 48        | 36          | 1151\.79       | 53            | 361                | 0                  | 205\.2189  |
| 45   | 11/6/2023  | Kitchen Smart         | sponsored\_brands   | Kitchen Smart\_test\_campaign\_8434558048617         | 1695        | 362    | 34        | 47          | 1181\.76       | 0             | 270                | 0                  | 174\.11777 |
| 2    | 1/8/2024   | Gordon's Chocolatier  | sponsored\_products | Gordon's Chocolatier\_test\_campaign\_2444732030837  | 1441        | 309    | 33        | 40          | 1028\.31       | 30            | 272                | 0                  | 168\.67568 |
| 2    | 1/8/2024   | Jack & Jill           | sponsored\_products | Jack & Jill\_test\_campaign\_7153248131222           | 2510        | 518    | 76        | 76          | 2410\.19       | 47            | 445                | 0                  | 258\.54335 |
| 44   | 10/31/2023 | Gordon's Chocolatier  | sponsored\_brands   | Gordon's Chocolatier\_test\_campaign\_8863355878171  | 4068        | 805    | 96        | 98          | 2571\.91       | 0             | 671                | 0                  | 454\.43633 |
| 2    | 1/8/2024   | Organique             | sponsored\_display  | Organique\_test\_campaign\_8666755074267             | 901         | 191    | 20        | 28          | 891\.25        | 21            | 161                | 0                  | 93\.7971   |
| 45   | 11/6/2023  | Iris                  | sponsored\_products | Iris\_test\_campaign\_2311016356135                  | 2403        | 494    | 46        | 51          | 1338\.11       | 43            | 393                | 0                  | 181\.51358 |
| 48   | 11/27/2023 | Super Power Batteries | dsp                 | Super Power Batteries\_test\_campaign\_4121845860043 | 10560       | 0      | 101       | 124         | 3358\.32       | 137           | 1203               | 9\.12039           | 114\.2731  |
| 51   | 12/18/2023 | Super Power Batteries | sponsored\_display  | Super Power Batteries\_test\_campaign\_7718354250855 | 1563        | 312    | 30        | 29          | 729\.36        | 18            | 267                | 0                  | 129\.85538 |
| 4    | 1/22/2024  | Kitchen Smart         | dsp                 | Kitchen Smart\_test\_campaign\_6405607844188         | 40745       | 0      | 568       | 580         | 16486\.61      | 536           | 4746               | 9\.01744           | 436\.89874 |
| 51   | 12/18/2023 | Super Power Batteries | dsp                 | Super Power Batteries\_test\_campaign\_3860452757180 | 38419       | 0      | 580       | 571         | 15666\.07      | 504           | 4556               | 9\.02667           | 413\.15565 |

Here, you see that the data output includes the week number, and media date along with the advertiser, ad product, and campaigns that will serve as the dimensions in our diminishing returns model. Each line item will function as a point in our larger output, used to generate curves for the model.

From here, you can set up a scheduled workflow to continuously query data on this cadence so that your interface always captures the latest data. Reference [Workflow Management Service](guides/amazon-marketing-cloud/reporting/workflow-management-service#workflow-executions)  for guidance on establishing a scheduled workflow to regularly query your data and append it to a data lake table.

### Feature engineering

In the event that you want to start including dimensions into your diminishing returns model that do not exist as their own column, but are inputs in the line items, you will need to run some code logic to extract values from the `line_item` column into their own columns. The best practice for doing so would be to create a specialized data prep notebook that queries the data directly from the data lake and performs the regex extraction functions to compile the new line items and save them in a database that can be referenced in the diminishing returns model.

<details class="details-bar">
<summary>Click to see code: extract values  </summary>

```
query = 'SELECT * FROM "my_amcdataset".mpo_by_wk_adprod_cust_campaign_creative_lnitem_view" WHERE filtered = ''False'' '

raw = wr.athena.read_sql_query(sql=query, database="my_amcdataset", ctas_approach=False,
                                      encryption='SSE_S3',
                                      workgroup='primary', keep_files=True)

```

</details>

The specific steps to extract the dimensions from the `line_item` columns will depend on the specific dimensions you’re interested in measuring and the overall line item structure. Some sample code below will give an example on extracting some values from a campaign column.

<details class="details-bar">
<summary>Click to see code: data cleaning  </summary>

```
### Data Cleaning

raw['camp_delimiter'] = raw['campaign'].str.extract(r'(\||- |_)')
raw['camp_delimiter'] = raw['camp_delimiter'].fillna(' ')

raw['ad_delimiter'] = raw['ad_name'].str.extract(r'(\||- |_)')
raw['ad_delimiter'] = raw['ad_delimiter'].fillna(' ')

raw['campaign'] = raw['campaign'].str.replace('Q3.Q4.2021','2021_Q3Q4', regex=True)
raw['campaign'] = raw['campaign'].str.replace('GR 11.22.21','GR_GR 11.22.21', regex=True)

raw['campaign'] = np.where((raw['advertiser'] == 'Super Power Batteries'),raw['campaign'].str.replace(' - ','_', regex=True),raw['campaign'])
raw['campaign'] = np.where((raw['advertiser'] == 'Super Power Batteries'),raw['campaign'].str.replace('AO','Always On', regex=True),raw['campaign'])
raw['campaign'] = np.where((raw['advertiser'] == 'Super Power Batteries'),raw['campaign'].str.replace('2021_Iris','2021__Iris', regex=True),raw['campaign'])
raw['campaign'] = np.where((raw['advertiser'] == 'Orangique'),raw['campaign'].str.replace(' - ','_', regex=True),raw['campaign'])
raw['campaign'] = np.where((raw['advertiser'] == 'Orangique'),raw['campaign'].str.replace('ROAS','ROAS_', regex=True),raw['campaign'])
raw['campaign'] = np.where((raw['advertiser'] == 'Orangique'),raw['campaign'].str.replace('ROAS__US_Loyalty_Summer','ROAS_Loyalty_US_Summer', regex=True),raw['campaign'])

raw['campaign'] = raw['campaign'].str.replace(' -B0',' - B0', regex=True)

```

</details>

Once complete, write the data cleaning table into a parquet file and S3 that can be referenced by the diminishing returns model.

## Diminishing returns curves model

### Model formula

The diminishing returns model uses cleaned data to run an LMfit model using the ABC function below to calculate curves for each unique pairing of dimensions and target variable (referred here as combinations) over time. The LMfit Python library provides tools for non-linear least-squared minimization and curve fitting.

#### Return function

![MPO3](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_3.png)

The variables in this formula are represented as follows (moving forward it will be referred to as the ABC model). The coefficients reflect the actual data for each input value.

- return: The result or output value (could be thought of as the "return").
- x: The input or the variable upon which the return depends.
- A: Maximum Value (Asymptote) - Represents the maximum possible value that rrr can approach. As x increases indefinitely, r will approach a. Essentially, a defines the ceiling or upper bound of the diminishing returns curve.
- B: Rate of Decay (Diminishing Factor) - The parameter b controls the rate at which the returns diminish as x increases. A larger value of b will make the returns diminish more slowly (flatter curve), while a smaller value of b will cause returns to diminish more rapidly (steeper curve). In simple terms, b dictates how quickly the curve bends downward.
- C: Steepness or Shape Factor - The parameter c affects the steepness and the shape of the curve. When c is large, the curve becomes steeper, meaning that the returns decrease more rapidly after a certain point. When c is small, the curve becomes flatter, implying that returns diminish more gradually.

The return function uses these 3 coefficients to generate an expected return for the spend value of X. The goal of the formula is to ensure that the function of spend (x) and maximum value (b) are within respect of the overall convex of the curve (c) while parsed by the midpoint (a). This function has shown previously to have the most consistent results when fitting a curve to a scatter plot to reflect a diminishing returns shape.

#### Marginal rate of return function

![MPO4](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_4.png)

The Marginal Rate of Return is a return function that uses the same A, B, and C coefficient outputs from the Diminishing Returns model to identify the cost to acquire an additional target KPI value (such as click or 1000 impressions). This metric provides some insight into the overall placement on the curve that your spend value is located and whether it’s profitable to continue spending with that specific tactic.

#### Time decay function

Used to weight the curves more heavily towards specified variables as it relates to a curve model.

![MPO5](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_5.png)

**Input variable**:

- x: This represents the input to the time decay function, which is typically time. The function calculates a decay value based on this time input. The term (x + 0.0001) ensures that the calculation avoids issues with x=0, preventing division by zero or logarithmic issues.

**Parameters**:

- a: This represents an amplitude or scaling factor for the decay function. It sets the base level of decay.
- b: Scaling parameter that adjusts how fast the decay occurs over time.
- c: Controls the shape or steepness of the decay curve. It influences how sharply the decay happens as time progresses.
- d: This is a multiplicative factor applied to the amplitude a at the beginning of the formula. It influences the initial value of the decay before the diminishing returns effect is applied.
- Threshold:  Determines when time decay starts. When set at 6, the time decay will start at the most recent 6 periods of data and continue to the most recent 6 weeks of data

**Decay equation**

- First Term: (a×d) represents an initial value for the decay before any adjustments for time xxx are applied. This is the starting point or baseline from which the decay will be subtracted.
- Second Term: introduces a diminishing effect over time. As x increases (representing the passage of time), this term reduces the overall decay.

**Impact of parameters**

- a: Determines the overall magnitude of the decay. Larger values of a will increase the decay effect.
- d: Modifies the starting level of the decay. If d is greater than 1, it amplifies the initial decay. If it's less than 1, it reduces the initial decay.
- b: Controls how quickly the decay effect kicks in. Larger values of b will make the decay more gradual, while smaller values will make the decay happen more quickly.
- c: Adjusts the shape of the decay curve. Larger values of c result in a steeper drop-off, meaning the decay happens more rapidly as x increases. Smaller values of c lead to a more gradual decay.

**Interpretation**

The `time_decay_func` calculates a decay value based on time (or another input variable) x. The function essentially subtracts a diminishing return from an initial level of decay (a×d).

- Initially, the decay is influenced by the product a×d.
- As x increases, the diminishing effect kicks in due to the second term, which reduces the overall decay value over time.
- The parameters a, b, c, and d control the magnitude, rate, and shape of this decay curve.

In practical terms, this function could be used to model a situation where the effectiveness of an event (like an advertisement or some intervention) decreases over time, with the decay following a curve shaped by the specified parameters.

### Curve functions

Least-squares minimization is commonly used for curve fitting, minimizing residuals on a curve with respect to a weight. The Python code below incorporates a time decay component as the weight, along with the ABC function and `abc_model`, which calls the lmfit model to be used in the diminishing returns curves. LMfit’s Model class wraps a model function that represents the model (without the data or weights). Parameters are then automatically identified from the model function’s named arguments. In addition, simple model functions can be readily combined and reused, and several common model functions are included in lmfit. The compute time of the lmfit model is also a major benefit with its capability of calculating hundreds of curves within minutes.

<details class="details-bar">
<summary>Click to see code: lmfit with time decay component  </summary>

```
def time_decay_func(x, a=1, b=6, c=.6, d=1.1):
    decay = (a*d)-a/(1+(b**-c)*((x+.0001)**c))
    return decay

def abc_func(x, a=1, b=1, c=-100):
    r = a/(1+(b**-(c/100))*x**(c/100))
    return r

def abc_inverse(x, a, b, c):
    r = -(x**(1-c)*(x**c+b**c)**2)/(a*b**c*c)
    return r

class abc_model(lmfit.model.Model):
    def __init__(self, independent_vars=["x"], prefix="", nan_policy="raise", **kwargs):
        kwargs.update({
            "prefix": prefix,
            "nan_policy": nan_policy,
            "independent_vars": independent_vars
        })
        super().__init__(abc_func, **kwargs)
  
    __init__.__doc__ = lmfit.models.COMMON_INIT_DOC
```

</details>

LMfit provides a flexible approach to a parameterizing a model for fitting to data using named parameters. These Parameters can be fixed, freely adjusted, or constrained between upper and lower bounds during the fitting process. The Python code uses the model bounds to ensure the curves follow a usable pathway for replicating the diminishing returns nature of media spend.

<details class="details-bar">
<summary>Click to see code: models and bounds setup  </summary>

```
# set up model and bounds

abc = abc_model()
abc.set_param_hint("a", value=10, min=5)
abc.set_param_hint("b", value=10, min=1, max=10000000)
abc.set_param_hint("c", value=-100, min=-150, max=-50)
params = abc.make_params()

targets = kpi_cols
results_dict = {}
```

</details>

| Parameter | Values                                                                               | Impact                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------- | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| a         | - Initial Value: 10 `<br>`- Minimum Value: 5                                       | - a represents the maximum value\(asymptote\) of the curve\. This means that as x \(cost\) becomes very large, the curve will approach a value close to a\.`<br>` - By setting a to 10 and restricting it to a minimum of 5, you are ensuring that the curve will not exceed 10, and will not flatten out below 5\.`<br>`-Adjusting a will change the height of the curve\. A larger aaa will stretch the curve upwards, while a smaller aaa will pull it downwards\.                                                                                                                                                                                                                                                                               |
| b         | - Initial Value: 10 `<br>` - Minimum Value: 1 `<br>` - Maximum Value: 10,000,000 | - b controls how quickly the returns diminish as x increases\.`<br>`- A smaller b will result in a curve that bends more sharply \(more rapid diminishing returns\)\. A larger b will result in a flatter curve, where the returns diminish more slowly\.`<br>`- Adjusting b affects the rate of decay\. If you increase b, the curve will flatten, meaning the returns diminish more gradually\. If you decrease b, the returns will diminish more quickly\.                                                                                                                                                                                                                                                                                       |
| c         | - Initial Value:\-100 `<br>` - Minimum Value: \-150 `<br>` - Maximum Value: \-50 | - c affects the steepness or sharpness of the curve\. The negative values indicate an inverse relationship, which means the curve will have a certain concavity\.`<br>`- With a larger negative value \(e\.g\., c=−50c = \-50c=−50\), the curve will be less steep, and the transition from increasing returns to diminishing returns will be more gradual\.`<br>` - With a smaller negative value \(e\.g\., c=−150c = \-150c=−150\), the curve will be steeper, and the returns will diminish more quickly after a certain point\.`<br>` - Adjusting c changes the sharpness of the curve\. A larger negative ccc \(closer to 0\) will make the curve less steep, and a smaller negative ccc \(more negative\) will make the curve steeper\. |

Then the code block below takes a for loop and runs an LMfit function across every ‘id’ or in this case, combinations of advertisers, product type and campaign and every target variable (or KPI in this case). It will generate a unique curve for each combination with a week’s worth of data serving as an individual point on a scatter plot.

It will save each individual curve into a folder in your Sagemaker notebook and display them below with their respective R2 and MAPE scores.

##### How adjustments affect the curve

- Increasing a stretches the curve upward, raising the maximum possible return.
- Decreasing a pulls the curve downward, lowering the maximum possible return.
- Increasing b flattens the curve, causing returns to diminish more slowly.
- Decreasing b makes the curve steeper, causing returns to diminish more quickly.
- Increasing c (less negative) makes the curve less steep, meaning the returns transition more smoothly from increasing to diminishing.
- Decreasing c (more negative) makes the curve sharper, with a faster transition to diminishing returns.

Then the code block below takes a for loop and runs an LMfit function across every ‘id’ or in this case, combinations of advertisers, product type and campaign and every target variable (or KPI in this case). It will generate a unique curve for each combination with a week’s worth of data serving as an individual point on a scatter plot.

It will save each individual curve into a folder in your Sagemaker notebook and display them below with their respective R2 and MAPE scores.

<details class="details-bar">
<summary>Click to see code: run lmfit function across all ids  </summary>

```

# iterate over each performance combination
date = datetime.datetime.today().strftime("%Y%m%d")

    abc_df = {}
    for i, p in enumerate(performance["id"].unique()):
        model_df = performance[performance["id"]==p]
    
        for t in targets:
            regression_df = model_df[["date", "cost", t]]
            regression_df.rename(columns={t: "kpi"}, inplace=True)
            regression_df["days_old"] = (regression_df["date"].max()-regression_df["date"]).dt.days
            regression_df["time_decay_weights"] = regression_df["days_old"]\
                .astype("int")\
                .apply(lambda x: time_decay_func(x))

            regression_df = regression_df[regression_df["kpi"]>0]

            # sort based on spend to correctly display at the end
            regression_df = regression_df.sort_values("cost")

            if regression_df.shape[0]<3:
                '''display(Markdown(f"Less than 3 points"))'''
                continue

            # calculate most recent 6 week average spend at cost per target
            recent_window = regression_df.sort_values(by="date").tail(6)
            recent_average_spend = recent_window["cost"].mean()
            recent_average_target = recent_window["kpi"].mean()

            if t.lower() == "impressions":
                recent_average_cost_per_target = recent_window["cost"].sum()/float(recent_window["kpi"].sum())#*1000
            else:
                recent_average_cost_per_target = recent_average_spend / recent_average_target

            # set amplitude guess for exponent = .5
            params["a"].set(
                value=regression_df["kpi"].max()*1.5,
                min=(regression_df["kpi"].max()*1.25),
                max=(regression_df["kpi"].max()*2.5))
            params["b"].set(value=regression_df["cost"].max()*.75)
            params["c"].set(min=150)

            # calculate time decay weights
            time_decay_weights = regression_df["time_decay_weights"]

            # fit and compute additional fit metrics
            r = abc.fit(
                regression_df["kpi"],
                params,
                x=regression_df["cost"],
                weights=time_decay_weights)

            mae = metrics.mean_absolute_error(r.data, r.best_fit, sample_weight=time_decay_weights)
            mape = metrics.mean_absolute_percentage_error(r.data, r.best_fit, sample_weight=time_decay_weights)
            r2 = metrics.r2_score(r.data, r.best_fit, sample_weight=time_decay_weights)

            new_id = p+"|"+t

            # store results
            x = regression_df["cost"].to_list()
            c = regression_df["time_decay_weights"].to_list()
            recent = regression_df.sort_values("date").tail(6)

            # store metrics and models in results
            results_dict[new_id] = r.best_values
            results_dict[new_id]["new_id"] = new_id
            results_dict[new_id]["id"] = p
            results_dict[new_id]["target"] = t
            results_dict[new_id]["model"] = copy.deepcopy(r)
            results_dict[new_id]["mae"] = mae
            results_dict[new_id]["mape"] = mape
            results_dict[new_id]["r2"] = r2
        
            # plot results
            r.plot(show_init=False)

            # add line for recent average cost per target
            cost = regression_df["cost"]
            recent_avg_cost_per_target = [spend/recent_average_cost_per_target for spend in x]
            c = regression_df["time_decay_weights"]
            plt.plot(cost, recent_avg_cost_per_target, ls=":", c="red", label="old method")

            # add 6 week points
            recent = regression_df.sort_values("date").tail(6)
            recent_cost = recent["cost"]
            recent_target = recent["kpi"]
            plt.plot(recent_cost, recent_target, "yo", markersize=10, markerfacecolor="white")

            # plot 6 week average
            plt.plot(recent_average_spend, recent_average_target, "r+", markersize=25)

            # format plot
            plt.title(f"{p} {t} vs cost")
            plt.ylabel(f"{t}")
            plt.xlabel("cost")

            # save plot to file
            file_name = f"{date}_mpo_{p}_{t}.png"
            curve_dir = f"curves/"
            save_to = os.path.join(curve_dir, file_name)
            plt.savefig(save_to, bbox_inches="tight")
```

</details>

The code above will iterate through each unique ID and KPI and will output a unique curve and evaluation metrics for each, as well as the corresponding A, B, C.
The code block above also includes a model amplitude guess based on the scale of the individual KPIs with the block below.

<details class="details-bar">
<summary>Click to see code: defined KPIs   </summary>

```
            # set amplitude guess for exponent = .5
            params["a"].set(
                value=regression_df["kpi"].max()*1.5,
                min=(regression_df["kpi"].max()*1.25),
                max=(regression_df["kpi"].max()*2.5))
            params["b"].set(value=regression_df["total_cost_media"].max()*.75)
            params["c"].set(min=-150)
  
```

</details>

Within the for loop of the model, we determine an amplitude guess that helps with the initial guesses for the bounds of the parameters a, b, and c in the context of fitting the curve to some data. Below is an interpretation of what that means.

**1. Setting amplitude guess for parameter a:**

Interpretation:

- Amplitude Guess: The term "amplitude" here refers to the maximum value of the curve (the asymptote).
- Initial Guess: The initial guess for a is set to 1.5×max of KPI. This means that the model assumes the maximum value of the curve will be 1.5 times the highest value in the KPI (key performance indicator) column of the regression_df DataFrame.
- Bounds: The minimum value for a is set to 1.25×max of KPI, and the maximum value is set to 2.5×max of KPI. This constrains the fitting process to look for a maximum value within this range.
- Impact: By setting these bounds, you're guiding the model to consider the KPI data when estimating the maximum value of the curve. The bounds also help the model avoid unrealistic results (e.g., an amplitude too small or too large compared to the observed KPI values).

**2. Setting initial guess for parameter b:**

- Interpretation: The initial guess for b is set to 0.75×max of `total_cost_media`. This means the model assumes that the rate of diminishing returns (controlled by b) should start around 75% of the highest value in the `total_cost_media` column of the `regression_df` DataFrame.
- Impact: The parameter b controls how quickly the returns diminish. By setting it to 75% of the maximum media spend, you're guiding the model to start with a reasonable assumption about the diminishing effect of media costs. This guess will influence the rate at which returns diminish as media costs increase.

**3. Setting bounds for parameter c:**
Interpretation: This sets a lower bound of -150 for the parameter ccc, which controls the steepness or shape of the curve.

- Impact: By setting a minimum value of -150 for c, you're allowing the curve to potentially have a sharp transition to diminishing returns, but not sharper than what a value of -150 would allow. This helps in constraining the steepness of the curve.

### Curve evaluations

With each combination, the model generates a curve graph that will visualize the overall distribution of return values of your KPIs (y axis) offset with each weekly total cost (x axis), similar to the visual below. With each curve, an adjusted R^2 value and MAPE value is provided to give a scoring metric on the overall model fit.

![MPO6](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_6.png)

<details class="details-bar">
<summary>Click to see model output </summary>

```
weighted r2: 0.75,
            weighted mae: 1991003,
            unweighted mape: 3.4,
            impressions mean: 6638017,
            impressions std: 4848806.6

    
Marked by red line and plus:

            6 week average spend: 13341,
            recent average impressions: 6260321
    
6 week average CPM: 2
[[Model]]
    Model(abc_func)
[[Fit Statistics]]
    # fitting method   = leastsq
    # function evals   = 40
    # data points      = 26
    # variables        = 3
    chi-square         = 7.3286e+13
    reduced chi-square = 3.1863e+12
    Akaike info crit   = 751.349481
    Bayesian info crit = 755.123771
    R-squared          = 0.87531664
[[Variables]]
    a:  41617090.1 +/- 1.6438e+08 (394.97%) (init = 2.524144e+07)
    b:  288503.553 +/- 3490973.77 (1210.03%) (init = 29714.77)
    c: -49.8522249 +/- 101.173889 (202.95%) (init = -50)
[[Correlations]] (unreported correlations are < 0.100)
    C(a, b) = +0.9983
    C(b, c) = -0.9671
    C(a, c) = -0.9513
  
```

</details>

In the output above, each dot represents a week of activity, , showing the overall number of clicks versus cost from the SQL query. The curve represents the statistical center of all the points in the graph, emphasizing recent data (green circles around blue dots) through a time decay component. The plus sign indicates the mean cost and clicks for the curve. The reporting metrics with R2 at 0.75, indicates that the curve is a good fit compared to the data, and the MAPE was 3.4 which is relatively low.

> [NOTE] That since the model generates a unique curve for each combination of your dimensions and KPIs, it’s important that you keep the number of dimensions and target KPIs relatively low to ensure there is enough data within each combination to draw a usable curve. This approach will also simplify drawing up the user interface for media planning.

On top of the curves values, the model will also output a table that includes each combination value and a column for A, B, and C representing the coefficient values generated from the model. If you plug these coefficients into the ABC function above along with a desired spend, to yield expected returns value for that combination with respect to the diminishing returns found in historical data.

![curve_eval](/_images/amazon-marketing-cloud/playbooks/mpo/table1_curv_eval.png)

The output shows a line item for each combination of campaigns, tactics, and ad types, along with the target KPI variable. The model generates unique A, B, and C coefficients for each combination. The Adjusted R2 and the MAPE values for the curve showing the overall quality of the curve fit to the data distribution.

This output can be incorporated to build internal user interface tool such as a dashboard or an Excel spreadsheet. Using the coefficient values and an Excel version of the ABC projection formula which will calculate in a field that can be referenced in a table that will display the projected output at a budget level.

![MPO7](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_7.png)

![MPO7a](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_7a.png)

## Optimization model

Once the curves are generated and you have the coefficients, you now have the backend MVP to start playing with various spend distribution across a proposed list of media tactic combinations to aide in media budget planning. If you are interested in seeing what will be the best distribution of spends across your tactics in pursuit of maximizing your return variable, you can incorporate an optimization model into your curve coefficients based on a specified budget and target variable.

### Optimization model constraint

![MPO8](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_8.png)

The optimization model uses a common open source Python package, scipy.optimize that focuses on maximizing the return on an ABC model with the constraints that the sum of all the X values is less than a specified budget amount.
In this case, y is the KPI value that we want to maximize given the A, B, and C constants of each combination, and the sum total of X which will represent the spend distribution, being equal to or below a certain budget threshold.

#### scipy.optimize minimize function

The minimization function model can be implemented as a separate Python notebook in which you can specify a target variable and budget utilizing the A, B and C outputs of the diminishing returns model as the constants. The best performing function uses Sequential Least Squares Programming to find the inverse minimum of a function of variables with any combination of bounds and inequality constraints.

The open-source SciPy.Optimize package is used for this purpose. In this implementation, our ABC function serves as the objective function and iterating through each A, B, and C constant to find the maximum return (or in this case, the inverse of the minimum) with respect to the budget constraint and secondary constraint.

<details class="details-bar">
<summary>Click to see code: minimization model  </summary>

```
def objective_function(x, *args):
    # Define the objective function to maximize
    # x is the vector of variables to be optimized (allocation to each observation)
    # args contains any additional parameters/constants needed for the function
  
    # Extract constants, label-to-constant mapping, and observations from args
    a, b, c, observations, penalty_factor = args
  
    # Calculate the return value to be maximized
    total_return =  np.sum([a[obs] / (1 + (b[obs] ** -c[obs]) * ((alloc) ** c[obs]))for obs, alloc in zip(observations, x)])
  
    # Calculate the penalty term to discourage allocating to more observations
    num_observations = np.sum(x > 0)  # Count the number of observations with non-zero allocations
    penalty_term = penalty_factor * num_observations
  
    # The objective is to maximize, so return the negative of total_return
    # Penalize more observations by subtracting the penalty term
    return -total_return - penalty_term

def budget_constraint(x, *y):
  
    constraint_1_min_value, constraint_1_max_value, search_value_1, constraint_2_min_value, constraint_2_max_value, search_value_2, constraint_3_min_value, constraint_3_max_value, search_value_3, constraint_4_min_value, constraint_4_max_value, search_value_4, constraint_5_min_value, constraint_5_max_value, search_value_5, constraint_6_min_value, constraint_6_max_value, search_value_6, budget, observations = y

    res1 = [index for index, substring in enumerate(observations) if  any(value in substring for value in search_value_1)]
    res2 = [index for index, substring in enumerate(observations) if  any(value in substring for value in search_value_2)]
    res3 = [index for index, substring in enumerate(observations) if  any(value in substring for value in search_value_3)]
    res4 = [index for index, substring in enumerate(observations) if  any(value in substring for value in search_value_4)]
    res5 = [index for index, substring in enumerate(observations) if  any(value in substring for value in search_value_5)]
    res6 = [index for index, substring in enumerate(observations) if  any(value in substring for value in search_value_6)]

    max_const1_allocation = x[res1]
    max_const2_allocation = x[res2]
    max_const3_allocation = x[res3]
    max_const4_allocation = x[res4]
    max_const5_allocation = x[res5]
    max_const6_allocation = x[res6]


    return (budget - sum(x), 
            constraint_1_max_value - sum(max_const1_allocation) - constraint_1_min_value, 
            constraint_2_max_value - sum(max_const2_allocation) - constraint_2_min_value, 
            constraint_3_max_value - sum(max_const3_allocation) - constraint_3_min_value, 
            constraint_4_max_value - sum(max_const4_allocation) - constraint_4_min_value, 
            constraint_5_max_value - sum(max_const5_allocation) - constraint_5_min_value, 
            constraint_6_max_value - sum(max_const6_allocation) - constraint_6_min_value)
  
def callback_function(xk):
    current_return = -objective_function(xk, *args)  # Calculate current return
    max_returns.append(current_return)
```

</details>

The following code provides an example to maximize the clicks across a bunch of tactics with a budget of $20,000 while spending no more than $20,000 on Super Power Batteries Brand.

<details class="details-bar">
<summary>Click to see code: maximize clicks  </summary>

```
# Total budget constraint
total_budget = 20000
adv_allocation = 20000

search_value = ['Super Power Batteries']

Nfeval  = 1

x = np.array([])

# Initial guess for allocations (you can adjust this based on your problem)
initial_allocations = np.ones(len(observations)) / len(observations) * total_budget

# Define bounds for allocations (optional but can be useful)
bounds = [(0, total_budget) for _ in observations]  # Each allocation should be between 0 and 1

# Setup args for the objective function
args = (a, b, c, observations)

# Setup constraints
f = lambda x: budget_constraint(x, adv_allocation, search_value, total_budget)
constraints = ({'type': 'ineq', 'fun': f})

max_returns = []

# Perform the optimization
result = minimize(objective_function, 
                  x0            = initial_allocations, 
                  method        = 'SLSQP', 
                  bounds        = bounds,
                  args          = args, 
                  constraints   = constraints, 
                  callback      = callback_function)

# Extract the optimal allocations
optimal_allocations = result.x

#print the result
for a_val, b_val in zip(observations, result.x):
    if (b_val > 0):
        print(f'{a_val}: {b_val}')
    
print("Maximum Return:", -result.fun)  

# Plot the progress of maximum return value
plt.plot(max_returns)
plt.xlabel('Iteration')
plt.ylabel('Maximum Return')
plt.title('Progress of Maximum Return Value')
plt.show()
  
```

</details>

The output is a simple list of the tactics you want to model against and the recommended spend for each, as well as the calculated return.

```
Super Power Batteries|mobile|display|dsp|bf|rt|promoted_product|views: 1686.7003676214338
Super Power Batteries|mobile|display|dsp|mf|contextual|other|views: 24.73779507258438
Super Power Batteries|desktop|display|dsp|mf|segments|im_ls|views: 7993.016679281838
Super Power Batteries|mobile|display|dsp|mf|rt|brand|views: 473.73310221327273
Super Power Batteries|desktop|display|dsp|mf|rt|sim_asin|views: 506.0256089123068
Super Power Batteries|mobile|display|dsp|mf|rt|competitive_asin|views: 1011.7393401406075
Super Power Batteries|desktop|display|dsp|mf|rt|cross_sell|views: 426.07115821098665
Super Power Batteries|mobile|display|dsp|mf|rt|cross_sell|views: 407.2172693556392
Super Power Batteries|desktop|display|dsp|mf|rt|competitive_asin|views: 1100.5750453251662
Super Power Batteries|mobile|display|dsp|mf|rt|sim_asin|views: 653.0119645828108
Super Power Batteries|desktop|display|dsp|mf|rt|brand|views: 661.2724121083951
Super Power Batteries|desktop|display|dsp|bf|rt|promoted_product|views: 696.5495372318074
Super Power Batteries|mobile|display|dsp|mf|segments|im_ls|views: 4359.34971994323
Maximum Weekly Return: 512113.9999867698
Total Spend: 20000.00000000008

```

The output from this optimization process can be utilized in two primary ways:

- Direct UI integration: It can be incorporated into a user interface to visualize budget distributions across different tactics.
- Budget planner tool input: The results can inform upcoming media spend decisions for campaigns.
  The callback function also allows you to plot the maximum weekly return clicks as it iterates through the minimization Monte Carlo steps.

![MPO9](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_9.png)

As you can see, at around the 2nd iteration step, a significant increase in the maximum value as the minimization step is finding the optimal allocations. Around step 20, you can see it hits the max point.

#### scipy.optimize Basin Hopping function

An alternative means for objective maximization is a Basin hopping function within the Scipy.Optimize package. Basin-hopping is a two phase method that combines a global stepping algorithm with local minimization at each step. It iterates through our media tactic combinations and through various allocation starting points to find the universal maximum. It can improve upon the minimization function that works by selecting a starting point and minimizing from there.

![MPO10](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_10.png)

The Basin Hopping model can use the same objective function and budget constraint function as the minimization model. The only difference is that when you call the basin hopping model you do not required defining an initial allocation or bounds.

<details class="details-bar">
<summary>Click to see code: basin hopping model for objective maximization   </summary>

```

# Total budget constraint
total_budget = 20000
adv_allocation = 20000

search_value = ['Super Power Batteries']

Nfeval  = 1

x = np.array([])

# Initial guess for allocations (you can adjust this based on your problem)
initial_allocations = np.ones(len(observations)) / len(observations) * total_budget

# Define bounds for allocations (optional but can be useful)
bounds = [(0, total_budget) for _ in observations]  # Each allocation should be between 0 and 1

# Setup args for the objective function
args = (a, b, c, observations)

# Setup constraints
f = lambda x: budget_constraint(x, adv_allocation, search_value, total_budget)
constraints = ({'type': 'ineq', 'fun': f})

# Perform the optimization
result = basinhopping(objective_function, 
                  x0    = initial_allocations, 
                  disp  = True,              
                  minimizer_kwargs = {'constraints': constraints}) 

# Extract the optimal allocations
optimal_allocations = result.x

#print the result
for a_val, b_val in zip(observations, result.x):
    if (b_val > 0):
        print(f'{a_val}: {b_val}')
    
print("Maximum Return:", -result.fun) 
```

</details>

And the output is below. You can see that the maximum return value is slightly higher than the optimization method.

```
Super Power Batteries|mobile|display|dsp|bf|rt|promoted_product|views: 1644.5057207225268
Super Power Batteries|mobile|display|dsp|mf|contextual|other|views: 49.24927379781089
Super Power Batteries|desktop|display|dsp|mf|segments|im_ls|views: 7913.271136059029
Super Power Batteries|mobile|display|dsp|mf|rt|brand|views: 464.7390494848537
Super Power Batteries|desktop|display|dsp|mf|rt|sim_asin|views: 506.18339940023947
Super Power Batteries|mobile|display|dsp|mf|rt|competitive_asin|views: 1001.8292293905135
Super Power Batteries|desktop|display|dsp|mf|rt|cross_sell|views: 412.1266323645135
Super Power Batteries|mobile|display|dsp|mf|rt|cross_sell|views: 427.31363107434544
Super Power Batteries|desktop|display|dsp|mf|rt|competitive_asin|views: 1169.887461148653
Super Power Batteries|mobile|display|dsp|mf|rt|sim_asin|views: 708.0617427702589
Super Power Batteries|desktop|display|dsp|mf|rt|brand|views: 733.6359493406524
Super Power Batteries|desktop|display|dsp|bf|rt|promoted_product|views: 625.7047652277356
Super Power Batteries|mobile|display|dsp|mf|segments|im_ls|views: 4343.492009219316
Maximum Weekly Return: 512258.30562260665
Total Spend: 20000.000000000447
```

As with the Minimization model, the output will be the combination of campaigns, ad product types and campaign types along with the optimal allocation.

To give a point of comparison, we wanted to demonstrate how the optimizer model would compare to a media allocation baseline that an advertiser may employ with their planning. We looked at the number of views garnered at each budget level if you were to evenly distribute your budget across tactics in your media flight. So, if we have 13 unique media tactics to plan across and a budget of $20,000, the Even Allocation approach assumes an even distribution of $1538.46 across each tactic.

| Even Allocation      | Scipy Minimize       | Basin Hopping        |
| -------------------- | -------------------- | -------------------- |
| 417,275 weekly views | 512,114 weekly views | 512,258 weekly Views |

 Plotting the results, we can compare the basin hopping optimization function (green dots) with the even allocation (orange dots).

![MPO11](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_11.png)

The comparison indicated that the optimization model is generating 100K more views at a 20K spend compared to the even distribution method. This is a 25% increase in projected views by employing the optimization model to help inform the media spend.

### Visualization

As the scipy package can feel like a black box that produces an output without context, it’s important to build trust in the model to confirm that the media tactics favored by the optimization model are the same media tactics with the higher return (Y axis) for the same spend (X axis).

![MPO12](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_12.png)

In this case, desktop and mobile im\_ls tactics show the highest return value for clicks relative to spend, which is reflected in the optimized budget distribution allocating them the majority of the budget. This demonstrates that the model prioritizes the steepness (verticality) of the return curves when allocating budget across tactics.

Additionally, if you’re interested in learning more about how much total budget to distribute across your tactics (i.e. what should your budget be), you can set up your optimization function as a for loop to iterate across different budget levels to find the point in which you are maximizing your weekly return with your weekly spend.

The Python code below will perform the iteration and plot the results:

<details class="details-bar">
<summary>Click to see code to iterate across budget levels  </summary>

```
# Total budget constraint
total_budget = 500000
budget_range = range(0,600000, 20000)
mobile_allocation = 100000

search_value = ['desktop']

Nfeval  = 1

max_returns = []

for budget in budget_range:


    x = np.array([])

    # Initial guess for allocations (you can adjust this based on your problem)
    initial_allocations = np.ones(len(observations)) / len(observations) * budget

    # Define bounds for allocations (optional but can be useful)
    bounds = [(0, budget) for _ in observations]  # Each allocation should be between 0 and 1

    # Setup args for the objective function
    args = (a, b, c, observations)

    # Setup constraints
    f = lambda x: budget_constraint(x, mobile_allocation, search_value, budget)
    constraints = ({'type': 'ineq', 'fun': f})

    # Perform the optimization
    result = minimize(objective_function, 
                  x0            = initial_allocations, 
                  method        = 'SLSQP', 
                  bounds        = bounds,
                  args          = args, 
                  constraints   = constraints)

    # Extract the optimal allocations
    max_returns.append(-result.fun) 

plt.plot(budget_range, max_returns, marker = 'o', label = 'Total Budgets')
highlight_index = list(budget_range).index(500000)
plt.scatter([500000], [max_returns[highlight_index]], color='red', marker='o', s=80, label='Total Budget of {total_budget}')


plt.title('Maximum Return vs Total Budget')
plt.ylabel('Maxmium Return')
plt.xlabel('Total Budget')
plt.grid(True)
plt.show()
  
```

</details>

And the plot output is below:

![MPO13](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_13.png)

Here we can see how the curve is shaping out across all the variants of media types to show the maximum return on views that can be gained on the varying budget levels ranging from 0 to 30,000. At a budget of $20,000, our optimization model is outputting a maximum number of just over 500,000 views.

## Budget planning and visualization

To demonstrate the steps, we will be taking a detailed walk-through a sample use case to answer a series of business questions to help inform a media planning budget and the specific constraints to optimize for. This visualization will take on the following steps:

- Generate the diminishing returns curves to project a return on a range of budget values
- Calculate the optimal spend considering a total all-in budget amount and additional constraints
- Output the results into a media dashboard

For this case, we will reference 6 months of historical sandbox data to optimize $100k of weekly budget towards media tactics with a max spend towards Desktop as $20k. We will build a diminishing returns curve off of the following media combinations:

- Inventory
- Device (which will house the Desktop dimension that we will include in the constraint)
- Ad Type
- Channel
- Funnel
- Tactic
- Audience

In this particular case, we will start with a query that will compile each of these dimensions along with the following target variables:

- Impressions
- Clicks
- Views
- Impression Conversions
- Click Conversions
- Spend

With respect to the applicability to project out into the future, these projection curves are able to align a projection into the future as far as the user would like. The major caveat with this is that these curves are a mathematical projection of the cost/return ratio using historical data, and not a predictive forecast model. So, with that in mind, the projection model has had the most value when planning media 1 week to 3 months into the future.

### Diminishing returns and optimization outputs

The query will have a timeline of the previous 6 months, and is recommended to query via workflow that will output the result into a data lake which can be queried directly within your Sagemaker notebooks or pipeline. The query below will compile the needed inputs and outputs to inform the diminishing returns tool.

<details class="details-bar">
<summary>Click to see SQL code: diminishing returns  </summary>

```

with traffic_and_conversion_events as (
    select
        impression_date as dim_date,
        advertiser,
        case
            when ad_product_type is null then 'dsp'
            else ad_product_type
        end as ad_product_type,
        campaign,
        creative,
        line_item,
        0 as impressions,
        0 as clicks,
        purchases,
        units_sold,
        product_sales,
        add_to_cart,
        detail_page_view,
        brand_halo_purchases,
        brand_halo_units_sold,
        brand_halo_product_sales,
        new_to_brand_product_sales,
        new_to_brand_purchases,
        new_to_brand_units_sold,
        winning_bid_cost / 100000 as winning_bid_cost,
        0 as cost
    from
        amazon_attributed_events_by_traffic_time
    union
    all
    select
        event_date as dim_date,
        advertiser,
        case
            when ad_product_type is null then 'dsp'
            else ad_product_type
        end as ad_product_type,
        campaign,
        creative,
        line_item,
        impressions as impressions,
        clicks,
        0 as purchases,
        0 as units_sold,
        0 as product_sales,
        0 as add_to_cart,
        0 as detail_page_view,
        0 as brand_halo_purchases,
        0 as brand_halo_units_sold,
        0 as brand_halo_product_sales,
        0 as new_to_brand_product_sales,
        0 as new_to_brand_purchases,
        0 as new_to_brand_units_sold,
        0 as winning_bid_cost,
        spend / 100000000 as cost
    from
        sponsored_ads_traffic
    union
    all
    select
        impression_date as dim_date,
        advertiser,
        'dsp' as ad_product_type,
        campaign,
        creative,
        line_item,
        impressions,
        0 as clicks,
        0 as purchases,
        0 as units_sold,
        0 as product_sales,
        0 as add_to_cart,
        0 as detail_page_view,
        0 as brand_halo_purchases,
        0 as brand_halo_units_sold,
        0 as brand_halo_product_sales,
        0 as new_to_brand_product_sales,
        0 as new_to_brand_purchases,
        0 as new_to_brand_units_sold,
        winning_bid_cost / 100000 as winning_bid_cost,
        total_cost / 100000 as cost
    from
        dsp_impressions
)
select
    extract(
        week
        from
            dim_date
    ) as week,
    min(dim_date) as dim_date,
    advertiser,
    ad_product_type,
    campaign,
    creative,
    line_item,
    sum(impressions) as impressions,
    sum(clicks) as clicks,
    sum(purchases) as purchases,
    sum(units_sold) as units_sold,
    sum(product_sales) as product_sales,
    sum(add_to_cart) as add_to_cart,
    sum(detail_page_view) as detail_page_view,
    sum(brand_halo_purchases) as brand_halo_purchases,
    sum(brand_halo_units_sold) as brand_halo_units_sold,
    sum(brand_halo_product_sales) as brand_halo_product_sales,
    sum(new_to_brand_product_sales) as new_to_brand_product_sales,
    sum(new_to_brand_purchases) as new_to_brand_purchases,
    sum(new_to_brand_units_sold) as new_to_brand_units_sold,
    avg(winning_bid_cost) as winning_bid_cost,
    sum(cost) as cost
from
    traffic_and_conversion_events
group by
    week,
    ad_product_type,
    advertiser,
    campaign,
    creative,
    line_item
  
```

</details>

The sample output will look like the below (which will include some of the inputs generated from a data lake):

| week | dim\_date  | advertiser           | ad\_product\_type   | campaign                                            | line\_item                                          | impressions | clicks | purchases | units\_sold | product\_sales | add\_to\_cart | detail\_page\_view | winning\_bid\_cost | cost       | customer\_hash | export\_year | export\_month | file\_last\_modified    | row\_num |
| ---- | ---------- | -------------------- | ------------------- | --------------------------------------------------- | --------------------------------------------------- | ----------- | ------ | --------- | ----------- | -------------- | ------------- | ------------------ | ------------------ | ---------- | -------------- | ------------ | ------------- | ----------------------- | -------- |
| 45   | 11/6/2023  | Iris                 | sponsored\_display  | Iris\_test\_campaign\_2311016356135                 | Iris\_test\_campaign\_2311016356135                 | 2418        | 460    | 48        | 36          | 1151\.79       | 53            | 361                | 0                  | 205\.2189  | sndbx          | 2023         | 12            | 2023\-12\-31T20\-01\-17 | 1        |
| 45   | 11/6/2023  | Kitchen Smart        | sponsored\_brands   | Kitchen Smart\_test\_campaign\_8434558048617        | Kitchen Smart\_test\_campaign\_8434558048617        | 1695        | 362    | 34        | 47          | 1181\.76       | 0             | 270                | 0                  | 174\.11777 | sndbx          | 2023         | 12            | 2023\-12\-31T20\-01\-17 | 1        |
| 2    | 1/8/2024   | Gordon's Chocolatier | sponsored\_products | Gordon's Chocolatier\_test\_campaign\_2444732030837 | Gordon's Chocolatier\_test\_campaign\_2444732030837 | 1441        | 309    | 33        | 40          | 1028\.31       | 30            | 272                | 0                  | 168\.67568 | sndbx          | 2023         | 12            | 2023\-12\-31T20\-01\-17 | 1        |
| 2    | 1/8/2024   | Jack & Jill          | sponsored\_products | Jack & Jill\_test\_campaign\_7153248131222          | Jack & Jill\_test\_campaign\_7153248131222          | 2510        | 518    | 76        | 76          | 2410\.19       | 47            | 445                | 0                  | 258\.54335 | sndbx          | 2023         | 12            | 2023\-12\-31T20\-01\-17 | 1        |
| 44   | 10/31/2023 | Gordon's Chocolatier | sponsored\_brands   | Gordon's Chocolatier\_test\_campaign\_8863355878171 | Gordon's Chocolatier\_test\_campaign\_8863355878171 | 4068        | 805    | 96        | 98          | 2571\.91       | 0             | 671                | 0                  | 454\.43633 | sndbx          | 2023         | 12            | 2023\-12\-31T20\-01\-17 | 1        |

 The table above shows an example query using sandbox data. Some dimensional inputs mentioned were not directly queryable within AMC but embedded within the campaign name and line item that were queried. The steps to extract those into dimensional inputs is below.

After cleaning the data to extract inventory, device, ad type, channel, funnel, tactic, audience, and applying target curve models and optimization steps, you obtain the following coefficients data and optimization output.

Model coefficients:

![model_coeff](/_images/amazon-marketing-cloud/playbooks/mpo/table2_model_co_eff.png)

### Optimization outputs

```
rtb|mobile|display|dsp|bf|rt|promoted_product|clicks: 61172.4669622025
rtb|mobile|display|dsp|mf|contextual|other|clicks: 33497.42456551051
rtb|desktop|display|dsp|mf|segments|im_ls|clicks: 49594.46990771672
rtb|mobile|display|dsp|mf|rt|brand|clicks: 45532.09660586958
rtb|desktop|display|dsp|mf|rt|sim_asin|clicks: 15366.922212105866
rtb|mobile|display|dsp|mf|rt|competitive_asin|clicks: 59966.228766983506
rtb|desktop|display|dsp|mf|rt|cross_sell|clicks: 8955.814882380044
rtb|mobile|display|dsp|mf|rt|cross_sell|clicks: 37944.34250128446
rtb|desktop|display|dsp|mf|rt|competitive_asin|clicks: 16304.778053507804
rtb|mobile|display|dsp|mf|rt|sim_asin|clicks: 49567.69200207549
rtb|desktop|display|dsp|mf|rt|brand|clicks: 5718.229677437304
rtb|desktop|display|dsp|bf|rt|promoted_product|clicks: 4059.7852668522874
rtb|mobile|display|dsp|mf|segments|im_ls|clicks: 112319.748596069
Maximum Weekly Return: 5314.362365359173
  
```

### Microsoft Excel model

With the model coefficients (A, B, and C), you are now about to build a tool that would allow you to project your volume of returns and marginal returns at a given spend level. This can be done within a UI tool that allows for user input fields and calculations that would support the ABC model mentioned above. For this use case, we can use a simple solution as Microsoft Excel.

The first step would be to create a table that would house all of your coefficient’s columns like the below:

![MPO14](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_14.png)

With this you can input a spend column as well as a projection column and marginal projection column with the following formulas.

![MPO15](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_15.png)

The projection cells will use the following formula:

```
=IFERROR((J3/(1+(K3^-L3)*M3^L3)),0)
```

And the marginal cost cell will use the following formula:

```
=IFERROR(-(M3^(1-L3)*(M3^L3+K3^L3)^2/(J3*K3^L3*L3)),0)
```

This will allow you to input a weekly spend figure into the spend column cell which would represent the spend value towards a specific tactic, it would generate the marginal cost per return and the projected volume based on historical data.

From here, you can reference these cells in another tab by laying out a proposed media mix and test different weekly budget combinations and see the change in the weekly return.

![MPO16](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_16.png)

To do this, you can reference the spend cell in the previous table and input the indexing match formula below:

```
=INDEX(Projections!$I$36:$I$49,MATCH(B3&C3&D3&E3&F3&G3&H3,Projections!$B$36:$B$49&Projections!$C$36:$C$49&Projections!$D$36:$D$49&Projections!$E$36:$E$49&Projections!$F$36:$F$49&Projections!$G$36:$G$49&Projections!$H$36:$H$49,0))
```

Which will match the spend with the corresponding ID combinations in the same row. Then you can do the same thing with the projected impressions, clicks, views, `impression_conversions`, and so on to take the corresponding projection and input it into your table.

```
=INDEX(Combinations!$N$3:$N$68,MATCH($B36&$C36&$D36&$E36&$F36&$G36&$H36&K$35,Combinations!$B$3:$B$68&Combinations!$C$3:$C$68&Combinations!$D$3:$D$68&Combinations!$E$3:$E$68&Combinations!$F$3:$F$68&Combinations!$G$3:$G$68&Combinations!$H$3:$H$68&Combinations!$I$3:$I$68,0),0)
```

![MPO17](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_17.png)

Then you could take the corresponding spend and volume to create your cost per kpis:

![MPO18](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_18.png)

Then use the index matching function above to reference the marginal rate of return:

![MPO19](/_images/amazon-marketing-cloud/playbooks/mpo/mpo_19.png)

As a result, you will be able to not just play with various budget amounts across your media distribution and see how the projected totals can change to impact your larger campaign goals. If you are looking to allocate some additional budget on top of your existing budget, then the marginal projections table will help to determine the relative investment to purchase one additional KPI value.

### Action

After selecting your preferred budget allocation, input the cost data into the Amazon Ads media planning tool for market activation. You can establish a regular schedule for data updates and adjust model parameters to create custom curves using the most recent data. This approach ensures your media planning remains dynamic and data-driven.
