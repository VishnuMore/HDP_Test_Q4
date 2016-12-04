# HDP_Test_Q4
Question 4 output data

Q4.
Find out CTC of each department grouped by each dept_manager's name and number of employees under the manager. The output should be in the following format: Dept_no, Department, Manager, CTC, total_emps. Please upload the final CSV along with the code and jar files(if any) to your github and provide the link below. 

To Solve this question I have used  first Hive and then  Pig.

Here In this question need below things:

Department number, department name, Manager for the department, CTC( sum of given salary of total number of employees under each department) and lastly need to find Total number of employees under each department and manager.

The Hive qury used here as :
"

INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/Documents/New_dept_d005' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments1 as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd005' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';

"

In above Query, I have  used Department , employee, salary, department manager tables and made left join on using emp_no column.
Here I am getting the Department number, department name, manager name, slary of employees department wise, and count of total employees under each department.
and I have exported this required columns in CSV file in mentioned path using the "INSERT OVERWRITE LOCAL DIRECTORY "/home/vishnu/Documents/New_dept_d005"

Now I have used Pig, here the output  CSV file of Hive "New_dept_d005" used as input to Pig and loaded in one bag to process it.
The Pig Script I have used as:

employee_dept_details = LOAD '/home/vishnu/Documents/New_dept_d005/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
employee_group = Group employee_dept_details all;
student_workpages_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
STORE student_workpages_sum INTO 'file:///home/vishnu/Documents/PigQ4_deptNo_d004';

In above Pig script, First I have loaded the CSV file generated using Hive in Bag "employee_dept_details" by defining the delemeter as "," and then the column name with datatype.
The In "employee_group" bag, I have grouped the "employee_dept_details" bag.
Then used foreach on "employee_group" bag


