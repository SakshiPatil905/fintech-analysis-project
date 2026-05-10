#SQL Answers

##Q1
###Query
SELECT status,
COUNT(*) AS transaction_cont
FROM cleaned_transactions
GROUP BY status;

###Result Summary
| status     | transaction_cont |
| ---------- | ---------------- |
| captured   | 19               |
| failed     | 7                |
| chargeback | 4                |

---
##Q2
###Query
SELECT merchant_name,
SUM(amount_usd) as total_captured_gmv 
FROM cleaned_transactions
WHERE status = 'captured' 
GROUP BY merchant_name;

###Result Summary
| merchant_name | total_captured_gmv |
| ------------- | ------------------ |
| Alpha Mart    | 29984.5            |
| Beta Stores   | 33431.0            |
| City Pharma   | 8640.0             |
| Delta Travels | 10300.0            |

---
##Q3
###Query
SELECT merchant_name, 
SUM(amount_usd) as total_captured_gmv 
FROM cleaned_transactions 
WHERE status = 'captured' 
GROUP BY merchant_name 
ORDER BY total_captured_gmv 
DESC LIMIT 10;

###Result Summary
| merchant_name | total_captured_gmv |
| ------------- | ------------------ |
| Beta Stores   | 33431.0            |
| Alpha Mart    | 29984.5            |
| Delta Travels | 10300.0            |
| City Pharma   | 8640.0             |

---
##Q4
###Query
SELECT transaction_date, 
SUM(amount_usd) as daily_gmv, 
COUNT(*) as success_count 
FROM cleaned_transactions 
WHERE status = 'captured' 
GROUP BY transaction_date;

###Result Summary
| transaction_date | daily_gmv | success_count |
| ---------------- | --------- | ------------- |
| 2026/03/01       | 26382.0   | 5             |
| 2026/03/02       | 11080.0   | 3             |
| 2026/03/03       | 16031.5   | 4             |
| 2026/03/04       | 13920.0   | 4             |
| 2026/03/05       | 6136.0    | 1             |
| 2026/03/06       | 8806.0    | 2             |

---
##Q5
###Query
SELECT merchant_name, 
(COUNT(CASE WHEN status = 'chargeback' THEN 1 END) * 1.0 / COUNT(*)) as cb_ratio
FROM cleaned_transactions GROUP BY merchant_name HAVING cb_ratio > 0.01;

###Result Summary
| merchant_name | cb_ratio |
| ------------- | -------- |
| Alpha Mart    | 0.09091  |
| Beta Stores   | 0.09091  |
| Eco Home      | 0.5      |
| Delta Travels | 0.25     |

---
##Q6
###Query
SELECT gateway_region, 
AVG(risk_score) as avg_risk 
FROM cleaned_transactions 
GROUP BY gateway_region 
HAVING avg_risk > 50 AND COUNT(*) > 5;

###Result Summary
| gateway_region | avg_risk          |
| -------------- | ----------------- |
| APAC           | 67.53846153846153 |
|                | 62.125            |

---
##Q7
###Query
SELECT user_id, transaction_date, 
COUNT(*) as failed_count 
FROM cleaned_transactions 
WHERE status IN ('failed', 'chargeback')
GROUP BY user_id, transaction_date 
HAVING failed_count >= 3;

###Result Summary
| user_id | transaction_date | failed_count |
| ------- | ---------------- | ------------ |
| U008    | 2026/03/05       | 4            |

---
##Q8
###Query
SELECT merchant_name, 
COUNT(*) as cb_count, 
COUNT(DISTINCT user_id) as unique_users, 
SUM(amount_usd) as cb_amount
FROM cleaned_transactions 
WHERE status = 'chargeback' 
GROUP BY merchant_name;

###Result Summary
| merchant_name | cb_count | unique_users | cb_amount |
| ------------- | -------- | ------------ | --------- |
| Alpha Mart    | 1        | 1            | 5400.0    |
| Beta Stores   | 1        | 1            | 1711.0    |
| Delta Travels | 1        | 1            | 2500.0    |
| Eco Home      | 1        | 1            | 6649.0    |

---