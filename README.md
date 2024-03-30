Advanced SQL Queries for Data Analysis: 

1. Calculate the average salary within each department without grouping the entire table by department, allowing each employee’s salary to be directly compared to their department average:

  -> SELECT 
      employee_id,
      department,
      salary,
      AVG(salary) OVER(PARTITION BY department) as avg_department_salary
      FROM employees;

2. Calculate total sales by region and then selects only those regions where sales exceed the average regional sales:

   -> WITH RegionalSales AS (
        SELECT region, SUM(amount) AS total_sales
        FROM sales
        GROUP BY region
        )
      SELECT region, total_sales
        FROM RegionalSales
        WHERE total_sales > (SELECT AVG(total_sales) FROM RegionalSales);
3. Represent an employee and each column represents a year’s total sales:
   -> SELECT *
        FROM 
          (SELECT EmployeeName, Year, Sales FROM EmployeeSales) AS SourceTable
        PIVOT
          (
            SUM(Sales)
            FOR Year IN ([2020], [2021], [2022])
          ) AS PivotTable;
4. Merging tables
    -> MERGE INTO TargetTable AS target
          USING SourceTable AS source
          ON target.ID = source.ID
          WHEN MATCHED THEN
              UPDATE SET target.Name = source.Name
          WHEN NOT MATCHED BY TARGET THEN
              INSERT (ID, Name) VALUES (source.ID, source.Name)
          WHEN NOT MATCHED BY SOURCE THEN
              DELETE;
5. 
