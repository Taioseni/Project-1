What issues will you address by cleaning the data?
1. Identifying and removing duplicate rows in the database is crucial
2. Handling null/missing values was a common challenge
3. Inconsistent data format from different sources
4. Normalisation issues which can lead to redundancy and inconsistency




Queries:
Below, provide the SQL queries you used to clean your data.

```
UPDATE all_sessions
SET city = null
WHERE city = '(not set)' OR city = 'not available in demo dataset'

UPDATE analytics
SET unit_price = unit_price/1000000

UPDATE analytics
SET unit_price = ROUND(unit_price, 2)
```