# HDP_Test_Q4
# Question 4 output data

* Q4.
Find out CTC of each department grouped by each dept_manager's name and number of employees under the manager. The output should be in the following format: Dept_no, Department, Manager, CTC, total_emps. Please upload the final CSV along with the code and jar files(if any) to your github and provide the link below. 

# Step by Step details of Q4 execuation :

 To Solve this question I have used  first Hive and then  Pig.

Here In this question need below things:

Department number, department name, Manager for the department, CTC( sum of given salary of total number of employees under each department) and lastly need to find Total number of employees under each department and manager.

The Hive qury used here as :
"

INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/Documents/d001' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd001' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';

"

In above Query, I have  used Department , employee, salary, department manager tables and made left join on using emp_no column.
Here I am getting the Department number, department name, manager name, slary of employees department wise, and count of total employees under each department.
and I have exported this required columns in CSV file in mentioned path using the "INSERT OVERWRITE LOCAL DIRECTORY "/home/vishnu/Documents/d005"

Now I have used Pig, here the output  CSV file of Hive "d005" used as input to Pig and loaded in one bag to process it.
The Pig Script I have used as:

1. In Pif First am ladong the Hive generated CSV file as input for each department wise in a bag along with the column name and its data type details and also the "PigStorage(',') to define the delemiter in input file.
"
employee_dept_details = LOAD '/home/vishnu/Documents/d005/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
"


2.Here Cretaing new bag and grouping the data which available in  above created bag of input file:
"
employee_group = Group employee_dept_details all;
"


3.Here In new bag doing foreach on above bag and generating the out put file using "flatten" on first bag and doing SUM operation and the Count of the total employee number department wise.
"
student_workpages_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
   "
   
4. Here Storing the generated output file given path in CSV format.

STORE student_workpages_sum INTO 'file:///home/vishnu/Desktop/d004';


# Summery:

* In Hive I have merged the multiple table data in one CSV file using join operation and exported that CSV file.

* In above Pig script, First I have loaded the CSV file generated using Hive in Bag "employee_dept_details" by defining the delemeter as "," and then the column name with datatype.
* The In "employee_group" bag, I have grouped the "employee_dept_details" bag.
* Then used foreach on "employee_group" bag and generated output file along with SUM and COUNT operation in required columns data.
* Lastly I am storing the generated output file in path as CSV

Using above Hive and Pig commands, I have generated the other remaning 8 department data.

# Uploaded Directory , File details as:


I have uploaded Main one zip "Q4_OutputData.zip" which contains below two main parts:

1."Question_four_hive_table_export_files" This Directory contains the All Departments Hive exported table CSV files.

2."Question_Four_output_csv_department_wise" This is final output directory which is generated using PIG, and it contains all output columns in CSV file of all 9 departments .

# Final Output format:
employee no,department_no,department name,manager name,CTC,Total number of employees

# Please find the all department wise Hive and pig commnds below for each department.

Please Find Details below:

* Following HIVE query used to get employee details filter by department id i.e d001 from multiple tables:
INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/Documents/d001' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd001' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';

* Following PIG script is used to genrate desired output.it takes csv file as input which is genrated by hive for processing.
employee_dept_details = LOAD '/home/vishnu/Documents/d001/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
employee_group = Group employee_dept_details all;
employee_salary_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
STORE employee_salary_sum INTO 'file:///home/vishnu/Desktop/d001';

* Following HIVE query used to get employee details filter by department id i.e d002 from multiple tables:
INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/Documents/d002' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd002' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';

* Following PIG script is used to genrate desired output.it takes csv file as input which is genrated by hive for processing.
employee_dept_details = LOAD '/home/vishnu/Documents/d002/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
employee_group = Group employee_dept_details all;
employee_salary_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
STORE employee_salary_sum INTO 'file:///home/vishnu/Desktop/d002';


* Following HIVE query used to get employee details filter by department id i.e d003 from multiple tables:
INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/Documents/d003' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd003' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';


* Following PIG script is used to genrate desired output.it takes csv file as input which is genrated by hive for processing.
employee_dept_details = LOAD '/home/vishnu/Documents/d003/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
employee_group = Group employee_dept_details all;
employee_salary_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
STORE employee_salary_sum INTO 'file:///home/vishnu/Documents/d003';


* Following HIVE query used to get employee details filter by department id i.e d004 from multiple tables:
INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/Documents/d004' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd004' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';

* Following PIG script is used to genrate desired output it takes csv file as input which is genrated by hive.
employee_dept_details = LOAD '/home/vishnu/Documents/d004/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
employee_group = Group employee_dept_details all;
student_workpages_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
STORE student_workpages_sum INTO 'file:///home/vishnu/Desktop/d004';

* Following HIVE query used to get employee details filter by department id i.e d005 from multiple tables:
INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/documents/d005' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd005' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';

* Following PIG script is used to genrate desired output it takes csv file as input which is genrated by hive.
employee_dept_details = LOAD '/home/vishnu/Documents/d005/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
employee_group = Group employee_dept_details all;
employee_salary_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
STORE employee_salary_sum INTO 'file:///home/vishnu/Desktop/d005';

* Following HIVE query used to get employee details filter by department id i.e d006 from multiple tables:
INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/Documents/d006' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd006' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';

* Following PIG script is used to genrate desired output it takes csv file as input which is genrated by hive.
employee_dept_details = LOAD '/home/vishnu/Documents/d006/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
employee_group = Group employee_dept_details all;
employee_salary_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
STORE employee_salary_sum INTO 'file:///home/vishnu/Desktop/d006';

* Following HIVE query used to get employee details filter by department id i.e d007 from multiple tables:
INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/Documents/d007' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd007' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';

* Following PIG script is used to genrate desired output it takes csv file as input which is genrated by hive.
employee_dept_details = LOAD '/home/vishnu/documents/d007/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
employee_group = Group employee_dept_details all;
employee_salary_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
STORE employee_salary_sum INTO 'file:///home/vishnu/Desktop/d007';

* Following HIVE query used to get employee details filter by department id i.e d008 from multiple tables:
INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/Documents/d008' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd008' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';

* Following PIG script is used to genrate desired output it takes csv file as input which is genrated by hive.
employee_dept_details = LOAD '/home/vishnu/Documents/d008/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
employee_group = Group employee_dept_details all;
employee_salary_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
STORE employee_salary_sum INTO 'file:///home/vishnu/Desktop/d008';

* Following HIVE query used to get employee details filter by department id i.e d009 from multiple tables:
INSERT OVERWRITE LOCAL DIRECTORY '/home/vishnu/Documents/d009' row format delimited fields terminated by ',' select distinct(de.emp_no),de.dept_no,d.dept_name,e.first_name,dm.emp_no,s.salary from dept_manager as dm left join departments as d on (d.dept_no = dm.dept_no) LEFT JOIN dept_emp as de ON (de.dept_no = d.dept_no) left join salaries as s on (s.emp_no = de.emp_no) left join employees as e on (e.emp_no = dm.emp_no) where dm.dept_no = 'd009' AND dm.to_date = '9999-01-01' AND de.to_date = '9999-01-01'AND s.to_date = '9999-01-01';

* Following PIG script is used to genrate desired output it takes csv file as input which is genrated by hive.
employee_dept_details = LOAD '/home/vishnu/Documents/d009/000000_0' USING PigStorage(',') as (emp_no:int,dept_no:chararray,dept_name:chararray,first_name:chararray,dmemp_no:int,salary:int);
employee_group = Group employee_dept_details all;
employee_salary_sum = foreach employee_group Generate 
   flatten(employee_dept_details),SUM(employee_dept_details.salary),COUNT(employee_dept_details.emp_no);
STORE employee_salary_sum INTO 'file:///home/vishnu/Desktop/d009';








