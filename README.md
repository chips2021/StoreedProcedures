# StoredProcedures
### Summarized Explanation of Optimizations for All Stored Procedures

**General Improvements Across Procedures**:

1. **Simplified Date Handling**:
   - Replaced `dateadd(DAY, 0, datediff(day, 0, pmai.invoice_date))` with `CAST(pmai.invoice_date AS DATE)`. 
   - **Benefit**: Simplifies the code, improves readability, and enhances performance by reducing function calls.

2. **Use of Common Table Expressions (CTEs)**:
   - Introduced CTEs to handle complex queries and filtering before the main `INSERT` operations.
   - **Benefit**: Improves code structure and readability, potentially leads to more efficient execution plans.

3. **Transactional Integrity**:
   - Ensured robust transaction handling with explicit `BEGIN TRANSACTION`, `COMMIT`, and `ROLLBACK` blocks.
   - **Benefit**: Maintains data integrity and provides clear, consistent error handling.

4. **Eliminated Redundant Operations**:
   - Removed unnecessary temporary tables where possible and used CTEs instead.
   - **Benefit**: Reduces overhead and simplifies the logic of the stored procedures.

5. **Efficient Filtering and Grouping**:
   - Filtered and grouped data in CTEs or optimized queries before the final `INSERT` or `MERGE` operations.
   - **Benefit**: Reduces the amount of data processed at each step, leading to faster execution times.

6. **Optimized `NOT EXISTS` Checks**:
   - Improved `NOT EXISTS` subqueries by ensuring they are written efficiently to avoid full table scans.
   - **Benefit**: Enhances performance by minimizing the number of records evaluated.

**Specific Improvements**:

1. **sp_Populate_FACTS_OilConsumption**:
   - Combined filtering and grouping in a single CTE.
   - Simplified `NOT EXISTS` check to improve performance.

2. **sp_Populate_FACTS_Car_Count**:
   - Introduced CTEs for initial filtering and aggregation.
   - Used `MERGE` statement efficiently to update or insert records.

3. **sp_Populate_FACTS_Repair_Car_Count**:
   - Used temporary tables only when necessary and replaced complex joins with CTEs for clarity.
   - Aggregated data in CTEs before final insertion.

4. **sp_Populate_FACTS_LOF_Sales**:
   - Filtered and aggregated data in a single CTE.
   - Simplified the `INSERT` process to directly utilize filtered data.

5. **sp_Populate_FACTS_Car_Serviced_Per_Category**:
   - Created CTEs to handle initial filtering and aggregation.
   - Simplified joins and inserts by pre-selecting relevant data in the CTEs.

6. **sp_Populate_FACTS_Services**:
   - Introduced a CTE for filtering relevant service records.
   - Simplified group by operations to improve readability and performance.

**Overall Benefits**:
- **Performance**: Reduced execution time and resource usage by simplifying operations and optimizing queries.
- **Readability**: Enhanced code readability and maintainability through structured use of CTEs and simplified logic.
- **Maintainability**: Made the stored procedures easier to update and debug by separating concerns and using clear, efficient SQL constructs.
- **Data Integrity**: Ensured robust transaction handling to maintain data consistency and reliability.

These optimizations collectively enhance the performance, readability, and maintainability of the stored procedures, leading to a more efficient and reliable database system.
