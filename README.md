# Pewlett-Hackard Analysis
#
## Overview:
This project's goal is to determine the number of retiring employees per title, identify eligible employees to participate in a mentorship program, and write a report that summarizes the analysis to help the company prepare for an uptick in retirements.

The first step was to create an Entity Relatioship Diagram (ERD) to map out the key aspects of the data and their relationship to one another:

![Picture](/EmployeeDB.png)

#
## List of requested deliverables:
- Deliverable 1: The Number of Retiring Employees by Title
- Deliverable 2: The Employees Eligible for the Mentorship Program
- Deliverable 3: A written report on the employee database analysis

#
## Tools:
- GitBash
- Quick DBD
- pgAdmin 4
- PostgreSQL
- Virtual Studio Code

#
## Results:

- Based on the criteria provided, 72,458 employees are likely to retire soon.
    - Senior Engineers and Senior Staff make up approximately 70% of the total number of soon-to-retire employees.
- 1,549 employees meet the requirements to participate in the mentorship program.
    - That is a ratio of 46.7 new employees to 1 mentor.

#
## Summary: 

According to the Social Security Administration (SSA), the full retirement age is around 66 years old, with early retirement starting at age 62 and late retirement capping at the age of 70. Employees born between January 1, 1952, and December 31, 1955, are within the full retirement age and likely to retire in the near future. When querying employees born within those dates and sorting by job title we can see the number of employees potentially to retire soon. To help with vizualisation, numbers were sorted by job title.

![Picture](/Images/retiring_titles_results.png)


In order to better prepare the future generations of employees who will be moving on to these positions, one of the management suggestions was to create a mentorship program. The criteria for the query was set for employees born between January 1, 1965 and December 31, 1965. The table below shows the numbers also grouped by job title.

![Picture](/Images/mentorship_eligibility_by_title.png)

## Final Questions:
#
### 1) How many roles will need to be filled as the "silver tsunami" begins to make an impact?

Based on an initial analysis, 72,458 roles will likely need to be filled in the next 0-3 years. In order to calculate how many employees are reaching retirement age, the following function was used:

```python
SELECT SUM(count) FROM retiring_titles;
```

### 2) Are there enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees?

In order to determine how many retirement-ready employees would meet the mentorship criteria based on departments, a new join was created to correlate the emp_no to dept_no and dept_name. Following the COUNT function was applied to get the numbers by department. 

```
SELECT * FROM mentorship_eligibility;

SELECT DISTINCT ON (emp_no) me.emp_no, 
	me.birth_date,
	me.from_date, 
	me.to_date, 
	de.dept_no,
	d.dept_name
INTO mentorship_elig_by_dept
FROM mentorship_eligibility as me
JOIN dept_emp as de ON (me.emp_no = de.emp_no)
JOIN departments as d ON (de.dept_no = d.dept_no)
WHERE (de.to_date = ('9999-01-01')) AND (me.birth_date BETWEEN '1965-01-01' AND '1965-12-31');

SELECT COUNT (med.dept_name), med.dept_name
FROM mentorship_elig_by_dept as med
GROUP BY dept_name
ORDER BY COUNT DESC;
```

This query gave the following data output: 

![Picture](/Images/mentorship_elig_by_dept.png)

To calculate how many employees meet the mentorship criteria, the following code was used returning a value of 1,549:

```
SELECT COUNT (med.dept_name)
FROM mentorship_elig_by_dept as med
--GROUP BY dept_name
ORDER BY COUNT DESC;
```

#
## Conclusion and Recommendations: 

Based on the overall number of soon-to-be retirees and the low number of mentors identified, Pewlett-Hackard should consider increasing the eligibility criteria for the mentorship program so they have more mentors available to support the large numbers of new hires. Furthermore, I would suggest they start considering the employees between the ages of 62 and 67 as they are also likely to retire within the next 1-5 years.