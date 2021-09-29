PLSQL -- does not include error handling code and incomplete expression for object types 

```
declare ::= declare [declare_clause]

declare_clause ::= [name::variable] [type] -- := [value::default] -- ; | [declare_clause]

plsql ::= begin -- [statement] -- end;

statement ::= [expr] | [statement::sql] | [statement::null] | [statement::goto]

expr ::= [unary] | [binary] | [ternary] | [expr]

conditional ::= if [expr::condition] then [expr] 
                -- [elsif_clause ] -- 
                -- else [statement] --
                end if;

elsif_clause ::= elsif [expr::condition] then [statement] | [elsif]

case ::= case [variable]
	 [when_clause]
	 else [statement] 
	 end case;

when_clause ::= when [condition::variable] then [statement] | [when_clause]

loop ::= -- for [name::variable] in [range] -- loop [expr] -- [exit] | [continue] -- | [loop] | end loop;

continue ::= continue; | continue when [expr::condition];

exit ::= exit; | exit when [expr::condition];

types ::= [scalar] | [composite] | [reference] | [large object]

function ::= create -- or replace -- procedure [name::function] ([parameter]) as [plsql]

parameter ::= [name::argument] in [type::argument] | [name::argument] out [type::argument] | [name::argument] in out [type::argument] |, [parameter]

object ::= -- create | create or replace -- type [name::object] as object ([variable] | [method] ,)

```
---

- PLSQL fully supports SQL data types.
- PL/SQL supports both static and dynamic SQL
	- static SQL is known at compile time
	- dynamic SQL is known until runtime

- PLSQL achieves high performance because an entire block of statements are sent to the DB at one time which reduce network traffic.
- PL/SQL stored subprograms are compiled once and stored in executable form, so subprogram calls are efficient.
	- Stored subprograms are cached and shared among users, which lowers memory requirements and call overhead.

- PL/SQL saves time on design and debugging by offering a full range of software-engineering features, such as exception handling, encapsulation, data hiding, and object-oriented data types.



