### CONVERSION RATE TABLE
---

```sql
USE signup_flow;
SET SESSION sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));

-- Sign Up conversion rate from July 1, 2022 to January 2023 (all successful registrations) 
-- Use tables: Visitors (Total Purchases) / Students (All registered users - are students so will be used interchangeably) / Student Purchase (Free registered users) 
-- Info we need: Common Table Expression CTE -- with statements (Total Visitors, All registered Users, Free Registered Users) 

-- Create a Table to be used for the following queries 
WITH total_visitors AS (
    SELECT 
        v.visitor_id,
        v.first_visit_date,
        s.date_registered AS registration_date,
        p.purchase_date
    FROM visitors v
    LEFT JOIN students s ON v.user_id = s.user_id
    LEFT JOIN student_purchases p ON v.user_id = p.user_id
    GROUP BY visitor_id
), 

-- Create a subquery known as count_visitors which will return the total visitors grouped by dates
count_visitors AS (
    SELECT first_visit_date AS date_session,
           COUNT(*) AS count_total_visitors
    FROM total_visitors
    GROUP BY date_session
),

-- Create a subquery known as count_registered which will return the total registered visitors grouped by dates
count_registered AS (
    SELECT first_visit_date AS date_session,
           COUNT(*) AS count_registered
    FROM total_visitors
    WHERE registration_date IS NOT NULL
    GROUP BY date_session
),

-- Create a subquery known as count_registered_free which will return the total registered visitors excluding any direct purchases
count_registered_free AS (
    SELECT first_visit_date AS date_session,
           COUNT(*) AS count_registered_free
    FROM total_visitors
    WHERE registration_date IS NOT NULL
      AND (purchase_date IS NULL OR TIMESTAMPDIFF(MINUTE, registration_date, purchase_date) > 30) -- distinguishes free users from paying users
    GROUP BY date_session
) 

-- Return all subqueries joined together
SELECT
    v.date_session AS date_session,
    v.count_total_visitors,
    IFNULL(r.count_registered, 0) AS count_registered,
    IFNULL(fr.count_registered_free, 0) AS free_registered_users
FROM count_visitors v
LEFT JOIN count_registered r ON v.date_session = r.date_session
LEFT JOIN count_registered_free fr ON v.date_session = fr.date_session
WHERE v.date_session < '2023-02-01'
ORDER BY v.date_session;
---



ORDER BY login_date;

