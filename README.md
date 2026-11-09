-- EdgeShield - MySQL Slow Query & Lock Mitigation Guard

-- 1. Enable Slow Query Log to track baseline latency
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 0.5; -- Log queries taking over 500ms

-- 2. Inspect active threads for lock bottlenecks
SELECT 
    ID, USER, HOST, DB, COMMAND, TIME, STATE, INFO
FROM 
    information_schema.PROCESSLIST
WHERE 
    COMMAND != 'Sleep' AND TIME > 5
ORDER BY 
    TIME DESC;

-- 3. Optimization Index Safeguard for heavy traffic tables
ALTER TABLE wp_posts ADD INDEX idx_post_status_type_date (post_status, post_type, post_date);
ALTER TABLE wp_postmeta ADD INDEX idx_meta_key_value (meta_key(50), meta_value(100));
