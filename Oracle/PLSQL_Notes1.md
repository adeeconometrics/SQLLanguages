# Interacting with Oracle DB Server and Control Structures

## SQL Statements in PL/SQL: SQL Cursor Attributes

PL/SQL accepts the following kinds of SQL statements:
- Select statements 
- DML statements
    - `insert`
    - `update`
    - `delete`
- TCL statement
    - `commit`
    - `rollback`
    - `savepoint`

DCL and DDL statements cannot be directly executed in PL/SQL since they are executed at 
run time. The recommended way of working with DDL and DCL statements within PL/SQL is to 
use Dynamic SQL with the `execute immediate` statement.

> Dynamic SQL is a programming technique that enables you to build SQL statements 
dynamically at runtime. You can create more general-purpose, flexible applications by 
using dynamic SQL because the full text of a SQL statement may be unknown at compilation.

---
## `SELECT` statement in PL/SQL
- Retrieves data from a database into a PL/SQL variable with a `select` so it can work in 
PL/SQL.

```SQL
select [select_list]
into [id_list]
from [id::table]
-- [cl:where] --

select_list ::= [id::select_item] --, [select_list] --
id_list ::= [id_list:variable] | [id::record]
    variable ::= [id::variable] --, [variable] --
```
More expressive syntactical construction is defined [here](https://bityl.co/8tVS)

### Using the `INTO` clause
- The `into` clause is mandatory between `select` and `from` clauses.
- Used to specify the names of PL/SQL variables that hold values that SQL returns from
the `select` clause.

Example:

```SQL
declare 
    v_emp_lname employees.last_name%TYPE;
begin
    select last_name
        into v_emp_lname
        from employees
        where employee_id = 100;

    dbms_output.put_line('His last name is' || v_emp_lname);
end;
```

Retrieving data in PL/SQL Example:
- Outputs the hiredate and salary of an employee with and `employee_id = 100`.
```SQL
declare 
    v_emp_hiredate employees.hire_date%TYPE;
    v_emp_salary employees.salary%TYPE;

begin
    select hire_date, salary
        into v_emp_hiredate, v_emp_salary
        from employees
        where employee_id = 100;

    dbms_output.put_line('Hiredate: ' || v_emp_hiredate);
    dbms_output.put_line('Salary: ' || v_emp_salary);
end;
```

- Output the sum of salaries for all employees in a specific department

```SQL
declare 
    v_sum_salary number(10,2);
    v_department_number number not null := 60;

begin
    select sum(salary)
        into v_sum_salary 
        from employees
        where department_id = v_department_number;
    
    dbms_output.put_line('Dep #60 Salary Total: ' || v_sum_salary);
end;
```

- The order of variables must correspond with the order of the items selected.
- The `select` statement within the PL/SQL block falls into ANSI classification of 
embedded SQL for which the following rule applies: *embedded queries must return exactly 
one row*.
    - A query that returns more than one row or no rows generates an error.

### Create and Copy of Original Table
- In some cases, tables should not be modified.

```SQL
-- copies the `employees` table into `copy_employees`
create table copy_employees
    as select *
    from employees;
```

## DML in PL/SQL

- You can issue DML commands in PL/SQL without restriction.
    - the `insert` statement adds new rows to the table
    - the `update` statement modifies existing rows in the table
    - the `delete` statement removes rows from the table
    - the `merge` statement selects rows from one table to update and/or insert into 
    another table. 
        - you cannot update the same row of the target table multiple times in the same 
        `merge` statement.
        - you must have `insert` and `update` privileges in the target table and the 
        `select` privilege in the source table

### Inserting Data Example

```SQL
begin 
    insert into copy_employees
    (employee_id, first_name, last_name, email, hiredate, job_id, salary)
    values
    (99, 'Ruth', 'Cores', 'RCORES' sysdate, 'AD_ASST', 4000);

end;
```

### Updating Data Example

```SQL
declare 
    v_salary_increase employees.salary%type := 800;

begin
    update copy_employees
        set salary = salary + v_salary_increase
        where jod_if = 'ST_CLERK';
end;
```

### Deleting Data Example

```SQL
declare 
    v_department_number employees.department_id%type := 10;
begin
    delete from copy_employees
        where department_id = v_department_number;
end;
```

### Merging Rows Example

```SQL
begin 
    merge into copy_employees c using employees e
        on (e.employee_id = c.employee_id)
    when matched then
        update set
            c.first_name = e.first_name,
            c.last_name = e.last_name,
            c.email = e.email
    when not matched then
        insert values(e.employee_id, e.first_name, e.last_name, e.mail);
end;
```


---

## Getting Information from a Cursor
- The cursor provides useful information to obtain the status of an operation
- Every time a SQL statement is about to be executed, the Oracle server allcoates
a private memory area to store the SQL statement and the data it uses. This memory is 
called the **implicit cursor**.
    - You have no direct control over implicit cursor.
    - You can use predefined PL/SQL variables called **implicit cursor attributes** to 
    find out how many rows are processed by the SQL statement.

<br>

### Implicit and Explicit Cursor
- Implicit Cursor is defined automatically by Oracle for all SQL DML statements
and for queries that return only one row. 
    - Implicit Cursor is always automatically named "SQL".

- Explicit Cursor is defined by PL/SQL programmer for queries that return more than one 
row.

<br>

### Cursor Attributes for Implicit Cursors
- Cursor attributes are automatically declared variables that allow you to evaluate what 
happened when a cursor was last used.
- Attributes for implicit cursors are prefaced with "SQL".
- Use these attributes in PL/SQL statements, but not in SQL statements.
- Using cursor attributes can test the outcome of your SQL statements. 

<br>

### Cursor Attributes for Implicit Cursor

| Attribute     | Description                                                                                    | 
|---------------|------------------------------------------------------------------------------------------------|
| `SQL%FOUND`   | Evaluates to `true` if the most recent SQL statement returned at least one row                 |
| `SQL%NOTFOUND`| Evaluates to `true` is the most recent SQL statement did not return even one row.            | 
| `SQL%ROWCOUNT`| An integer value that represents the number of rows affected by the most recent SQL  statement|


<br>

### Using Implicit Cursor Attributes

```SQL
-- SQL%ROWCOUNT for deleted rows.
declare 
    v_department_number copy_employees.department_id%type := 50;
begin
    delete from copy_employees
        where department_id = v_department_number;
    
    dbms_output.put_line(SQL%ROWCOUNT || ' row deleted.');
    -- prints the number of rows deleted.
end;
```

```SQL
-- SQL%ROWCOUNT for updated rows.
declare 
    v_salary_increase employees.salary%type := 800;
begin
    update copy_employees
        set salary = salary + v_salary_increase
        where job_id = 'ST_CLERK';
    
    dbms_output.put_line(SQL%ROWCOUNT || ' row updated.');
    -- prints the number of rows updated.
end;
```

<br>

---

# Writing Control Structure

Control structures usually involve conditional expressions that evaluate to a boolean
values. PL/SQL uses a ternary logic that involves `null` values. The evaluation of 
Ternary logical statements are described [here](https://en.wikipedia.org/wiki/Three-valued_logic).

Ternary logic can be less intuitive to some, here are useful things to keep in mind
when evaluating boolean expressions with `null`:
- simple comparisons involving `null` **always** results to `null`.
    - comparing `null = null` yields to `null`.
- the logical operator `not` applied to a `null` yields `null`.
- in a conditional statement, if a condition yields `null`, it behaves like a `false`,
and the associated sequence of statements is not executed.

## Conditional Statements
- PL/SQL has two conditional (or branching statements): `case` and `if` statements.

<br>

### If Statement Structure
Synactical Construction:
```
conditional ::= if [expr::condition] then [expr] 
                -- [elsif_clause ] -- 
                -- else [statement] --
                end if;

elsif_clause ::= elsif [expr::condition] then [statement] | [elsif]
```
- `expr:condtion` evaluates to a boolean expression that returns `true`, `false`, or 
`null`.

Example:

```SQL

declare 
    v_age number := 20;
begin
    if v_age < 11 then
        dbms_output.put_line('I am a child');
    elsif v_age < 18 then
        dbms_output.put_line('I am ' || v_age || 'yrs old. ');
    else 
        dbms_output.put_line('I am an adult');
    end if;
end;
```

- Note that undeclared variables defaults to `null`.

### Case Statement Structure
- For longer sequence of if-elseif-else statements, Case statements are preferred for
readability.

Syntactical Construction
```
case ::= case [variable]
	 [when_clause]
	 else [statement] 
	 end case;

when_clause ::= when [condition::variable] then [statement] | [when_clause]
```

Example:

```SQL
declare 
    v_number  number := 15;
    v_txt varchar2(50);
begin
    case v_number
        when 20 then v_txt := 'number is 20.';
        when 17 then v_txt := 'number is 17.';
        when 15 then v_txt := 'number is 15.';
        when 10 then v_txt := 'number is 10.';
        else v_txt := 'some other number.';
    end case;
    dbms_output.put_line(v_txt);
end;
```

PL/SQL also provide `case` expressions which returns a value that may be bound to a 
variable.

```SQL
-- case expression
declare 
    v_out varchar(15);
    v_in number := 50;
begin
    v_out := case v_in
        when 1 then 'low value'
        when 50 then 'middle value'
        when 99 then 'high value'
        else 'other value'
    end;
    dbms_output.put_line(v_out);
end;
```

## Looping Statements
PL/SQL provides 3 kinds of loops:
- basic loops - useed when statements inside the loop must execute at least once.
- for-loop - used when beginning and end of the loop is known. 
- while-loop - terminates when a condition is `false` or `null`.

Syntactical construction for basic loops
```
basic_loop::= loop
    [stmt];
    exit [cl:when];
end loop;
```

Syntactical construction for for-loops
```
::| loop terminates when the condition is false or null
for_loop ::=  for [name::variable] in -- reverse -- [range]  
    [stmt];
end loop;

range ::= [value::lower] .. [value::upper]
```
- `revserse` causes the loop to decrement each iteration from upper bound to lower bound.
- Bounds should not be `null`.

Syntactical construction for while-loops
```
::| loop condition terminates when false or null
while_loop ::= while [expr:conditional] loop
    [stmt];
end loop;
```
- If the condition yields `null`, then the loop is 
bypassed and control passes to the statement that follows 
the loop.

---
## References
- https://docs.oracle.com/cd/A87860_01/doc/appdev.817/a76939/adg09dyn.htm.
- https://docs.oracle.com/cd/B19306_01/appdev.102/b14261/selectinto_statement.htm
- https://en.wikipedia.org/wiki/Three-valued_logic



## Online compiler for PL/SQL
- https://livesql.oracle.com/