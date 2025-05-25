[SQL GFG Practice][https://youtube.com/playlist?list=PLqM7alHXFySGweLxxAdBDK1CcDEgF-Kwx&si=7YMRUJ4ya41mbcR4] && [SQL Practice YouTube][https://www.youtube.com/watch?v=-6v7ctxC7yk&list=PLqM7alHXFySGweLxxAdBDK1CcDEgF-Kw]

##### 1. Problem find the second highest salary: [https://www.geeksforgeeks.org/sql-query-to-find-second-largest-salary/]

``` postgressql
	limit query ->
	SELECT _`select_list`_
    FROM _`table_expression`_
    [ ORDER BY ... ]
    [ LIMIT { _`count`_ | ALL } ]
    [ OFFSET _`start`_ ]

	dense rank query ->
	DENSE_RANK() OVER (
	    [PARTITION BY partition_expression, ...]  ORDER BY sort_expression, ...
	)
```

``` mysql
	solution:
	select max(sal) from emp where sal not in(select max(sal) from emp);
	- sub query is executed first

	my way:
	SELECT salary FROM your_table_name ORDER BY salary ASC LIMIT 2;

	dads way:
	SELECT * FROM Employees WHERE Salary = (
    SELECT MAX(Salary)
    FROM Employees
    WHERE Salary < (SELECT MAX(Salary) FROM Employees));
	
```

##### different ways of solution

- ****Ranking Employees:**** Determine employee rankings based on salary.
- ****Data Analysis:**** Analyze salary distribution trends.
- ****Reward Calculations:**** Assign percentile-based rewards.

1. Using subqueries : A ****subquery**** can be used to exclude the ****maximum salary**** and find the ****second-highest salary****. Below is a simple query to find the ****employee**** whose salary is the ****highest****.

```MySql
SELECT name, salary    
FROM employee    
WHERE salary = (    
    SELECT MAX(salary)    
    FROM employee    
    WHERE salary < (SELECT MAX(salary) FROM employee)    
);
```

2. Using limit and offset clause: 

```MySql
select * from employee   
group by salary   
order by  salary desc limit 1,1;
```

3. Using Common Table Expressions: Using built in expressions like DENSE RANK, can simplify this process, we can find the second highest salary:

``` mysql
WITH RankedSalaries AS (    
    SELECT name, salary,    
           DENSE_RANK() OVER (ORDER BY salary DESC) AS Rank    
    FROM employee    
)    
SELECT name, salary    
FROM RankedSalaries    
WHERE Rank = 2;
```

