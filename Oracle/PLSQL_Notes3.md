# Handling Exceptions

### What is an exception?
- An exception occurs whenever an error is found during the execution of a program that disrupts 
the normal operation of the program.
- Exceptions happen because of unexpected events that our program encounters; It may come from 
user error, logic error, or faulty transactions. Handling exceptions improve software 
maintainability and user experience.
    - When the code does not work as expected, it raises an exception.
    - When the exceptions are raised, the rest of the PL/SQL block is not executed; It either 
    jumps to an Exception handler or crash the program abruptly.

<br>

### Common sources of Errors 
- System Error
- Data Error
- User Action Error
- Other things

<br>

### Some Reason Include Exception
- Protects users from errors with unhelpful messages, sudden software crashes. 
- Protects the database from errors since data can be lost or overwritten. 
- Errors may be costly in time and resources. 

<br> 

### Exception Handlers
- A block that always terminate when PL/SQL raises an exception, but you can specify an exception 
handler to perform final actions before the block ends. 
    - The exception section begins with the `exception` keyword. 

Syntax for Trapping Exceptions
```
exception ::= exception
    [exception:when]

    when ::= when [exceptions] then [stmt]; | [exception:when]

exceptions ::= [type:exception] --, [exceptions] --
```

Example:

```SQL
declare
    v_country_name countries.country_name%type := 'Korea, South';
    v_elevation coutnries.highest_elevation%type;
begin
    select highest_elevation into v_elevation
        from coutnries 
        where country_name = v_country_name;
    
    dbms_output.put_line(v_elevation);

    exception
        when no_data_found then
        dbms_output.put_line(
            'Country name, ' || v_country_name || ', cannot be found. Try again.'
        );
end;
```

- Exception handlers only traps only those exceptions that are specified;
any other exceptions are not trapped unless you use `others` exception handler in 
which case traps all exceptions that are not already trapped. 
    - `others` must be the last exception handler -- similar to `finally` block in Java.

<br>

### Guidelines when trapping exceptions
1. Always add exception handlers whenever there is a possibility for error.
2. Error are especially likely during calculations, string manipulation, and
SQL database operations. 
3. Handle named exceptions whenever possible, instead of using `others` in exceptions handlers.
4. Learn the names and causes of predefined exceptions. 
5. Test your code with different combinations of bad data to see what potential errors arise.
6. Write out debugging information in your exception handler. 
7. Carefully consider whether each exception handler should commit the transaction, roll it back,
or let it continue.
8. No matter how severe the error is, you want to leave the database in a consistend state and 
avoid storing any bad data.

---
## Trapping Oracle Server Exceptions and User Defined Exceptions

### Exception Types
- Oracle Server Errors - predefined and non-predefined.
    - errors that are recognized and raised automatically by the Oracle Server.
- User-defined exceptions

<br>

### Handling Exceptions with PL/SQL
- Implicitly (automatically) by the Oracle server
    - exceptions are raised automatically by the server
    - exception types are predefined
- Explicitly (manually) by the programmer
    - exceptions are raised manually by the programmer.
        - explicitly raised using a `raise` exception
        - exception raised may be user-defined or predefined


<br>

### Oracle Server Errors
- Predefined 
    - Has a predefined name, in addition to a standard Oracle error number (ORA-#####) and 
    message e.g. `ORA-01403` raise the predefined exception `NO_DATA_FOUND`,
- Non-predefined 
    - Have no predefined name but has Oracle error number.
    - You can declare your own names for these so that you can reference these names in the 
    exception section. 

<br>

### Trapping Predefined Oracle Errors
- Reference the predefined name in the exception handling routine. 
- Sample set of Predefined Exceptions:
    1. `NO_DATA_FOUND`
    2. `TOO_MANY_ROWS`
    3. `INVALID_CURSOR`
    4. `ZERO_DIVIDE`
    5. `DUP_VAL_ON_INDEX`

<br>

### Trapping Non-Predefined Oracle Server Errors 
- Handling non-predefined exceptions are similar to predefined exceptions but it does not have a 
name. In addition, these errors does not have a standard Oracle Error Number and Error message. 
- To use specific handers (rather than handling through an `others` clause), you can create your 
own names for them in the `declare` section and associate the names with the specific `ORA-#####`
numbers using the `PRAGMA EXCEPTION_INIT` function. 
- The declared exception is raised implicitly. In PL/SQL, the  `PRAGMA EXCEPTION_INIT` tels the 
compiler to associate an exception name with a specific Oracle error number. This allows you to refer to any Oracle Server exception by a name and to write a specific handler for it.

Example:
```SQL
declare 
    e_insert_exception exception;
    pragma exception_init(e_insert_exception, -01400)
begin
    insert into departments(department_id, department_name)
        values(280, null);
    exception
        when e_insert_exception then
            dbms_output.put_line('insert failed.');
end;
```

<br>

### Functions for Trapping Exceptions
- Based on the values of the code or the message, you can decide which subsequent actions to take.
    - SQLERRM returns character data containing the message associated with the error number.
    - SQLCODE returns the numeric value for the error code. (You can assign it to a number 
    variable).

| SQLCODE value | description                         |
|---------------|-------------------------------------|
|0              | No exception is encountered.        |
|1              | User-defined exception.             |
|+100           | `NO_DATA_FOUND` exception.          |
|-[number]      | Another Oracle Server error number. |
- `+100` is an internationally-agreed code when no rows are returned from a query.

- You cannot use SQLCODE or SQLERRM directly in SQL statement. You must assign their values
to local variables then use the variables in the SQL statement.

Example:
```

```

---
## Reference
- https://www.tutorialspoint.com/plsql/plsql_exceptions.htm










