# dtacamp_Data_Analyst_Certificate_Project

# **Data Validation**

## _Account Info_
The account_info table contains one record per customer and serves as the main customer-level dataset for this analysis. It includes profile details, subscription plan information, price paid, and churn status, which was used as the target variable in the churn analysis.

### customer_id
- No missing values were found.
- No duplicate values were identified.
- The field was treated as the unique customer identifier.

### email
- No missing values were found.
- Values appeared unique and valid.
- Retained for reference only.

### state
- 50 distinct values were found, consistent with U.S. states.
- No major cleaning was required.

### plan
- Four categories were present: Free, Basic, Pro, and Enterprise.
- These matched the documented plan tiers.
- The field was retained as a categorical variable.

### plan_list_price
- Values were numeric and non-negative.
- Prices were consistent with plan tiers.
- Retained as a numeric feature.

### churn_status
- Churned users were recorded as “Y”, while non-churned users were blank.
- The field was recoded into a binary churn flag for analysis.

### Methodological Decision: Churn Normalization
To prepare the churn field for analysis, churn_status was recoded into a binary variable:

- 0 = Active customers (blank values in the original field) 
- 1 = Churned customers ("Y" in the original field)


## _Customer Support_
The customer_support table contains support ticket records and captures interactions between customers and the support team. This dataset was used to assess customer friction through ticket volume, support topic, and resolution time.

### ticket_time
- No missing values were found.
- All records contained valid timestamps.
- The field was converted to datetime format.
- The observed date range spans from June 2025 to December 2025.
- According to the Product Manager, these timestamps are recorded in Pacific Time.

### user_id
- No missing values were found.
- There were 367 unique users across 918 tickets.
- This indicates that many users submitted more than one ticket.
- In this table, the identifier is numeric, whereas in account_info it follows a string format (for example, "C10000").
- This field can be used for merging after identifier standardization.

### channel
- No missing values were found.
- Four categories were identified: email, chat, phone, and -.
- The - value is not a valid support channel and likely represents missing or unrecorded data. It should be recoded as Unknown or Missing.

### topic
- No missing values were found.
- Four categories were identified: billing, other, technical, and account.
- No major cleaning was required.

### resolution_time_hours
- No missing values were found.
- The field is numeric, with no negative values or zeros identified.
- It was retained as a numeric variable for later analysis.

### state
- No missing values were found.
- The field contains only binary values (0 and 1).
- Unlike the state field in account_info, this field does not represent geographic location.
- Because the field name is ambiguous, it should be renamed before analysis to avoid confusion.

### comments
- A large number of missing values were present, which is expected for a free-text field.
- Some repeated strings suggest standardized requests or recurring support issues.
- This field may contain sensitive privacy-related requests and should not be used directly in visualizations.
- If needed, it can be transformed into a binary flag, such as privacy_deletion_request.


## _User_activity_
The user_activity table contains event-level records describing how customers interacted with the platform. This dataset was used to derive engagement-based features for the churn analysis.

### event_time
- No missing values were found.
- All timestamps appeared valid.
- The field was converted to datetime format.
- The observed period ranges from June 2025 to December 2025.
- This time window is aligned with the customer_support table.

### user_id
- No missing values were found.
- There were 246 unique users in this table.
- All identified users also existed in the account_info table.
- The identifiers are numeric and required standardization to match the account-level ID format.
- Only 246 out of 400 total customers appeared in this activity log, meaning that 154 customers (38.5%) had no recorded events.
- This absence may reflect either low engagement or data capture gaps.

### event_type
- No missing values were found.
- Four distinct event types were identified: read_article, watch_video, track_workout, and share_workout.
- The event labels were consistent and interpretable.
- These categories support the distinction between content consumption and more active product usage.


# **Exploratory Analysis**

## _Customer distribution by plan_
![Captura de tela 2026-04-06 171658](Captura%20de%20tela%202026-04-06%20171658.png)
- The customer base is distributed across four subscription tiers. Basic has the largest number of customers, followed by Free, Enterprise, and Pro. This shows that the customer base is not evenly distributed across plans, which is important when interpreting churn later as a rate rather than only in absolute numbers.

## _Count of customer_id by churn_flag_
![Captura de tela 2026-04-06 173116](Captura%20de%20tela%202026-04-06%20173116.png)
- The churn status distribution shows that most customers remain active. According to the chart, 286 customers (71.5%) are active, while 114 customers (28.5%) have churned. This suggests that although retention is stronger overall, churn still affects a substantial share of the customer base and should be treated as an important business issue.

## _Churn Rate by plan_
![Captura de tela 2026-04-06 173652](Captura%20de%20tela%202026-04-06%20173652.png)      
- Churn is not evenly distributed across subscription tiers. The Free plan has the highest churn rate at 40.95%, followed by Enterprise at 26.09%, Basic at 23.73%, and Pro at 22.35%. This suggests that plan type is strongly associated with retention outcomes, with the Free plan representing the highest-risk customer segment.

## _Churn Rate by state_
![Captura de tela 2026-04-06 174121](Captura%20de%20tela%202026-04-06%20174121.png)
- The state-level map shows that churn is not evenly distributed across the United States. Some states, such as New Jersey and Michigan, display particularly high churn rates, while others show more moderate or relatively low churn levels. However, this geographic variation should be interpreted with caution, as some states may have a small number of customers, which can make churn rates appear more extreme. For this reason, the map is useful as a complementary view, but plan-level churn patterns appear to provide a more reliable basis for business decisions.


# **Definition of a Metric for the Business to Monitor**

## _Recommended metric: Churn Rate_
The most important metric for Fit.ly to monitor is churn rate, defined as the proportion of customers who have cancelled their subscription during the observed customer base.

Churn Rate = Number of Churned Customers / Total Number of Customers

This metric is the most appropriate for the business problem because it directly measures customer loss, which is the main issue highlighted by leadership. Since Fit.ly operates on a subscription-based model, churn has a direct impact on recurring revenue, customer lifetime value, and acquisition pressure.

## _How the business should use this metric_
Fit.ly should monitor churn rate at two levels:

1. **Overall churn rate**:
This provides a high-level view of whether retention is improving or worsening over time.
2. **Churn rate by plan**:
This helps identify which customer segments are most at risk and where retention efforts should be prioritized.

Tracking only the overall churn rate may hide important differences between subscription tiers. In this analysis, churn was not evenly distributed across plans, which means the business should use segmented churn monitoring to support more targeted interventions.

A practical approach would be to review churn rate on a regular basis, such as monthly or quarterly, and compare both:

- overall churn trend over time;
- churn rate by subscription plan.

This would allow leadership to quickly identify whether retention actions are working and whether specific plans require additional attention.

## _Estimated initial value based on the current data_

Based on the current dataset, the estimated overall churn rate is:

- 114 churned customers / 400 total customers = 28.5%

At the plan level, the current churn rates are:

- **Free**: 40.95%
- **Enterprise**: 26.09%
- **Basic**: 23.73%
- **Pro**: 22.35%

These values suggest that the Free plan is the most vulnerable segment and should be treated as the highest priority for retention monitoring.

## _Interpretation_

The initial values indicate that churn is a meaningful business issue for Fit.ly. While the majority of customers are still active, an overall churn rate of 28.5% is substantial for a subscription-based company. The much higher churn rate in the Free plan also suggests that customer retention challenges are not evenly distributed across the customer base. For that reason, Fit.ly should monitor churn both as an overall KPI and as a segmented metric by plan.


# **Final Summary and Recommendations**

Fit.ly’s churn analysis shows that customer loss is a meaningful business issue and that it is not evenly distributed across the customer base. The analysis found that churn is present at an overall rate of 28.5%, which is substantial for a subscription-based business. Although most customers remain active, the size of the churned segment is large enough to create pressure on recurring revenue and long-term growth.

The strongest pattern identified in the analysis is the difference in churn across subscription plans. The Free plan has the highest churn rate at 40.95%, well above the rates observed in Enterprise (26.09%), Basic (23.73%), and Pro (22.35%). This suggests that plan type is strongly associated with retention outcomes, and that churn is concentrated more heavily in lower-commitment users.

The geographic analysis showed that churn also varies across states, but those results should be interpreted with caution because some states may have relatively small customer counts. For that reason, state-level differences are better treated as a complementary signal rather than the main basis for decision-making. Overall, the most reliable finding is that churn differs significantly by plan, especially in the Free tier.

## Recommendations

Based on these findings, Fit.ly should take the following actions:

1. _Prioritize retention efforts in the Free plan_

The Free plan has the highest churn rate and should be treated as the first priority for intervention. Fit.ly should investigate why free users disengage or leave at a much higher rate, and consider strategies such as onboarding improvements, clearer value communication, feature education, or targeted conversion campaigns.

2. _Monitor churn rate regularly, both overall and by plan_

Leadership should track churn rate as a core KPI on a recurring basis. Monitoring only the overall churn rate may hide important differences between customer segments, so plan-level churn monitoring should be part of the regular reporting process.

3. _Design retention strategies by customer segment_

Since churn is not evenly distributed across plans, Fit.ly should avoid applying the same retention strategy to all customers. Instead, the business should tailor retention efforts according to plan type, with greater attention given to segments showing the highest churn risk.

4. _Continue investigating support and engagement signals_

This analysis established a strong customer-level view of churn, but support behavior and user activity should continue to be explored in future work to better understand why some users leave. This may help Fit.ly identify more detailed churn drivers and improve intervention timing.

## Conclusion

In summary, churn at Fit.ly is significant enough to justify focused business action. The clearest finding is that the problem is especially pronounced in the Free plan, which should become the main target of near-term retention efforts. By monitoring churn rate consistently and acting on segment-level differences, Fit.ly can build a more targeted and effective retention strategy.
