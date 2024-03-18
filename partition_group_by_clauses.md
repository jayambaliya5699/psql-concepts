### Table Schema:

| name    | salary | department |
|---------|--------|------------|
| Alice   | 2000   | Sales      |
| Kevin   | 3500   | Technical  |
| Bob     | 3000   | Sales      |
| Glenn   | 1550   | HR         |
| Jack    | 2500   | Sales      |
| Sachin  | 3000   | Technical  |
| Morman  | 1250   | HR         |
| Charlie | 4000   | Technical  |


From the above table, consider that we want to fetch the average salary as per the department.
We can achieve this either using **GROUP BY** or using **PARTITION BY** clause.

### 1.GROUP BY:

**SELECT department, avg(salary) FROM employee GROUP BY department;**

Result:

| department | avg    |
|------------|--------|
| Sales      | 2500.00|
| HR         | 1400.00|
| Technical  | 3500.00|

Let's say we want to fetch the average salary by department, but we don't want our table information gets affected.
i.e. we want to preserve the table information as it is while aggregating the data. We can do that with the help of 
**PARTITION BY** clause.

### 2.PARTITION BY:

**SELECT \*, avg(salary) OVER (PARTITION BY department) from employee;**

Result:

|   name   | salary | department |      avg      |
|----------|--------|------------|---------------|
| Morman   |  1250  | HR         | 1400.00       |
| Glenn    |  1550  | HR         | 1400.00       |
| Jack     |  2500  | Sales      | 2500.00       |
| Alice    |  2000  | Sales      | 2500.00       |
| Bob      |  3000  | Sales      | 2500.00       |
| Sachin   |  3000  | Technical  | 3500.00       |
| Kevin    |  3500  | Technical  | 3500.00       |
| Charlie  |  4000  | Technical  | 3500.00       |


Same as **GROUP BY** clause we can use multiple aggregator functions to receive the information using **PARTITION BY** clause:

Refer this:

**SELECT \*, avg(salary) OVER (PARTITION BY department),max(salary) OVER (PARTITION BY department) from employee;**

Result:

|   name   | salary | department |      avg      |  max  |
|----------|--------|------------|---------------|-------|
| Morman   |  1250  | HR         | 1400.00       | 1550.00  |
| Glenn    |  1550  | HR         | 1400.00       | 1550.00  |
| Jack     |  2500  | Sales      | 2500.00       | 3000.00  |
| Alice    |  2000  | Sales      | 2500.00       | 3000.00  |
| Bob      |  3000  | Sales      | 2500.00       | 3000.00  |
| Sachin   |  3000  | Technical  | 3500.00       | 4000.00  |
| Kevin    |  3500  | Technical  | 3500.00       | 4000.00  |
| Charlie  |  4000  | Technical  | 3500.00       | 4000.00  |

