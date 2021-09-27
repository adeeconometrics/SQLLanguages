### CFG


The grammar below express syntactical constructions of Oracle SQL Database lite which is a subset of SQL language supported by Oracle.


```
SQL ::= [ddl:stmt] | 
		[dml:stmt] | 
		[dql:stmt] | 
		[tcl:stmt]

ddl:stmt ::= [stmt:create] | 
		[stmt:alter] | 
		[stmt:drop] | 
		[stmt:grant] | 
		[stmt:revoke]

	create ::= [create:schema] | 
			[create:sequence] | 
			[create:synonym]  | 
			[create:table] | 
			[create:user] | 
			[create:view] | 
			[create:index] | 
			[create:database]

		schema ::= create schema [id::schema].[schema:create_table_list];
			create_table_list ::= create table [create:table]

		sequence ::= create sequence -- [id::schema]. -- [id::sequence] -- [sequence:command] -- ;
			command ::= increment by [value::integer] | 
					starts with [value::integer] | 
					maxvalue [value::integer] | 
					nomaxvalue | 
					minvalue [value::integer] | 
					nominvalue | 
					cycle | 
					nocycle |
					cache | 
					no cache | 
					order | 
					no order

		synonym ::= create -- public -- synonym -- [id::schema]. -- [id::synonym] for -- [id::schema]. -- [id:object]; 

		table ::= create table -- [id::schema]. -- [id::table] -- ([table:column_list]) | as [subquery] --;
			column_list ::= [id::column] [type] -- auto increment | default [expr] -- -- [cl:constraint] -- --, [column_list] -- 
		 
		user ::= create user [id::user] identified by [password];

		view ::= create -- or replace -- -- force | no force -- view -- [id::schema]. -- [id::view] -- ([view:alias]) -- as [subquery];
			alias ::= [id::alias] --, [alias] -- 

		index ::= create -- unique -- index -- [id::schema]. -- [id::index] on -- [id::schema]. -- [id::table]([index:columns]) -- key columns = [number] --;
			columns ::= [id::column] -- asc | desc -- --, [columns] -- 

		database ::= create database [id::database] [database:parameters];
			parameters ::= [parameters:parameter] --, [parameters:parameter] --
				parameters ::= database_id [id::database] |
						extend_size [value::npages] |
						database_size [value::maxbytes]

	alter ::= [alter:sequence] | 
			[alter:session] | 
			[alter:table] | 
			[alter:user] | 
			[alter:view] 

		sequence ::= alter sequence -- [id::schema]. -- [id::sequence] -- -- increment by integer | maxvalue integer | minvalue integer | nomaxvalue | nominvalue --;
		session ::= alter session set [value::date_value] = [value::date_value];
		table ::=
		user ::= alter user [id::user] identified by [password];
		view ::= alter view -- [id::schema]. -- [id::view] compile; 


	drop ::= [drop:index] | 
			[drop:schema] | 
			[drop:sequence] | 
			[drop:synonym] | 
			[drop:table] | 
			[drop:user] | 
			[drop:view] | 
			[drop:index]

		index ::= drop index -- [id::schema]. -- [id::index];
		schema ::= drop schema [id::schema]. -- cascade | restrict --;
		sequence ::= drop sequence -- [id::schema]. -- [id::sequence];
		synonym ::= drop -- public -- synonym -- [id::schema] -- [id::synonym];
		table ::= drop table -- [id::schema]. -- [id::table] -- cascade | cascade constraints | restrict --;
		user ::= drop user [id::user] -- cascade --;
		veiw ::= drop -- [id::schema]. -- view [id::view] -- cascade | restrict --;
		index ::= drop index -- [id::schema]. -- [id::index];


	grant ::= grant [grant:role] to [user_list];
		role ::= [id::role] | [role:privelage_list] on [id::object]
			privelage_list ::= [[insert, delete, update, select, reference, all]] --, [privelage_list] -- 


	revoke ::= revoke [revoke:role] from [user_list];
		role ::= [id::role] | [role:privelage_list] on [id::object]
			privelage_list ::= [privelage_list:privelage] --, [privelage_list] -- | all
				privelage ::= insert | delete | update | select | reference 


dml:stmt ::= [stmt:insert] |
		[stmt:select] |
		[stmt:delete] |
		[stmt:update] |
		[stmt:subquery] 

	insert ::= insert into -- [id::schema]. -- -- [id::table] | [id::view] -- -- ([insert:column_list]) -- -- values ([insert:expr_list]) | [subquery] --;
		column_list ::= [id::column] --, [column_list] -- 
		expr_list ::= [expr] --, [expr_list] --


	select ::= [subquery] -- [cl:order_by] | [cl:for_update] --;

	delete ::= delete from -- [id::schema]. -- -- [id::table] | [id::view] -- -- where [expr:condition] --;

	subquery ::=  ( -- intersect | intersect all | union | union all | minus -- [query:spec] )


dql:stmt ::= [dql:select]

	select ::=


tcl:stmt ::= [tcl:commit] | 
		[tcl:savepoint] | 
		[tcl:rollback] | 
		[tcl:set_transaction]

		::| commit and commit work are equivalent in meaning
	commit ::= commit -- work --;
	savepoint ::= savepoint [id::savepoint];
	rollback ::= rollback -- work | to [id:savepoint] --;
	set_transaction ::= set transaction isolation level [set_transaction:isolation_level];
		isolation_level ::= read committed | repeatable read | serializeable | singleuser

query = [query:spec]
	spec::= select -- [spec:hint] | [spec:hint] [distinct] | [distinct] | [spec:hint][all] | [all] -- -- [spec:select_subclause] --
		hint ::= [hint:hint_expr] // hint //;
            hint_expr ::= delete | select | update
		select_subclause ::=  -- * | [select_subclause:subexpr_0] | [select_subclause:subexpr_1] -- 
			subexpr_0 ::= -- [id::schema]. -- -- [id::table] | [id::view] --.* | [expr] -- as [id::alias] | [id::alias] -- | 
					[subexpr_0] -- , [subexpr_0] -- 
					
            subexpr_1 ::= from [subexpr_1:from_list] where [expr:condition] -- [cl:where] -- -- starts with [expr:condition] connected by [expr:condition] | group by [dml:stmt:insert:expr_list]-- -- having [expr:condition] --
 

cl:: = [cl:order_by] | 
		[cl:for_update] | 
		[cl:constraint] | 
		[cl:drop] | 
		[cl:factoring] | 
		[cl:where] | 
		[cl:join] | 
		[cl:group_by] | 
		[cl:model]

	order_by ::= order by -- [order_by:subclause] --
			subclause ::= [expr] | 
			[position] | 
			[expr] [position] | 
			[id::alias] | 
			asc | 
			desc --, [subclause] --  

	for_update ::= for update -- of | [for_update:subclause] --;
		subclause ::= -- [id::schema]. -- -- [id::table] | [id::view] | [id::column] -- --, [for_update] --

	constraint ::= [constraint:column] | [constraint:table]
		column ::= constraint [id::constraint] | 
				-- null | 
				not null | 
				unique | 
				primary key |
				[col:references_constraint] | 
				check([expr:condition]) --  

			references_constraint ::= references -- [id::schema]. -- [id::table] -- ([id::column]) | 
				on delete cascade | 
				([id::column]) on delete cascade --

		table ::=  constraint [id::constraint] |
				-- unique | primary -- ([table:column_list]) -- key columns = [number] -- |
				foreign key ([table:colmn_list]) references  -- [id::schema].  -- [id::table](table:column_list) on delete cascade | check ([table:column_list])
	
			column_list ::= [id::column] --, [id:column] -- 

	drop ::= drop -- primary key | column | column [id::column] | unique (drop:column_list) | constraint [id::constraint] -- -- cascade --; 
	where ::= where [expr:condition]

	<!-- *join ::= [id::table_reference] -- [join:inner] | [join:outer] --
		inner ::= -- inner -- join [id::table_reference] -- on [expr:condition] | using ([inner:column_list]) -- | -- cross | natural | natural inner -- join [id::table_reference]
			column_list ::= [id::column] --, [id::column] --
		outer ::=  -->


	group_by ::= group by [group_by:list] -- having [expr:condition] --
		list ::= [list:expr] | [list:rollup_cube] | [list:grouping_set]
			expr ::= [expr] --, [expr] -- 
			rollup_cube ::=  -- rollup | cube -- ([rollup_cube:expr_list])  
				expr_list ::=  [cl:group_by:list:expr] | 
						([cl:group_by:list:expr]) | 
						[cl:group_by:list:expr], ([cl:group_by:list:expr]) | 
						([cl:group_by:list:expr]), [cl:group_by:list:expr] 

		grouping_set ::= grouping sets ()

	model ::=  

```

Reference: https://docs.oracle.com/cd/B14156_01/doc/B13812/html/sqcmd.htm#i1007881