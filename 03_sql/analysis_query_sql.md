--Q1
SELECT status,
COUNT(*) AS transaction_cont
FROM cleaned_transactions
GROUP BY status;

--Q2
SELECT merchant_name,
SUM(amount_usd) as total_captured_gmv 
FROM cleaned_transactions
WHERE status = 'captured' 
GROUP BY merchant_name;

--Q3
SELECT merchant_name, 
SUM(amount_usd) as total_captured_gmv 
FROM cleaned_transactions 
WHERE status = 'captured' 
GROUP BY merchant_name 
ORDER BY total_captured_gmv 
DESC LIMIT 10;

--Q4
SELECT transaction_date, 
SUM(amount_usd) as daily_gmv, 
COUNT(*) as success_count 
FROM cleaned_transactions 
WHERE status = 'captured' 
GROUP BY transaction_date;

--Q5
SELECT merchant_name, 
(COUNT(CASE WHEN status = 'chargeback' THEN 1 END) * 1.0 / COUNT(*)) as cb_ratio
FROM cleaned_transactions GROUP BY merchant_name HAVING cb_ratio > 0.01;

--Q6
SELECT gateway_region, 
AVG(risk_score) as avg_risk 
FROM cleaned_transactions 
GROUP BY gateway_region 
HAVING avg_risk > 50 AND COUNT(*) > 5;

--Q7
SELECT user_id, transaction_date, 
COUNT(*) as failed_count 
FROM cleaned_transactions 
WHERE status IN ('failed', 'chargeback')
GROUP BY user_id, transaction_date 
HAVING failed_count >= 3;

--Q8
SELECT merchant_name, 
COUNT(*) as cb_count, 
COUNT(DISTINCT user_id) as unique_users, 
SUM(amount_usd) as cb_amount
FROM cleaned_transactions 
WHERE status = 'chargeback' 
GROUP BY merchant_name;