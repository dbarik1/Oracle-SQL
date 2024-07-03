# Oracle-SQL

select * from employees;
select employee_id, first_name, sysdate, hire_date , trunc((sysdate - hire_date)/365) years_of_exp
from employees
order by years_of_exp desc;

select employee_id, first_name, hire_date,
        trunc(hire_date, 'MONTH') as "trunc_roundoff", round(hire_date, 'MONTH') as "round_off"
        from employees;

**--It will exact the day/month/year for that sysdate **       
select extract(DAY from sysdate) from dual;       

 
select first_name, hire_Date,
    to_char(hire_date, 'MON-YYYY hh24:MI:SS') "formatted date"
from employees;


select first_name, hire_Date,
    to_char(SYSDATE, 'MM-YYYY hh24:MI:SS') "formatted date"
from employees;

select salary, salary * commission_pct* 100 "original_salary",
    to_char(salary * commission_pct* 100, 'L999,999.00') "converted_salary"
    from employees
    where commission_pct is not null;
    
select to_number('$33,990.99','$99,999.00') converted_number from dual;

--nvl function is used for alternatives, if the commission_ct is null, then it will add 0 in as alternatives
select employee_id, salary, salary + salary * nvl(commission_pct,0) new_Salary
from employees;


select employee_id, salary, commission_pct, nvl2(commission_pct,'has','has not') new_salary
from employees;


-- if the length of both fist and last name is same, then it will return null or else first column length.
select first_name, last_name, length(first_name), length(last_name),
nullif(length(first_name), length(last_name)) results
from employees
where nullif(length(first_name), length(last_name))  is not null;

-- coalesce function is used, if the first experssion is null it print 2nd if not it will print 1st exp.
select state_province, city, COALESCE(state_province, city) from locations;

select avg(salary), avg(all salary), avg(distinct salary) from employees;

select avg(commission_pct), avg(nvl(commission_pct,0)) from employees;

-- count function

select count(*),
    count(commission_pct),
    count(nvl(commission_pct,0)),
    count(distinct commission_pct),
    count(distinct nvl(commission_pct,0))
    from employees;
    

-- MAX function    
SELECT MAX(SALARY) FROM EMPLOYEES; 

select max(salary), max(commission_pct), max(nvl(commission_pct,0)), max(hire_Date)
from employees;

-- min function

select min(salary),
    min(commission_pct),
    min(nvl(commission_pct,0)),
    min(hire_date),
    min(first_name)
    from employees;
    
-- sum function

select sum(salary), sum(All salary), sum(distinct salary) from employees;

--listagg function (list the names in one single line with comma delimeter

select listagg(first_name,',') within group (order by first_name) "employees"
from employees;

select listagg(salary, ' - ') within group (order by salary) as salary_ingroup
from employees
where salary > 10000;

select listagg(city,' - ') within group(order by city) as city_new
from locations
where country_id ='US';

select * from locations;

SELECT j.job_title,
  LISTAGG (e.first_name,', ') WITHIN GROUP (ORDER BY e.first_name) AS employees_list
FROM employees e, jobs j
WHERE e.job_id = j.job_id
GROUP BY j.job_title;


select max(salary), avg(salary), count(*),
    listagg(distinct manager_id,',' ), min(salary)
    from employees;


-- group by clause

select job_id, avg(salary) from employees
group by job_id
order by avg(salary);

select job_id, department_id, avg(salary), count(*)
from employees
group by job_id, department_id
order by count(*) desc;

select department_id, count(distinct job_id) "num_of_job_id" from employees
group by department_id;


select department_id, avg(salary), min(salary), max(hire_date), count(*)
from employees
group by department_id
order by count(*) desc;

-- we can restrict the values in the order of execution in sql engine. Where comes first then group by, select and order by
select job_id, department_id, avg(salary), min(salary), count(*)
from employees
--where job_id in ('PU_CLERK','ST_CLERK','SH_CLERK')
group by job_id, department_id
order by count(*) desc;

-- having clause. Where clause only filter rows values but having caluse filter values from group.

select job_id, avg(salary)
from employees
group by job_id
having avg(salary) > 10000;

select job_id, avg(salary), count(*)
from employees
where salary > 10000
group by job_id
having avg(salary) > 5000;


select min(avg(salary)) from employees
group by department_id;



-- Join Examples

select * from employees;
select * from departments;

select * from employees natural join departments;
select employee_id, first_name, last_name, department_id, department_name
from employees natural join departments;

select * from employees join departments using(department_id, manager_id);


![image](https://github.com/dbarik1/Oracle-SQL/assets/166466302/719a65b3-7e2f-4876-95b5-62bf19630401)
