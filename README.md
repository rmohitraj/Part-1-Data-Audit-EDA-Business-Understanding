# Part-1-Data-Audit-EDA-Business-Understanding
# Customer Churn Analysis and Retention Strategy Investigation

## Overview
This notebook conducts an in-depth exploratory data analysis (EDA) to understand the factors contributing to customer churn for a direct-to-consumer (D2C) business. The goal is to identify key churn drivers and formulate hypotheses to guide future customer retention strategies.

## Project Structure
The notebook is structured into the following main sections:

1.  **Data Loading and Initial Inspection**: Loading various datasets (customers, orders, support tickets, web events, churn labels, intervention history, RFM snapshot) and displaying their initial rows.
2.  **Data Cleaning and Preprocessing**: 
    *   Checking for missing values across all dataframes.
    *   Identifying and reporting duplicate records.
    *   Performing basic checks for invalid or unusual values in key columns (e.g., negative gross amounts, discount percentages, sentiment scores).
    *   Detecting outliers using the IQR method for numerical columns.
    *   Verifying `customer_id` consistency across all related dataframes.
    *   Converting date columns to datetime objects and checking for parsing errors and date ranges.
3.  **Exploratory Data Analysis (EDA)**:
    *   **Customer Demographics/Profile Fields**: Analyzing distributions of city tier, age group, and acquisition channel.
    *   **Order Behavior**: Examining distributions of order category, quantity, gross amount, discount percentage, delivery days, and ratings.
    *   **Monetary Behavior**: Calculating and visualizing total spending per customer, and the Average Order Value (AOV).
    *   **Support Ticket Issues**: Analyzing distributions of issue types, support channels, sentiment scores, reopened tickets, and resolution hours.
    *   **Return/Refund Behavior**: Analyzing returned orders distribution, gross amount for returned vs. non-returned orders, and top categories with highest returns.
    *   **Web/App Activity**: Analyzing distributions of sessions, product views, cart additions, and login days in the last 30 days.
    *   **Campaign or Intervention History**: Analyzing distributions of last campaign received, last campaign cost, and campaign priority.
    *   **Churn Distribution**: Visualizing the overall churn rate (approximately 47%).
4.  **Churn Risk Hypotheses**: Formulating five specific hypotheses about potential churn drivers, each supported by relevant visualizations and explanations.
5.  **Business Memo: Pre-Campaign Retention Strategy Investigation**: A memo outlining key areas for further investigation based on the EDA and hypotheses to inform effective retention campaigns.

## Key Findings

*   **High Churn Rate**: The business faces a significant churn rate of approximately 47%.
*   **Web/App Engagement**: Churned customers consistently show lower web/app engagement (sessions).
*   **Age Groups**: Churn rates vary across different age groups, suggesting demographic-specific vulnerabilities.
*   **Acquisition Channels**: The acquisition channel appears to influence customer retention, with some channels potentially bringing in higher-churn-risk customers.
*   **Monetary Value**: Customers with lower total spending are more likely to churn, indicating a link between financial investment and retention.
*   **Support Issues**: Certain types of support tickets (e.g., technical issues) are associated with higher churn rates, pointing to specific pain points driving dissatisfaction.

## How to Run the Notebook

This notebook is designed to be run in Google Colab.

1.  **Open in Google Colab**: Upload or open the `.ipynb` file in Google Colab.
2.  **Mount Google Drive**: The notebook expects the data files to be located in your Google Drive. Ensure your Google Drive is mounted. The first code cell (`cell_id: 693488b9`) handles this:
    ```python
    from google.colab import drive
    drive.mount('/content/drive')
    ```
    If it's not already mounted, you will be prompted to authorize Google Drive access.
3.  **Data Path**: Update the `base_path` variable in the 'Data Loading and Initial Inspection' section (`cell_id: 59301751`) to point to the correct location of your `d2c churn data package/` directory within Google Drive. For example:
    ```python
    base_path = '/content/drive/MyDrive/Capstone Project/d2c churn data package/'
    ```
    Make sure the `d2c churn data package/` folder containing all the CSV files is present at this path.
4.  **Run Cells Sequentially**: Execute all cells in the notebook sequentially from top to bottom. The notebook is designed to be run in order, as subsequent cells depend on the output and variables defined in previous cells.

## Data Sources
The analysis uses several CSV files, which should be placed in the `d2c churn data package/` directory:

*   `customers.csv`
*   `orders.csv`
*   `support_tickets.csv`
*   `web_events_snapshot.csv`
*   `churn_labels.csv`
*   `intervention_history.csv`
*   `rfm_modeling_snapshot.csv`

## Next Steps
Based on the business memo and the initial EDA, the next steps involve conducting deeper investigations into the identified areas to move from correlation to causation. This includes:

*   **Detailed Root Cause Analysis**: For specific support issues and engagement drops.
*   **CLTV and Retention Rate Analysis by Acquisition Channel**: To optimize marketing spend.
*   **Micro-segmentation of Low-Spending Customers**: To understand their unique barriers and potential retention strategies.

These insights will inform the design of highly targeted and effective retention campaigns, aiming to improve customer loyalty and reduce overall churn.
