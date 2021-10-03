# Creating and Calling Procedures

### Definition: Subprogram
- A subprogram is either a procedure or a function.
    - You use a function to return a value.
    - You use a procedure to perform an action. 

### What is possible with a subprogram
- Subprograms can create a function that must return a single value, not including out-parameters.
- Subprograms are building blocks of packages and triggers.
- Subprograms can create a package/group of functions.
- Named procedure do not return values except as out-parameters.

### Benefits of Subprograms
- Modularity
- Maintainability - as modifications need only be done once to improve multiple applications using the same routine, it minimizes the testing.
- Code reuse
- Improved data security - subprograms can be granted with security privileges.
    - Run the privileges of the subprogram owner, not the user.
- Data integrity
- Performance - subprograms are compiled once and not everytime it is requested. Subprograms are cached.
- Clarifies code. 

### Differences between Anonymous blocks and Subprograms

| Annonymous blocks           | Subprograms                | 
| ----------------------------| ---------------------------|
| Unnamed                     | Named                      |
| Not reusable                | Reusable                   |
| Compiled on every execution | Compiled once when created | 
| Not stored in the database  | Stored in the database     |
| Do not return values        | [Must] Return values       | 
| Does not take parameters    | Can capture parameters     |

<br>

### Syntactical difference between Anonymous blocks and Subprograms
- The block structure of Subprograms to Anonymous blocks are similar.
```
::| Anonymous block
declare 
    -- [id:variables] --
    -- [id:records] --
    -- [id:cursors] --
begin
    -- [stmt:sql] -- 
end;

::| Subprograms
create -- or replace -- procedure [id] -- [parameters, ...] -- is|as
    [id:variables]
    -- [id:records] --
    -- [id:cursors] --
begin
    -- [stmt:sql] --
end;
```

<br>

### Parts of a Subprogram
- Declarative part (optional)
- Executable part (required)
- Exception handling part (optional)

<br>

### Subprograms: Procedures and Functions
- As metioned, subprograms can either be a named procedure or a function. 
- Both functions and procedures are named PL/SQL blocks which are called subprograms.
- Both functions and procedure have a similar block structures.
- Both functions and procesure can return `out` and `in out` parameters. 
- A function must return a value using `return` statement. Whereas the `return` statement to named procedure are optional.
    - `return` statement within a function returns back the control to the caller and the 
    corresponding result from a function. Meanwhile, the `return` statement for named procedures 
    only returns the control i.e. there is no corresponding value.
- A function can be called from SQL, procedures cannot.
- Functions are considered as expressions, procedure are considred as statements. 

<br>

### Definition: Procedure
- A procedure is a named PL/SQL block.
- A procecure is used to perform an action. 
- A procedure is compiled and stored in the database as a schema obeject. 

Syntax:
```
create -- or replace -- procedure [id] -- [id::parameter] [type], ... -- is | as
-- [id::local_variable] [type]; ... --
begin
    [sql:stmt];
end;
```

Example:
```sql
create or replace procedure add_department is
    v_dept_id dept.department_id%type;
    v_dept_name dept.department_name%type;
begin
    v_dept_id := 280;
    v_dept_name := 'ST-Corriculum';

    dept(department_id, department_name) insert into values (v_dept_id, v_dept_name);
    dbms_output.put_line('Inserted ' || SQL%ROWCOUNT ||' row(s).' );
end;
```

<br>

### Invoking Procedures
- Named procedures can be called from:
    - anonymous block
    - another procedure
    - application
- Named procedure cannot be called inside a SQL statement.

Syntax:
```
begin
    [id::procedure] -- ([value:arguments], ...) --;
end;
```

Example:
```SQL
begin
    add_department;
end;
```

<br>

### Notes on Procedures
- Even when compilation error occurs, named procedures are cached in the database. There are two ways to correct a buggy code:
    - use a `create or replace procedure` statement to overwrite the existing code.
    - `drop` the procedure first then execute `create procedure` statement. 

----

## Notes on Parameters
- Parameters are variables that communicates with the named procedure / function.
- Parameters act like local variables.
- Convention `p_[name]` prefix.

### Types of Parameters
- Formal Parameter - parameters declared in the procedure heading.
- Actual Parameter - actual values that are passed into the procedure (also known as arguments).
    - can be literal values, variables, or expression

### Modes on Passing Parameters
- `in` (default) - provides values for a subprogram to process
- `out` - returns a value to the caller.
- `in out` - supplies input value which can be returned as a modified value to the caller.

### Syntax for passing parameters
- Positional - pass the parameter in the order that it is declared e.g. `procedure (3, 'True', 45.0)`.
- Named - pass the parameter with the name associated with it; one may not follow order e.g. `procedure(p_1=>3, p_3=>45.0, p_2 => 'True')`.
- Combination - pass the parameter with respect to its order; one may annotate the name of the parameter e.g. `procedure(2, p_2= 'True', 45,0)`.

---

# References
- https://docs.oracle.com/database/121/LNPLS/subprograms.htm#LNPLS653