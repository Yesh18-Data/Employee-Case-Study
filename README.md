# Employee-Case-Study


### Introduction 

*__This case study focuses on HR management at EDGE IT SOLUTIONS, a hypothetical tech company. By analyzing employee data, it aims to uncover insights into recruitment, retention, performance evaluation, and satisfaction. The study seeks to enhance workforce strategies for organizational success. This analsys consists of 4 Tables they are__*


* *__Location__*
* *__Job__*
* *__Department__*
* *__Employee__*

#### Employee Case Study Questions

* *__1. List out the department-wise maximum salary, minimum salary, average salary of the employees.__*<br>

* *__2.List out job-wise maximum salary, minimum salary, average salaries of the employees.__*<br>

* *__3. List out the number of employees joined in every month in ascending order.__*<br>

* *__4. List out the number of employees for each month and year, in the ascending order based on the year, month.__*<br>

* *__5. How many employees joined in January or September month.__*<br>

* *__6. Which is the department ID, having greater than or equal to 3 employees joined in April 1985.__*<br>

* *__7. How many employees who are working in the sales department.__*<br>

* *__8. Which is the department having greater than or equal to 5 employees and display the department names in ascending order.__*<br>

* *__9.How many employees are working in "New York".__*<br>

* *__10. Display the employee details with salary grades.__*<br>

* *__11. List out the no. of employees on grade-wise.__*<br>

* *__12. Display the employee salary grades and the number of employees between 2000 to 5000 ranges of salary.__*<br>

* *__13. Display all employees in sales or operation departments.__*<br>

* *__14. List out the distinct jobs in sales and accounting departments.__*<br>

* *__15. List out the common jobs in research and accounting departments in ascending order.__*<br>

* *__16. Display the employees who are working in the sales department.__*<br>

* *__17. Display the employees who are working as 'Clerk'.__*<br>

* *__18. Display the list of employees who are living in "New York".__*<br>

* *__19. Display the Nth highest salary drawing employee details.__*<br>








"SQL Queries for HR Data Analysis: Employee Information and Insights"


*__1. LIST OUT THE DEPARTMENT WISE MAXIMUM SALARY, MINIMUM SALARY, AVERAGE SALARY OF
THE EMPLOYEES.__*

```
           SELECT department_id,
		     max (salary) as Max_salary,
			 min(salary) as Min_salary,
			 avg(salary) as Avg_salary
			 from employee

			 group by department_id
			 having count(*) > 0			
```
 2. LIST OUT JOB WISE MAXIMUM SALARY, MINIMUM SALARY, AVERAGE SALARIES OF THE
EMPLOYEES.
```
                 select job_id,
				 max(salary) as Max_salary,
				 min(salary) as Min_salary,
				 avg(salary) as Avg_salary

				 from employee
				 
				 group by job_id
				 having count(*) >0
```
				

3. LIST OUT THE NUMBER OF EMPLOYEES JOINED IN EVERY MONTH IN ASCENDING ORDER.
```
                SELECT EMPLOYEE_ID, COUNT (*) AS NUM_Employee
				from employee
			   group by employee_id
				having count(*) >0
```
4. LIST OUT THE NUMBER OF EMPLOYEES FOR EACH MONTH AND YEAR, IN THE ASCENDING ORDER BASED ON THE YEAR, MONTH. 
```
   SELECT LEFT(DATENAME(MONTH, hire_date), 3)+ '-'+right(datename(year, hire_date),  4)
   AS join_month_Year,
       COUNT(*) AS num_employees
FROM employee
GROUP BY  LEFT(DATENAME(MONTH, hire_date), 3)+ '-'+right(datename(year, hire_date),  4) 

HAVING COUNT(*) > 0
ORDER BY join_month_Year asc
```


5. HOW MANY EMPLOYEES JOINED IN JANUARY OR SEPTEMBER MONTH.

```
              SELECT COUNT(EMPLOYEE_ID) AS NP_of_Employee_JOINED_IN_01_09
              from employee AS EMP
              where (DATEpart(month,hire_date)) = 1 or (DATEpart(month,hire_date))  = 9					  
```
		 
  6. WHICH IS THE DEPARTMENT ID, HAVING GREATER THAN OR EQUAL TO 3 EMPLOYEES JOINED IN
APRIL 1985. 
```
                    SELECT  COUNT(EMP.EMPLOYEE_ID) AS NUMBER_OF_EMPLOYEES ,Department_ID
					FROM Employee AS EMP
					WHERE DATENAME(MONTH,HIRE_DATE) = 'APRIL' AND DATEPART(YEAR,HIRE_DATE) = 1985
					GROUP BY Department_Id
					HAVING COUNT(EMP.EMPLOYEE_ID) >= 3
```					 

7. HOW MANY EMPLOYEES WHO ARE WORKING IN SALES DEPARTMENT. 
```
             Select name as DEPARTMENT_NAME , COUNT(EMP.EMPLOYEE_ID) AS NUM_OF_EMPLOYEE
			 FROM Employee AS EMP
			 INNER join DEPARTMENT AS DEPT ON DEPT.DEPARTMENT_id = emp.Department_id
             where (dept.NAME) = 'sales'
			 group by (dept.Name)

```

 8. WHICH IS THE DEPARTMENT HAVING GREATER THAN OR EQUAL TO 5 EMPLOYEES AND DISPLAY
THE DEPARTMENT NAMES IN ASCENDING ORDER. 
```
           select name as department_name , COUNT(emp.employee_id) as num_of_employees
		   from Employee as emp
		   inner join Department as dept on dept.Department_ID = emp.Department_Id
		   group by dept.Name
		   having count(dept.Name)>=5
		   order by dept.Name asc
	```	   

  9. HOW MANY EMPLOYEES ARE WORKING IN "NEW YORK".
  
	```	  select COUNT(emp.employee_id)as num_of_employees,loc.city
		  from Employee as emp
		  inner join  department as dept on  dept.Department_ID = emp.Department_Id
		  inner join location as loc on loc.location_Id = dept.location_id
		  where CITY = 'New york'
		  group by loc.city
```
                

  10. DISPLAY THE EMPLOYEE DETAILS WITH SALARY GRADES. 

```
           SELECT EMP.Employee_ID,CONCAT(first_name,middle_name,last_name) AS Employee_name,
		   EMP.SALARY,   try_cast(JB.Designation as int)
		   FROM Employee AS EMP
		   INNER JOIN Department AS DEPT ON DEPT.Department_ID = Emp.DEPARTMENT_ID
		   INNER JOIN  JOB AS JB ON  JB.JOB_ID = EMP.JOB_ID
		   ORDER BY Salary DESC
```

11.LIST OUT THE NO. OF EMPLOYEES ON GRADE WISE.
```   
    ;with SalaryGradetemp                 

          as (select EMP.EMPLOYEE_ID,salary,
			 
    (Case 
         When salary <=1000 Then '0 -1000'
         When salary >= 1001 and salary < 2000 Then '1001 -2000'
		 when Salary  >= 2001 and Salary < 3000 then  '2001 -3000'
         else
             '4000'
     end) as salary_Grade                                           
      from Employee as emp
	 
        )         
	select salary_grade,COUNT(EMPLOYEE_ID) AS NUM_OF_EMPLOYEE
	 from SalaryGradetemp
	   GROUP BY salary_Grade
           ORDER BY  salary_Grade


	  --ALTERNATIVE

	 ;with salary_gradetemp1
	  as (


	  select EMP.Employee_Id, salary,
	                 (case
					          when salary <=1000 then ' Grade D'
							  when salary >1000 AND SALARY <=2000  THEN 'Grade  C'
			                  WHEN SALARY >2000  AND SALARY <=3000 THEN 'Grade B'
							  ELSE  'Grade A'

			         END) AS SALARY_GRADE
					 FROM Employee AS EMP
					 
					)		  

					 
					 select salary_grade,COUNT(Employee_Id) AS NUMBER_OF_EMPLOYEE
					 from   salary_gradetemp1
					 group by SALARY_GRADE
					 order by SALARY_GRADE  desc

	```	  
			 
 12.DISPLAY THE EMPLOYEE SALARY GRADES AND NO. OF EMPLOYEES BETWEEN 2000 TO 5000
    RANGE OF SALARY.
```						     
    ;with SalaryGradetemp2                 

          as (
			  select EMP.EMPLOYEE_ID,salary,
			 
    (Case 
         When salary <=1000 Then '       0 -1000'
         When salary >= 1001 and salary < 2000 Then '1001 -2000'
		 when Salary  >= 2001 and Salary < 3000 then  '2001 -3000'
         else
             '4000'
     end) as salary_Grade                                           
      from Employee as emp
	 
        )         
		      select salary_grade,COUNT(EMPLOYEE_ID) AS NUM_OF_EMPLOYEE
			  from SalaryGradetemp2
		      WHERE  salary BETWEEN 2000 AND 5000
			  GROUP BY salary_Grade
			  ORDER BY  salary_Grade
```

13.DISPLAY ALL EMPLOYEES IN SALES OR OPERATION DEPARTMENTS.
```
                     select CONCAT_WS(' ',first_name,middle_name,last_name) as Employee_fullName
					  from Employee as emp
					  inner join Department as dept on dept.Department_ID = emp.Department_Id
					  where dept.Name = 'OPERATIONS' OR  dept.Name = 'SALES'
```

14. LIST OUT THE DISTINCT JOBS IN SALES AND ACCOUNTING DEPARTMENTS.
      ```    
                  SELECT  DISTINCT DESIGNATION AS JOB
                  FROM Employee AS EMP
                  INNER JOIN JOB AS JB ON JB.Job_ID = EMP.Job_Id
                  INNER JOIN Department AS DEPT ON DEPT.Department_ID =EMP.Department_Id
                   --WHERE DEPT.NAME  = 'SALES' AND DEPT.Name = 'ACCOUNTING'
                  WHERE DEPT.Name IN ('SALES','ACCOUNTING','OPERATIONS','RESEARCH')
       ```

 15. LIST OUT THE COMMON JOBS IN RESEARCH AND ACCOUNTING DEPARTMENTS IN ASCENDING ORDER.
  ```
                          SELECT DESIGNATION AS JOB
                          FROM Employee AS EMP
			              INNER JOIN JOB AS JB ON JB.Job_ID = EMP.Job_Id
			              INNER JOIN Department AS DEPT ON DEPT.Department_ID = EMP.Department_Id
			              WHERE Name IN ('RESEARCH','ACCOUNTING')
			          	  ORDER BY Designation			          
	```		        

16. DISPLAY THE EMPLOYEES WHO ARE WORKING IN SALES DEPARTMENT.
```
        SELECT Employee_ID,CONCAT(' ',First_Name,Middle_Name,Last_Name) AS EMPLOYEE_NAME
		FROM Employee AS EMP
		WHERE EMP.Department_Id = ( SELECT  DEPT.Department_ID
		FROM Department AS DEPT
		WHERE Dept.name = 'SALES' )

		-- BY USING JOINS

        SELECT Employee_ID,CONCAT(' ',First_Name,Middle_Name,Last_Name) AS EMPLOYEE_NAME
		FROM Employee AS EMP
		INNER JOIN Department AS DEPT ON DEPT.Department_ID = EMP.Department_Id
		WHERE DEPT.Name = 'SALES'
```
17. DISPLAY THE EMPLOYEES WHO ARE WORKING AS 'CLERK'.
```
    SELECT Employee_ID,CONCAT_WS(' ',First_Name,Middle_Name,Last_Name) AS EMPLOYEE_NAME
		FROM Employee AS EMP
		WHERE EMP.Job_Id = ( SELECT JB.Job_Id FROM JOB AS JB
		WHERE JB.Designation =  'CLERK' )

		-- BY USING JOINS
		
        SELECT Employee_ID,CONCAT_WS(' ',First_Name,Middle_Name,Last_Name) AS EMPLOYEE_NAME
		FROM Employee AS EMP
		INNER JOIN JOB AS JB ON JB.Job_ID = EMP.Job_Id
		WHERE JB.Designation = 'CLERK'
```		 

18. DISPLAY THE LIST OF EMPLOYEES WHO ARE LIVING IN "NEW YORK".

```          
        SELECT Employee_ID,CONCAT_WS(' ',First_Name,Middle_Name,Last_Name) AS EMPLOYEE_NAME
		FROM Employee AS EMP
        WHERE Department_Id = (SELECT DEPT.DEPARTMENT_ID  FROM Department  AS DEPT
		                           WHERE DEPT.Location_ID = (SELECT LOC.Location_ID FROM location AS LOC
								                                WHERE CITY = 'NEW YORK') ```
							   
19. DISPLAY THE N'TH HIGHEST SALARY DRAWING EMPLOYEE DETAILS.
```
                   SELECT DISTINCT SALARY FROM Employee AS Emp1
				            WHERE (N-1) = ( SELECT COUNT(DISTINCT Salary)
							                    FROM Employee  as EMP2
												WHERE Emp1.Salary >= EMP2.Salary )

          -- IN N Place we have keep any number to get the Nth highest salary
```


							
