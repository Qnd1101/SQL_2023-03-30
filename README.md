# SQL_2023-03-30

## Join 기법 : 
 * ### 한 개의 테이블에서 원하는 정보를 모두 조회하지 못한 경우,
 * ### 여러개의 테이블에서 정보를 조회하는 기법

### 내부조인(inner join) :
  * #### 테이블들의 '공통 컬럼'을 기준으로 join

### 외부조인(outer join) :
  * #### Null 값을 포함한 튜플을 검색
  * #### where절에서 조건문을 쓸 때 별칭.컬럼 = 별칭.컬럼 ex) a.컬럼 = b.컬럼
  * #### 왼쪽을 기준으로 오른쪽에 있는 모든 값(Null 포함)도 끌어온다 해서 left join
      > ex) where a.컬럼 = b.컬럼(+)
  * #### 오른쪽을 기준으로 왼쪽에 있는 모든 값(Null 포함)도 끌어온다 해서 Right join
      > ex) where a.컬럼(+) = b.컬럼
### 셀프조인(self join)
   * 1개의 테이블로 가상의 2개의 테이블을 만들어 조인
   
### 조인문 연습
J-1) 급여가 3000이상인 employee_id, last_name, salary, department_name, city 을 조회하시오
select E.employee_id, E.last_name, E.salary, D.department_name, L.city    
from employees E, departments D, locations L    
where E.department_id = D.department_id and D.location_id = L.location_id and 3000 <= E.salary; -- 내부조인(등가조인)

J-2) 급여가 2500 이하이고 사원번호가 200이하인 사원의 last_name, 부서명을 조회하시오.
select E.last_name, D.department_name
from employees E, departments D
where E.department_id = D.department_id and E.salary <= 2500 and E.employee_id <= 200; --내부조인(등가조인)

J-3) 급여범위가 jobs 테이블의 최소급여(min_salary)와 최대급여(max_salary) 사이에 있는 사원의 last_name, job_id를 조회하시오.
-- 비등가조인(NON-EQUI JOIN)
select E.last_name, J.job_id                  
from employees E, jobs J   
where E.salary between J.min_salary and J.max_salary; --내부조인(비등가조인)

J-4) 자신의 매니저보다 먼저 고용된 사원들의 last_name 및 고용일을 조회한다.(셀프조인)
select EMP.last_name, EMP.hire_date   
from employees EMP, employees MGR    
where EMP.hire_date < MGR.hire_date and EMP.manager_id = MGR.employee_id; -- 셀프조인

J-5) 사원정보(employee_id, last_name, manager_id)와 사원의 직속 상관정보(employee_id, last_name) 조회하시오
-- 외부조인을 이용하여 Null 값도 조회함
select EMP.employee_id as 사원번호, 
	EMP.last_name as 사원이름, 
	EMP.manager_id as 사원의매니저번호, 
	MGR.employee_id AS 매니저의사원번호, 
	MGR.last_name as 매니저이름
from employees EMP, employees MGR
where EMP.manager_id = MGR.employee_id(+)
order by EMP.employee_id;
