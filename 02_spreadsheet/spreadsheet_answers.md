## Cleaning Steps
Whitespace Removal:Trimmed leading and trailing spaces from merchant name, status, and gateway region.

Status Normalization:Unified fragmented status labels (e.g., "CAPTURED", "Captured ", "captured") into a single lowercase captured state.

Missing Value Imputation:Identified 9 missing values in gateway region and 1 missing value in risk score, replacing them with the placeholder "blank" to maintain data integrity without dropping rows.

Case Consistency:Applied title casing to merchant name and uppercase formatting to gateway region.

## Standardization Rules
Date Formatting:Standardized the transaction_date from YYYY-MM-DD to YYYY/MM/DD for uniform reporting.

Status Categorization:Map complex failure descriptions (e.g., "failed e05 timeout") to a simplified "failed" status.

Regional Grouping:Unified casing for regions (e.g., "apac" to "APAC") and categorized all undefined entries as "blank".

Currency Unification:Ensured all currency codes (INR, EUR, USD) followed ISO standards for lookup compatibility.

## Lookup and Enrichment Logic
USD Conversion:Integrated the exchange_rates master table to fetch daily rates. Calculated amount USD by multiplying raw amount by the specific usd rate for that date and currency.

High Value Flagging:Implemented a business rule to flag transactions where the converted amount USD exceeds $5,000.

High Risk Flagging:Implemented a risk threshold to flag transactions with a risk score greater than 70.

Merchant Metadata:Linked transactions to the merchant master to ensure consistent naming and categorization.

## Final Answers
Total raw rows:                      30
Total cleaned rows:                  30
Invalid or missing rows handled:     10 (9 Region, 1 Risk)
Top region by GMV:                   APAC ($35,954.50)
Number of high value transactions:   5
Number of high risk transactions:    10
Top merchant by captured GMV:        Beta Stores ($33,431.00)

## Formula Samples
Amount in USD:
= [raw_amount] * VLOOKUP([transaction_date] & [currency], exchange_rates_table, 3, FALSE)

High Value Flag (USD > 5000):
= IF([amount_USD] > 5000, 1, 0)

High Risk Flag (Risk Score > 70):
= IF(VALUE(SUBSTITUTE([risk_score], "score:", "")) > 70, 1, 0)