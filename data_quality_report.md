# Data Quality Report

This report summarizes the data quality assessment and preprocessing steps performed on the D2C churn dataset. The analysis aimed to identify and address common data issues such as missing values, duplicates, invalid entries, and inconsistencies, ensuring the data is robust for further exploratory analysis and model building.

## 1. Missing Values

Missing values were systematically checked across all loaded dataframes:

-   **`customers_df`**: `loyalty_tier` column found to have missing values. (Output suggests 1386 missing values).
-   **`rfm_modeling_snapshot_df`**: `loyalty_tier`, `preferred_category`, `hair_type`, and `skin_type` columns found to have missing values.
-   **Other Dataframes**: `orders_df`, `support_tickets_df`, `web_events_snapshot_df`, `churn_labels_df`, and `intervention_history_df` reported no missing values.

**Handling**: Missing `loyalty_tier` values were noted. For `rfm_modeling_snapshot_df`, the missing values in `preferred_category`, `hair_type`, and `skin_type` were also noted. Further handling (imputation or removal) would be context-dependent for specific analyses, but for this EDA, they were kept as is for initial observation.

## 2. Duplicate Records

A check for duplicate rows was performed across all dataframes. No duplicate records were found in any of the following dataframes:

-   `customers_df`
-   `orders_df`
-   `support_tickets_df`
-   `web_events_snapshot_df`
-   `churn_labels_df`
-   `intervention_history_df`
-   `rfm_modeling_snapshot_df`

**Handling**: No action required as no duplicates were found.

## 3. Invalid or Unusual Values (Basic Checks)

Several checks were performed to identify values outside expected ranges or with unusual distributions:

-   **Orders with negative `gross_amount`**: No orders were found with a negative `gross_amount`.
-   **`age_group` distribution in `customers_df`**: The distribution of `age_group` was inspected, revealing categories such as '18-24', '25-34', '35-44', '45-54', and '55+'. The distribution appeared reasonable.
-   **Orders with `discount_pct` outside [0, 1]**: No orders were found with `discount_pct` values less than 0 or greater than 1.
-   **Support tickets with `sentiment_score` outside [-1, 1]**: No support tickets were found with `sentiment_score` values outside the expected range of -1 to 1.

**Handling**: No invalid values were detected in these specific checks, indicating good data integrity for these fields.

## 4. Outliers

Outliers were detected using the Interquartile Range (IQR) method for numerical columns in `orders_df` and `web_events_snapshot_df`.

-   **`orders_df`**: Outliers were found in `quantity`, `gross_amount`, `discount_pct`, `delivery_days`, and `rating`. These outliers typically represent unusually high or low values (e.g., very large orders, high discounts, long delivery times, extreme ratings).
-   **`web_events_snapshot_df`**: Outliers were detected in `sessions_30d`, `product_views_30d`, `cart_adds_30d`, `purchases_30d`, `support_pages_visited_30d`, and `login_days_30d`. These could correspond to highly engaged or disengaged users.

**Handling**: Outliers were identified but not removed or transformed at this stage, as their impact on different analyses (e.g., averages vs. robust statistics) needs further consideration. For certain modeling tasks, Winsorization or robust scaling might be applied.

## 5. Join/Key Issues (`customer_id` Consistency)

The consistency of `customer_id` across all related dataframes was verified against the master `customers_df`.

-   The total unique `customer_ids` in `customers_df` was 2400.
-   All `customer_ids` present in `orders_df`, `support_tickets_df`, `web_events_snapshot_df`, `churn_labels_df`, `intervention_history_df`, and `rfm_modeling_snapshot_df` were found to be present in `customers_df`.

**Handling**: This confirms referential integrity for `customer_id` across the datasets, ensuring reliable merging and relational analysis.

## 6. Date Consistency Issues

All date columns were converted to datetime objects, and checks for parsing errors and date ranges were performed:

-   **`customers_df` (`signup_date`)**: Successfully parsed. Date Range: 2024-01-01 to 2025-09-15.
-   **`orders_df` (`order_date`)**: Successfully parsed. Date Range: 2024-01-09 to 2025-11-29.
-   **`support_tickets_df` (`ticket_date`)**: Successfully parsed. Date Range: 2024-01-13 to 2025-09-30.
-   **`web_events_snapshot_df` (`snapshot_date`)**: Successfully parsed. Date Range: 2025-09-30 to 2025-09-30.
-   **`churn_labels_df` (`snapshot_date`)**: Successfully parsed. Date Range: 2025-09-30 to 2025-09-30.
-   **`intervention_history_df` (`snapshot_date`)**: Successfully parsed. Date Range: 2025-09-30 to 2025-09-30.
-   **`rfm_modeling_snapshot_df` (`signup_date`, `snapshot_date`)**: `signup_date` was not re-checked for range, `snapshot_date` was successfully parsed. Date Range: 2025-09-30 to 2025-09-30.
-   **Orders DataFrame: Negative Delivery Days Check**: No negative `delivery_days` were found, indicating valid durations.

**Handling**: Dates were successfully converted without parsing errors, and date ranges were verified to be logical.

## 7. Data Leakage Considerations

Several columns were identified as potential sources of data leakage if not handled carefully, particularly in a predictive modeling context:

-   **`churn_next_60d` in `churn_labels_df`**: This is the target variable and should only be used for labels, not as a feature.
-   **`snapshot_date`**: Direct use as a feature might leak temporal information. It's better to derive time-based features (e.g., days since last interaction).
-   **`last_campaign_received`, `last_campaign_cost`, `campaign_response_rate` in `intervention_history_df`**: These could leak future information if the intervention occurred after the feature observation window but before the churn label observation window.
-   **`returned` in `orders_df`**: If the return status is known post-prediction point for churn, it would cause leakage.
-   **Aggregated features derived from future data**: Any features aggregated over a period extending beyond the feature snapshot date would leak information.

**Handling**: Awareness of these potential leakage sources is critical during feature engineering for any predictive modeling tasks. Features must be derived from data available *up to or at* the `snapshot_date` to prevent future information from influencing predictions.
