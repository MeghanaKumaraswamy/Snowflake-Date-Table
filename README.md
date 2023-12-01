# Snowflake-Date-Table

CREATE OR REPLACE TABLE DEVELOPMENT.DL3.date_table AS
WITH date_range AS (
  SELECT DATEADD(DAY, seq4(), '2010-01-01') AS date_value
  FROM TABLE(GENERATOR(ROWCOUNT => 11323))
)
SELECT
  TO_DATE(TO_VARCHAR(date_value, 'YYYY-MM-DD'), 'YYYY-MM-DD') AS mdy,
  DATE_FROM_PARTS(
    YEAR(date_value), 1, 1
  ) AS year_mdy,
  DATE_FROM_PARTS(
    YEAR(date_value), MONTH(date_value), 1
  ) AS month_mdy,
  DATEADD(DAY, 5 - DAYOFWEEK(mdy), mdy) AS fun_friday_mdy,
  CONCAT(
    RIGHT(YEAR(date_value), 2),
    'Q',
    DATE_PART(QUARTER, date_value)
  ) AS yyqq
FROM date_range
