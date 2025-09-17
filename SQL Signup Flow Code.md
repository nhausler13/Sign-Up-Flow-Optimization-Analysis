### SIGN UP FLOW
---

```sql
-- Create a query to identify Preferred sign-up methods
-- Obtain sign-up errors  

-- Steps: 
-- 1. Count the number of clicks on each sign-up option
-- 2. Distinguish between successful and unsuccessful clicks 

-- Quick overview of columns in the actions table
-- SELECT * FROM actions;  
-- Action_name column keeps track of various clicks a visitor makes on the website (signup/email/google/facebook/linkedin)

SELECT 
    visitor_id,
    action_name,
    action_date
FROM actions
WHERE action_name LIKE '%sign%' 
  AND action_name LIKE '%success%';

-- Main query for sign-up flow analysis
SELECT 
    ac.visitor_id,               -- alias is ac
    s.user_id,                   -- alias is s
    CAST(s.date_registered AS DATE) AS registration_date, -- cast registration as date
    CAST(ac.action_date AS DATE) AS signup_date,          -- cast action_date as date
    CASE 
        -- Determine signup type based on action_name
        WHEN ac.action_name LIKE '%facebook%' THEN 'facebook'
        WHEN ac.action_name LIKE '%linkedin%' THEN 'linkedin'
        WHEN ac.action_name LIKE '%google%' THEN 'google'
        ELSE 'email'
    END AS signup_type,  
    
    CASE 
        -- Direct success: successful signup on first attempt
        WHEN ac.action_name LIKE '%success%' 
             AND s.date_registered IS NOT NULL
             AND CAST(s.date_registered AS DATE) = CAST(ac.action_date AS DATE)
        THEN 'direct_success'
        
        -- Failed attempt: no registration after signup action
        WHEN ac.action_name LIKE '%fail%' 
             AND s.date_registered IS NULL
        THEN 'fail'
        
        -- Successful retry: failed initially but registered later
        WHEN ac.action_name LIKE '%fail%' 
             AND s.date_registered IS NOT NULL
             AND CAST(s.date_registered AS DATE) >= CAST(ac.action_date AS DATE)
        THEN 'successful_retry'
    END AS signup_attempt,  
    
    -- Capture error messages for failed attempts
    IFNULL(e.error_text, ' ') AS error_message, 
    
    -- Categorize visitors by operating system and device
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
LEFT JOIN visitors v ON v.visitor_id = ac.visitor_id     -- join the actions table with visitors table 
LEFT JOIN students s ON s.user_id = v.user_id           -- join students table with visitors table 
LEFT JOIN error_messages e ON e.error_id = ac.error_id  -- join error_messages table with actions table 
LEFT JOIN sessions se ON se.visitor_id = ac.visitor_id  -- join sessions table with actions table 

WHERE 
    ac.action_name LIKE '%sign%'         -- action is a sign-up attempt
    AND (ac.action_name LIKE '%success%' OR ac.action_name LIKE '%fail%') -- success or fail
    AND v.first_visit_date >= '2022-07-01' 
    AND ac.action_date BETWEEN '2022-01-01' AND '2023-02-01' -- date range

GROUP BY visitor_id 
HAVING signup_attempt IS NOT NULL  -- only consider valid signup attempts
ORDER BY signup_date;

