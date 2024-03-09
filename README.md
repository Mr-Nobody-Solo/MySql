
==================--------------- Install my SQL in Linux --------------========================
	$ sudo apt-get install mysql 
	or $ sudo apt-get install default-mysql
	* Active service : sudo systemctl status mysql.service
				sudo systemctl start/enable mysql.service
				sudo systemctl stop/disable mysql.service #for stop
#Change default option : sudo mysql_secure_installation
#Check details : sudo mysqladmin -u root -p version
#Now for test : mysql -u root -p  >> select 1+1;

------ Problems ----------
# If some how command is not execute after giving ';' : $ delimiter ;


--------------------------- DATABASE -----------------------------------------------------

## some common queries : 
	show databases;
	use (database_name); --to get inside database or active
	show tables;
	explain (here table name); --to see field name and type
	
##CREATING NEW DATABASE : 
	create database (name); 	
	drop database (name);  -- for delete 
### TO REMOVE SAFE UPDATE IN SQL : SET SQL_SAFE_UPDATES = 0;
## ADD SECURITY IN DBMS :
	alter database (name) read only = 1; -- for active read only, can't delete  
		alter database (name) read only = 0; -- undo read only 
 	alter database (name) encryption = 'y';   -- to add encryption 
 		alter database (name) encryption = 'n';  -- undo encryption


------------------------------ Table basic ---------------------------------------------

## NEW TABLE : create table (table name here) ( (column 1 name_here) (data_type), (column 2 name_here) (data_type) );
## Delete TABLE : drop table (here table name)
## SEE TABLE COLUMNS NAME : explain (here table name);
## SEE FULL TABLE : select * from (TABLE NAME HERE);
## RENAME TABLE : rename (table name) to (new table name);
## ADD COLUMN : alter table (table name)    add (column name) (datatype with size);
## REMOVE COLUMN : alter table (table name)    drop (column name);
## RENAME COLUMN : alter table (t name)    rename column (column name) to (now name);
## CHANGE COLUMN POSITION : 
	alter table (t name)    modify (column name) ( data type with size)
	after (column name which column will be left of it) ;     (or first;  -- for place it in 1st)
	
### VIEW | CREATE VIRTUAL TABLE :
	#CREATE :
	create view (here new table name)
	as
	select (here column 1 ,here column 2 from existing table)
	from (here table name which have the main record);
  	-- All sql query can be done by this table and its update automatically if something happened in main table.
  	# DELETE :
  	drop view (here view table name);
		
		
-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-playing with records / field  -.-.-.-.-.-.-.-.-.-.-.-.-.-.-.-.--.-.-.-.-.-

##ADD FULL ROW INTO TABLE : 
	insert into (table name here)
	values
	(---,---,---),
	(---, ---, ---);		--here must provide all column information.

##ADD SOME COLUMNS IN A ROW :
	insert into (here table name) ( col_1, col_2 )
	values
		(col_1 info, col_2 info),
		( .. , .. ),
		( .. , .. )
	;
	
## UPDATE ROWS (MODIFY)
 	# TO SET ALL ROWS WITH SAME VALUE !!! :
 		update (table name)
  		set (column name1 = VALUE , column name2 = VALUE);
  	
  	# TO SET SOME SPECIFIC ROWS :
  		update (table name)  
  		set (column name1 = VALUE , column name2 = VALUE)		--VALUE CAN BE NULL ALSO.
  		where (CONDITION );		-- identify by number type column name would be batter;
  
## DELETE ROWS  : 
	# DELETE ALL ROWS  RECORDS !!! : 
		delete from (table_name); 	-- Do not try to use it.
	# DELETE SPECIFIC ROWS :  
		delete from (table name) where (condition);
		

------------------------------ Playing with COLUMN -----------------------------------------------

#ADD CURRENT TIME AND DATE INTO ROW VALUES:
	insert into (here table name)
	value(current_date(),current_time(),now());		--here now() store both current date and time into DATETIME datatype;
	
## ADD UNIQUE or NOT NULL or PRIMARY KEY FIELD(COLUMN) :
	-- PRIMARY KEY -> UNIQUE + NOT NULL
	
	method 1: (if the table is not create yet)
	create table (here name) (
	col_name data_type UNIQUE ,	--this is a unique field.
	col_name data_type NOT NULL,	-- it will not accept any null value.
	);
	
	method 2: (for unique OR not null)
	alter table (here table name)
	modify (here column name) (here datatype) UNIQUE / NOT NULL ;
	
	method 3:
	alter table (table name)
	constraints unique(column name);	
	method 3:
	alter table (table name)
	constraint primary key(column name);
	
	----------Once Primary key added the add records syntax -------------
	insert into (here table name) ( (here field name),(here field name) )
	values ( .. , ..);
	
	
#ADD CHECK OR VALUE ENTRY CONDITION FOR A FIELD :
	method 1: ( if the table is not create yet)
	create table (here name) (
	col_1 int,	--this is field minimum value is added
	col_name data_type,
	constraint (here checker name such as min_val_of_col1) check(col_1 > 5)
	);
	
	method 2:
	alter table (table name)
	add constraint (here checker name such as min_val_of_col1) check(col_1 > 5);
	-- it will not work if non-satisfying already present in checker field.
	
	## FOR DELETE THE CHECK :
	alter table (table name) drop check (checker name); --not work with mariaDB
	
#ADD DEFAULT VALUE IN A FIELD:
	method 1: ( if the table is not create yet)
	create table (here name) (
	col_1 int default (here default value),	
	col_name data_type,
	);
	
	method 2:
	alter table (here table name) 
	alter (field name) set default (here default value);
	
#AUTO_INCREMENT INTO PRIMARY KEY: (It can be apply only primary key)
	method 1: (creating table)
	create table (here name) (
	column_name data_type primary key auto_increment;
	col_name d_type
	);
	# FOR SET STARTING VALUE : (The table field must come with auto_increment)
	alter table (here table name) auto_increment = (here start value);
	
## FOREIGN KEY in anther table PRIMARY KEY:
	method 1 : (creating a table)
	create table (here table name) (
	field_name data-type primary key auto_increment,
	field_name data-type,
	fk_field_name fk_data_type,
	constraint foreign key(fk_field_name) references (here referred table name)(fk_field_name)
	);
	-- stay alert about letters in word 
	
	method 2 :
	alter table (here table name)
	add constraint (here foreign key name prefer to unique name)
	foreign key(here fk_field_name that present in this table) references (here other table name)(fk_field_name);
	
	#DELETE FOREIGN KEY : 
	alter table (here table name)
	drop foreign key (here foreign key unique name);
	
	### ON DELETE SET NULL , ON DELETE CASCADE | DELETE RECORD IF IT IS CONNECT TO OTHER TABLE :
		alter table (here table name) 
		add constraint foreign key(here column name) references (here table name which primary key it is)( (here common column name) )
		(...);		-- here 'ON DELETE SET NULL' / 'ON DELETE CASCADE' ;
		
		-- If 'ON DELETE SET NULL' it will set NULL value to the connected foreign key after other table record delete.
		-- If 'ON DELETE CASCADE' it will delete THE RECODE that has connected through the foreign key while deleting other table record.
		
		-- Caution : Both option must be add while creating foreign key.😑️
		-- # delete foreign key : alter table (here table name) drop foreign key (here foreign key unique name);
		 
		
## INDEX | ADDING FIRST SEARCH OPTION ON A SPECIFIC COLUMN :
	-- it use B-Tree data structure for search.So select and where in query can run fast but slow for update.
	#CREATE :
	create index (here write new index name)
	on (here table name)( (here specific column name that used to search to much in BIG record.) );		-- on employees(first_name) ;
		
	
---------------------------- QUERY OR SEE TABLE RECORDS -----------------------------------------

## SEE ALL VALUES OF SPECIFIC COLUMN : 
	select col_name1,col_name2
 	from (table name);
 	
## SEE FULL ROW WITH SPECIFIC CONDITION (REAL QUERY) :
	 select * from (table name)
	 where (HERE CONDITION ); -- CONDITION EXAMPLE :
  		-- for number -->  >= , != , < , =  etc  
  		-- If column have number type than ->  (column name) >= (enter number)
  		-- If column have date type than ->  (column name) >= "yyyy-mm-dd"  
  		-- If column have varchar type than ->  (column name) = "....." //here specific name
  		
--- Special searching -------------------

# _ / % | SEARCH RECORDS WITH WILD-CARD : 
	_ is use to say that here can be any random value such as for year -> 20_ _ (between 2000-2099)
	# USING _ :	
		select * from (here table name)
		where (here field-name) like '_rrr';  	--here _ means string first letter can be any but 2nd,3rd,4th letter must be 'r';
		
	# USING % :
		select * from (here table name)
		where (here field-name) like 2%;	--here & means all records that has first number 2.
		-- FOR MATCH THE LAST NUMBER -> LIKE %2;
		
	-- wild-card & and _ both can be use at the same time such as :
		select * from (here table name)
		where (here field name) like "_a%"; 	-- it means string 1st character is random but second character must be 'a' after that any character can have.

----------------------------------------------

## ORDER BY ASC/DESC | SEE RECODES ASCENDING (ASC) OR DESCENDING (DESC) ORDER : 
	#FOR ONE FIELD :
		select * from (here table name)
		order by (here field-name) (...) ;	--here in (...) asc for ascending and desc for descending sort records
	
	#FOR TWO FIELD :
		select * from (here table name)
		order by (here first-field-name) asc , (here second-field-name) desc; 	--it use when one field have multiple same values.
		
## LIMIT CLAUSE  | TO SET HOW MANY RECORD HAVE TO SHOW IN A PAGE :
	#START FROM BEGINNING : 
		select * from (here table name)
		limit 10;				-- here first 10 record will shown.
	#START FROM SOME RECORDS LATTER :
		select * from (here table name)
		limit 5,10; 				-- it will help to show 10 records after escaping first 5 records.
		
	#IT CAN BE USED WITH ORDER BY :
		select * from (here table name) 
		order by (column name) desc 
		limit 3,5;
		
##  RIGHT-INNER-LEFT JOIN | SHOW TWO JOINED TABLE : (INNER,LEFT,RIGHT JOIN)
	-- here (...) stand for inner or right or left
	
	select * from (here left table name ) (...) join (here right table name)
	on  (here left_table name).(here fk_field name) = (here right_table name).(here pk_field name) ;
	
	-- fk = foreign key and pk = primary key
	-- inner show both connect rows
	-- left show all left table rows and connected right table rows
	-- right show all right table rows and connected left table rows
	
## JOIN | SELF JOIN ( APPLY JOIN TO THE SAME TABLE IF THERE ARE SOME COMMON HAVE ONE COLUMN TO OTHER COLUMN):
	select 
	* 	-- here can be write instant of "*" -> "A.customer_id,A.name, concat(B.name,'. ID = ',B.customer_id) as 'referred by' "
	from 	
	(here table name) as (here provide a letter) 	-- such as 'customers as A'
	inner join 
	(here same table name) as (here different one)	-- such as 'customers as B'
	on 
	(here first letter).(here relational column name) = (here second letter).(here main column name that the relation made of) ;
	-- A.referral_id = B.customer_id;
	
	
## UNION | SEE TWO TABLE RECORDS AT THE SAME TIME :
	#IF BOTH TABLE COLUMN SIZE IS EQUAL :
		select * from (here table 1 name)
		union 					-- only the use of union will show one of the same multiple records.
		select * from (here table 2 name);
		
	#IF COLUMN SIZE ARE DIFFERENT : 
		select (here column 1 , here column 2 ) from (here table 1 name)
		union all 								-- here 'union all' will include duplicate value present in both table. 
		select (here column 1 , here column 2 ) from (here table 2 name);
		
## GROUP BY | SEE TABLE RECORD ACCORDING TO GROUP :
	# IF DATE ARE SAME :
		select * from (here table name) 
		group by (here column name that have the same date);
	
	# AGGREGATE FUNCTION | SUM(), MAX() :
		select *, sum(here column name which have the value) 		-- Such as 'sum(amount)'
		from (here table name)
		group by ( here column name that came multiple time in records)		-- Such as 'group by(id)'
		having (here would condition if need to filter) ;	-- Such as 'amount is NOT NULL';
		-- If group by is used then need to use 'having' instant of 'where' for filter query.  
		
	-- Example : select sum(amount),order_date from transactions group by order_date;	
			-- here records are group by according to each date transactions.
	
	## GROUP BY WITH ROLLUP | ADD ANOTHER ROW TO SHOW THE SUM OF GROUP BY BY VALUES :
		select sum( (here column name) ) , (here column name that are going to group by)
		from (here table)
		group by with ROLLUP;

--- FUNCTIONS ------------------------------------------

# USE OF SUM(),AVG(), COUNT() , CONCAT(), MAX(), MIN() IN MYSQL:
	select sum( (here column name) ) as (here give a sum column name)
	from (here table name);
	
--- Logical operator -----------------------------------

# USE .. AND ..  ,.. OR .. , BETWEEN .. AND .., IN .. , NOT IN .. , NOT :
	#FOR NOT : 
		select * from (here table name)
		where NOT (condition );		-- here NOT will help not to show satisfying condition. 	
	#FOR AND , OR :
		select * from (here table name)
		where (condition 1) (...) (condition 2);	//here (...) AND , OR
	#FOR BETWEEN :
		select * from (here table name)
		where (here checker field name) between (here left range) and  (here right range) ;
	#FOR IN : 
		select * from (here table name)
		where (here specific field name) 
		in (here write matching number or word present in field records , .. , .. ) ;	--Every searching item must be in same field;

## Sub-query ----------------
	-- Most needed thing but little complex in MySql ( ^ - ^ )
	-- sub-query is a query that inside in '( ... )'
	-- It used in after select or after where.
	-- Example 1 : select name (select avg(income) as income from employees) from employees;
	-- Example 2 : select * from customers where customer_id not in (select customer_id from regular_customers);
				-- here customer_id is foreign key in regular_customer.
	-- if some record come multiple it is good to use DISTINCT
	
## Store queries that use multiple time 
	
	# WITHOUT PARAMETER : 
		delimiter $$
		create procedure (here function name and () )  	-- Example : get_employees() 
		begin 
			(here write queries) 		-- Example : select * from employees;
		end $$
		
	# WITH PARAMETER :
		delimiter $$
		create procedure (here function name ( in (here name) (here data_type) ) 	-- Example : get_employee(in id int) 
		begin 
			(here write queries) 		-- Example : select * from employees where employee_id = id;
		end $$	
	
	# DELETE PROCEDURE : drop procedure (here procedure name only not need '()' ) 		-- Example : drop procedure get_employees;
	
## Trigger helps to do something while INSERT,DELETE or UPDATE record
	
	# Apply in same table : 
		create trigger (here understandable trigger name)	
		before (...) on (here table name)	-- (...) -> delete or update or insert
		for each row
		set (...).(here column name) = (here value that need to insert or update) ;
		
		Example : 
		create trigger before_salary_update
		before update on employees
		for each row
		set new.salary = new.hourly_pay*2080;
		-- It change salary automatically according to hourly_pay column record value changes.
		
		
	# Apply in other table :
		create trigger (here understandable trigger name)
		after (...) on (here table name)	-- (...) -> delete or update or insert
		for each row
		update (here other table name that record gonna change)
		set (here column name) = (here previous table column name with value that gonna change)
		where (here condition to identify the record) ;
		
		Example :
		create trigger after_update_employee_salary
		after update on employees
		for each row 
		update expenses
		set expenses_total  = expenses_total + (NEW.salary-OLD.salary)
		where expenses_name = 'salaries';
		-- It change the amount of 'expenses_total' automatically while updating salary in employees table.
		
		
		
