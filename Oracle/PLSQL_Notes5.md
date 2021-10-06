# Creating Packages

### Reasons for using packages
- Packages helps modularity
- Abstraction (information hiding)
- Maintainability 

### PL/SQL Package
- PL/SQL package are Containters that group together related PL/SQL subprograms, variables, cursors and exceptions

### Components of a PL/SQL Package
- Package Specification - the interface to your applications
- Package Body - contains the definition of the subprograms that were declared in the package specification.

Note that if any changes are made in the body, the applications that communicates with the interface
may not necessarily be recompiled. The two-part structure is an example of a principle in modular
programming called **encapsulation**.

Syntax for package specification: 
```
create -- or replace -- package [id::package] [modifier]
    public [entities]
end [id:package];    

modifier ::= is | as
entities ::= [variable] | [subprogram] | [cursor] | [exceptions] | [types] | [subtypes] | [constants]
```
- All constructs declared in the package specifications are automatically `public`.
- Constants are declared in the package specification
- Package specification can exist without the body

Example
```SQL
create or replace package check_emp_pkg is
    c_max_length_of_service constant number := 100;
    procedure check_hiredate (p_date in employees.hiredate%type);
end check_emp_pkg;
```

Syntax for package body:
```
create -- or replace -- package body [id::package] [modifier]
    public [entities]
end [id::package];    

modifier ::= is | as
entities ::= [variable] | [subprogram:bodies] | [cursor] | [exceptions] | [types] | [subtypes] | [constants]
```
- Define the subprograms in appropriate order
- Every subprogram declared in the package specification must also be included in the package body
- Package body cannot exist without package specification which contains its declarations

Example: 
```sql
create or replace package body check_emp_pkg is
    procedure check_hiredate (p_date in employees.hiredate%type) is
        begin
            if months_between(sysdate, p_date) > g_max_length_of_service * 12 then
                raise_application_error(-20200, 'Invalid Hiredate');
            end if;
    end check_hiredate;
end check_emp_pkg;
```

### Invoking Package Subprogram

| Context                | Syntax                           | 
|------------------------|----------------------------------|
| Within the same package| `[id::subprogram];`              |
| External to the package| `[id::package].[id::subprogram];`|


Removing a pakage:
- `drop package [id::package];`
- `drop package body [id::package];`
- It is not possible to remove the package specification on its own

Describe a package
- `describe [id::package];`

- A package may contain private types