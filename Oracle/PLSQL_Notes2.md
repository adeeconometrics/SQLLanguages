# Working with Composite Data Types

Composite Data Types
- Stores values that have internal components
- Passing an entire composite variable as parameters to subprograms are possible.
- You can access internal components of composite variables individually.
    - Internal components can either be scalar or composite.
- You can use scalar components wherever you can use scalar variables. You can use
composite components wherever you can use composite variables of the same type.

<br>

## PL/SQL Records
- You cannot create a `record` type at schema level. Therefore, a `record` type cannot be an ADT attribute data type.
- A record is a composite data type consisting of a group of related data items stored as
fields, each with its name and data type. 
    - You can refer to a whole record by its name and/or to individual fields by their 
    names.
- PL/SQL records must contain one or more components/fields of any scalar or composite 
type
- PL/SQL records are not the same as rows in database table
- PL/SQL records can be assigned with initial values and can be defined as `not null`.
- PL/SQL records can be components of other records i.e., nested records. 
- PL/SQL records are initialized to `null` in each fields that are not specified.
- PL/SQL records variable **does not inherit** the initial value of the referenced item. 
- PL/SQL records may be declared anywhere that a scalar variables can be declarred e.g. 
annonymous blocks, functions, procedures, package specification, package body, triggers, etc.
    - The scope and visibility of a PL/SQL record is the same as scalar variables.

Syntax:

```
[id::record] [id::table] %ROWTYPE;
```
- `%ROWTYPE` is used to declare a record based on another record. 

### Defining your own Records
- It is possible to declare your own record structures containing any fields you like.
- Start with `type` keyword.
- It must include at least one field and the other fields may be defined using scalar data
types such as `date`, `varchar2`, or `number` or using attributes such as `%type` and `%rowtype`.

Syntax:
```
type [id::type] is record ([fields]);
[id] [type:record];

fields ::= [id] [type] -- not null | defatult := [value] -- --, [fields] --
```
### Ways to Create Records
- Define a `record` type and then declare a variable of that type.
- Use `%rowtype` to declare a record variable that represent either a full or partial row
of a database table or view.
- Use `%type` to declare a record variable of the same type as previously declared record 
variable.

Example:

```SQL
declare 
    type person_rec is record(
        first_name employees.first_name%type,
        last_name employees.last_name%type,
        department_name departments.department_name%type
    );

    v_person_department_record person_rec;
begin
    select e.first_name, e.last_name, d.department_name
        into v_person_department_record
        from employees e join departments d
            on e.department_id = d.department_id
        where employee_id = 200;

    dbms_output.put_line(v_person_department_record.first_name ||
    ' ' || v_person_department_record.last_name || ' is in the ' ||
    v_person_department_record.department_name || ' department.' );

end;
```

---
## Explicit Cursors

### Context Areas and Cursors
- The Oracle server allocates a private memory area called a context area to store the 
data 
processed by a SQL statement.
    - Every context area (and therefore every SQL statement) has a cursor associated 
    with it.
- Cursor are comparable to a label for a context area, or a pointer to the context area. 
In fact, it is both.

### Implicit and Explicit Cursor
- Implicit Cursor are automatically defined by Oracle for all DML statements and for 
`select` statements that return only one row.

- Explicit Cursor are declared by the programmer for queries that return more than one 
row. This is useful to name a context area and access its stored data.

<br>

### Limitations of Implicit Cursor
> Programmers must think about the data that is possible as well as the data that exists 
now.
- If ever there is more than one row in the table selected (without `where` clause),
it will cause an error.

Example:

```SQL
declare
    v_salary employees.salary%type;
begin
    select salary into v_salary
        from employees;
    
    dbms_output.put_line('Salary is: '|| v_salary);
    -- this will prompt an error.
end;
```

 <br>

### Explicit Cursor
- Allows you to retrieve multiple rows from a database table, have a pointer to each row 
that is retrieved, and work on the rows one at a time.
- Reasons to use an explicit cursor:
    - It is **the only way in PL/SQL** to retrieve more than one row from a table.
    - Each row is fetched by a separate program statement, giving the programmer **more 
    control** over the processing of the rows.

Example:

```SQL
declare 
    cursor dept_cur is 
        select department_id, department_name
            from departments;
    v_department_id deparments.department_id%type;
    v_department_name departments.department_name%type;

begin 
    open dept_cur;
    loop
        fetch dept_cur into v_department_id, v_department_name;
        exit when dept_cur%notfound;
        dbms_output.put_line(v_department_id || ' '|| v_department_name);
    end loop;
    close dept_cur;
end;
```

<br>

### Explicit Cursor Operations
- The set of rows returned by a multiple-row query is called the **active set**,
and is stored in the context area.
- The size of the explicit cursor is the number of rows that meet your query criteria.
- To get the data in an explicit cursor, you must `open` the cursor and `fetch` each row  
from it one at a time. Once done, you have to `close` the cursor.

### Steps for using Explicit Cursors
1. Declare a cursor.
    - The cursor in the declarative section is defined by the SQL `select` statement.
    - Do not include the `into` clause in the cursor declaration because it appears
    later in the `fetch` statement. 
    - If processing rows in a specific sequence is required, then  use the `orderby` 
    clause in the query.
    - The cursor can be defined in any valid `select` statements including `join`, 
    `subqueries`, etc.
    - If a cursor declaration references any PL/SQL variables, these variables must be 
    declared before declaring the cursor.
    
    ```
    cursor [id::cursor] is [stmt:select];
    ```
2. `open` the cursor. Fill the cursor active set with data.
    - This will populate the cursor's active set with the results of the `select`
    statement in the cursor's definition. 
    - `open` statement positions the cursor pointer at the first row.
    - `open` statement allocates memory for a context area.
    - `open` statement executes the `select` statement in the cursor declaration,
    returning the results into the active set.

3. `fetch` the contents of that cursor. Retrieve the current row into variables.
    - Inside this statement, the cursor will unpack its contents. The `exit when`
    checks to see if the `fetch` reached the end that is `exit when [id::cursor]%notfound`.
    ```
    fetch [id::cursor] into [id::variable,...]
    ```
4. Define `exit` condition to terminate the loop.
    ```
    exit when [id::cursor]%notfound;
    ```
5. `close` the statements to control a cursor. Release the active set.
    - `close` statement release the active set of rows.
    - At this point, it is now possible to reopen the cursor to establish a fresh active 
    set using a new `open` statement. 
    - If cursor is reopened, the associated `select` statement is re-executed to 
    repopulate the context area with the most recent data from the database.
    - Opening a closed cursor will raise an `INVALID_CURSOR` exception.

<br>

#### Guidelines for Fetching data from the Cursor
- Include the same number of variables in the `into` clause of the `fetch` statement as 
columns in the `select` statement, and be sure that the data types are compatible.
- Match each variable to correspond to the columns position in the cursor definition.
- Use `%type` to ensure data types are compatible between variable and table.
- Test to see whether the cursor contains rows.
- If a fetch acquires no values, then there are no rows to  process (or left to process) 
in the active set and no error is recorded.
- You can use the `%notfound` cursor attribute to test for the exit condition.

---
## Explicit Cursor Attributes
- When appended to the cursor variable name, these attributes return useful information
about the execution of a cursor manipulation statement. 

| Attribute  | Type    | Description                                               |
|------------|---------|-----------------------------------------------------------|
| `%isopen`  | Boolean | Returns `true` if cursor is open.                         |
| `%notfound`| Boolean | Returns `true` if most recent fetch did not return a row. |
| `%found`   | Boolean | Returns `true` if most recent fetch returned a row.       |
| `%rowcount`| Number  | Returns the total number of rows `fetch` so far.          |


---
## Cursor For Loop
- Process rows in an explicit cursor.
- It is a shortcut because the cursor is opened, a row is  fetched once for each 
iteration in the loop, the loop exits  when the last row is processed, and the cursor is 
closed automatically.

- You can simplify your coding by using a cursor FOR loop instead of the `open`, `fetch`, and `close` statements. 
- No variables are declared to hold the fetched data, and no `into` clause is required. 

Syntax:

```
for [id::record] in [id::cursor] loop
    [stmt];
end loop;
```

Example:

```SQL
declare
    cursor emp_cur is 
        select employee_id, last_name
            from employees
            where department_id = 50;

begin
    for _iter in emp_cur loop
        dbms_output.put_line(
            _iter || ' ' || _iter.last_name
        );
    end loop;
end;
```

<br>

### Guidelines for Cursor `FOR` loops

- Do not declare the record that controls the loop because it is declared implicitly.
- The scope of the implicit record is restricted to the loop. You cannot reference the
record outside the loop.
- You can access fetched data using `[id::cursor_local].[id::column]`.
- Testing cursor attributes are still valid.
- Cursor is closed automatically.
- You can specify the `select` statement on which the cursor is based directly in the for 
loop: `for [id:record] in [stmt:select] loop [stmt] end loop;`.
    - In this case, you cannot reference cursor attributes. 

<br>

---
## Cursor with Parameters For Update Clause

### Cursors with Parameters
- A parameter is a variable whose name is used in a cursor declaration. 
    - When the cursor is opened, the parameter value is passed to the Oracle server,which uses it 
    to decide which rows to retrieve into the active set of the cursor
    - You can open and close an explicit cursor several times in a block, or in different 
    executions of the same block, returning a different active set on each occassions.
    
- Each parameter named in the cursor declaration must have its corresponding value in the `open`
statement.
- You do not give sizes on the parameters.
- Parameter data types are the same as those for scalar variables. 
- Parameter names are used in the `where` clause of the cursor `select` statement. 

Syntax for defining a cursor:
```
cursor [id::cursor] ([parameters]) is [stmt:select];

parameters ::= [type] [id::parameter] --, [parameters] --
```

Syntax for opening a cursor with parameters:
```
open [id::cursor] ([arguments]);

arguments ::= [value] --, [arguments] --
```

Example

```SQL
declare 
    cursor country_cur (p_region_id numbe) is 
        select country_id, country_name 
            from countries
            where region_id = p_region_id;
    
    v_country_record country_cur%rowtype;

begin
    open country_cur(5);
    loop 
        fetch country_cur into v_country_record;
        exit when country_cur%notfound;
        dbms_output.put_line(
            v_country_record.country_id || ' ' 
            || v_country_record.country_name
        );
    end loop;
    close country_cur;
end;
```

<br>

### Cursor `FOR` loops

```SQL
declare 
    cursor employees_cur (p_department number) is
        select employee_id, last_name 
            from employees
            where department_id = p_department;
begin
    for _cursor in employees_cur(10) loop
        dbms_output.put_line(
            _cursor.country_id || ' ' 
            || _cursor.country_name
        );
    end loop;
end;
```

<br>

### Using Cursors `FOR UPDATE`
- When `for update` is applied in a cursor, each row is locked as we open the cursor.
    - This prevents the users from modifying the rows while our cursor is open.
    - This allows us to modify the rows ourselves using a `where current of` clause.
- This does not prevent other users from reading the rows.

Syntax:
```
cursor [id::name] is 
    [stmt:select]
        for update -- of [id] -- -- [wait] | nowait --  
```
If the row have been locked by another session:
- `nowait` returns an Oracle server error immediately
    - Tells the server not to wait if any of the requested rows have been locked by another user.
    - Control is immediately returned to your program so that it can do other work before trynig again to acquire the lock.
- `wait n` waits for `n` seconds, and returns an Oracle server error if other sessions
is still locking the rows at the end of that time. 

- No argument: tells the server to wait indefinitely until the rows are available.

Example:

```SQL
declare 
    cursor employee_cur is
        select e.employee_id, d.department_id
            from employees e, departments d
            where e.department_id = d.department_id
            and department_id = 80
            for update of salary;
...
```

### Using `WHERE CURRENT OF` clause
- Used in conjunction with the `FOR UPDATE` clause to refer to the current row (the most recently 
`fetch` row) in an explicit cursor i.e., you must include a `for update` clause in the cursor 
query so that the rows are locked on `open`.
- Used in the `update` or `delete` statement, whereas the `for update` clause is specified in the 
cursor declaration.
- Used for updating or deleting the current row from the corresponding database table.
- Enables you to apply updates and deletes to the row currently being addressed without the need 
to use a `where` clause.




Syntax:
```
where current of [id::cursor];
```
- `[id::cursor]` is the name of the cursor that must have been declared with the `for update` clause. 

Example:

```SQL
declare 
    cursor employee_cur is
        select employee_id, salary, department_name
            from employees e, departments d
            where e.department_id = d.department_id
            for update of salary nowait;
begin
    for _cursor in employee_cur loop
        update employees
            set salary = _cursor.salary * 1.1
            where current of employee_cur;
    end loop;
end;
```




---
## References
- https://docs.oracle.com/database/121/LNPLS/composites.htm#LNPLS005
- https://docs.oracle.com/cd/A91202_01/901_doc/server.901/a88827.pdf















