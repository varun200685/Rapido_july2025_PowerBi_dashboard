Rapido Bookings — SQL Analysis

SQL practice project analyzing a Rapido ride-booking dataset (583 rows) in MySQL, covering aggregation, filtering, conditional logic, and window functions — skills directly applicable to BFSI/analytics reporting (transaction summaries, cancellation/default rates, customer segmentation).

Dataset

Ride-booking data with the following fields:

Date, Booking_ID, Booking_Status, Customer_ID, Vehicle_Type, Pickup_Location, Drop_Location, V_TAT, C_TAT, Canceled_Rides_by_Customer, Canceled_Rides_by_Driver, Incomplete_Rides, Incomplete_Rides_Reason, Booking_Value, Payment_Method, Ride_Distance, Driver_Ratings, Customer_Rating

Tools


MySQL Workbench
Table: rapido1.bookings


What's covered

#QuestionConcepts1Booking count by statusGROUP BY2Total revenue from successful ridesWHERE, SUM3Top 5 pickup locations by bookingsGROUP BY, ORDER BY, LIMIT4Avg distance & value by vehicle type (successful only)WHERE vs HAVING5Cancellations by customer vs driverCOUNT(column) on nullable fields6Highest-revenue payment methodWHERE, ORDER BY, LIMIT7Customer with 2nd-highest total spendSubquery + DENSE_RANK() OVER (ORDER BY ...)8Daily cancellation rateConditional aggregation, operator precedence9Rank pickup locations by revenue within each vehicle typeDENSE_RANK() OVER (PARTITION BY ...)10Frequent pickup → drop location pairsGROUP BY (multi-column), HAVING COUNT(*) > n

Data limitations

This practice used a 583-row subset of the full Rapido dataset (the original source data has 20,000+ records, used separately for the Power BI dashboard project). Two consequences worth noting:


Q8 (daily cancellation rate) returns only a single row, since every booking in this subset falls on the same date (01-07-2024). The query logic is correct and would return one row per day on a multi-date dataset — there's just no date variation here to demonstrate it.
Q10 (frequent location pairs) uses a lowered threshold (HAVING COUNT(*) > 2 instead of > 20) to account for the smaller sample size, since no pair realistically repeats 20+ times in 583 rows.


Both queries are written to scale correctly to the full dataset without modification — only the threshold in Q10 would need adjusting back up.

Key takeaways


WHERE vs HAVING: WHERE filters individual rows before aggregation; HAVING filters aggregated groups after GROUP BY. A common mistake is using HAVING for row-level filters — it can work but is inefficient and non-standard.
PARTITION BY vs no partition: window functions rank across the whole result set by default; PARTITION BY resets the ranking within each group (e.g., ranking locations within each vehicle type).
Subquery pattern for "Nth highest": window functions can't be filtered in the same query they're calculated in — wrap the ranked query in a subquery and filter in the outer WHERE.
COUNT(column) vs COUNT(*): COUNT(column) ignores NULLs, which is a quick way to count non-null occurrences (e.g., cancellation reasons) without a CASE WHEN.


Files


rapido_sql_practice.sql — all 10 queries, commented and ready to run against the bookings table.


Related project

Paired with my Rapido Operations Dashboard built in Power BI, using the same dataset.
