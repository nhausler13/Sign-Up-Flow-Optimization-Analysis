### LOGIN FLOW
---
```sql
USE signup_flow;

-- Adjust SQL mode to allow grouping
SET SESSION sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));

-- Query to analyze login attempts and errors
SELECT 
    ac.session_id,                              -- alias: ac
    CAST(s.date_registered AS DATE) AS registration_date,  -- cast registration as date
    CAST(ac.action_date AS DATE) AS login_date,            -- cast action_date as date
    
    CASE
        -- Determine login type based on action_name
        WHEN ac.action_name LIKE '%facebook%' THEN 'facebook'
        WHEN ac.action_name LIKE '%linkedin%' THEN 'linkedin'
        WHEN ac.action_name LIKE '%google%' THEN 'google'
        ELSE 'email'
    END AS login_type,
    
    CASE
        -- Direct success: login successful on first attempt
        WHEN ac.action_name LIKE '%success%' 
             AND s.date_registered IS NOT NULL
             AND CAST(s.date_registered AS DATE) = CAST(ac.action_date AS DATE)
        THEN 'direct_success'
        
        -- Failed attempt: login failed and user not registered
        WHEN ac.action_name LIKE '%fail%' 
             AND s.date_registered IS NULL
        THEN 'fail'
        
        -- Successful retry: failed initially but succeeded later
        WHEN ac.action_name LIKE '%fail%' 
             AND s.date_registered IS NOT NULL
             AND CAST(s.date_registered AS DATE) >= CAST(ac.action_date AS DATE)
        THEN 'successful_retry'
    END AS login_attempt,
    
    -- Capture error messages for failed login attempts
    IFNULL(e.error_text, ' ') AS error_message,
    
    -- Categorize visitors by device type
    se.session_os,
    CASE
        WHEN se.session_os LIKE '%android%' OR se.session_os LIKE '%iOS%' THEN 'mobile'
        WHEN se.session_os LIKE '%Windows%' OR 
             se.session_os LIKE 'OS%' OR
             se.session_os LIKE '%Linux%' OR
             se.session_os LIKE '%Ubuntu%' OR
             se.session_os LIKE '%Chrome%'
        THEN 'desktop'
        ELSE 'other'
    END AS DEVICE

FROM actions ac
LEFT JOIN visitors v ON v.visitor_id = ac.visitor_id        -- join actions with visitors
LEFT JOIN students s ON s.user_id = v.user_id              -- join students with visitors
LEFT JOIN error_messages e ON e.error_id = ac.error_id     -- join error messages
LEFT JOIN sessions se ON se.visitor_id = ac.visitor_id     -- join sessions table

WHERE 
    ac.action_name LIKE '%login%'           -- login action
    AND ac.action_name LIKE '%click%'       -- login click
    AND (ac.action_name LIKE '%success%' OR ac.action_name LIKE '%fail%')  -- success or fail
    AND v.first_visit_date >= '2022-07-01' 
    AND ac.action_date BETWEEN '2022-01-01' AND '2023-02-01'

GROUP BY session_id
HAVING login_attempt IS NOT NULL
   AND registration_date <= login_date
