### 2020-10-27 高级数据库技术
```
DECLARE  
emp employee %rowtype; //一条元组作为对象?  
BEGIN  
EXCEPTION  
END;  
阻抗失配问题，定义游标；类似缓存，存储多条满足条件的元组。  

Declare  
  theld char(3);  
  CURSOR c IS; //?    
BEGIN  
  OPEN c;  
    ......  
    CLOSE c;  
END;  
```
## 实验
```
//建表title
create table Title_NIU
(title varchar(255),
year int,
length int,
inColor number(1),
studioName varchar(255),
producerC varchar(255) primary key
)
insert into Title_NIU values('Star Wars', 1977, 124, 1, 'Fox', 12345)
insert into Title_NIU values('Mighty Ducks', 1991, 104, 1, 'Disney', '67890')
insert into Title_NIU values('Pretty Woman', 1990, 119, 1, 'Disney', '999')
insert into Title_NIU values('Pretty Woman', 1992, 95, 1, 'Paramount', '99999')

//建表movieStar
create table MovieStar_NIU 
(name varchar(255) primary key,
address varchar(255),
gender char(1),
birthdate date
)
insert into MovieStar_NIU values('Mary Moore', 'Maple St', 'F', to_date('1950-07-03','yyyy-mm-dd'))
insert into MovieStar_NIU values('Tom Hanks', 'Cherry Ln', 'M', to_date('1970-05-03','yyyy-mm-dd'))

//建表movieExec
create table MovieExec_NIU
(name varchar(255),
address varchar(255),
Cert varchar(255) primary key,
netWorth varchar(255)
)
insert into MovieExec_NIU values('Mary Moore', 'Maple St', '12345', '100000')
insert into MovieExec_NIU values('George Lucas', 'Oak Rd', '23456', '200000')
```
### 判断是否是影星or制片人
影星 ！制片人   = 1
！影星 制片人   = 2
影星 制片人     = 3
！影星 ！制片人  = 4
```
set serveroutput on;
declare
star number;
exec number;
begin
select count(*) into star from MovieStar where
name='Mary Moore' and address='Maple st';
select count(*) into exec from MovieExec where
name='Mary Moore' and address='Maple st';
if (star=1 and exec=1) then
dbms_output.put_line('the balance =' || 3);
elsif (star!=1 and exec!=1) then
dbms_output.put_line('the balance =' || 4);
elsif (star!=1 and exec=1) then
dbms_output.put_line('the balance =' || 2);
else 
dbms_output.put_line('the balance =' || 1);
end if;
exception
when no_data_found then
dbms_output.put_line('the account doesn''t exist');
end ;

//create a procedure ERROR!!!
create procedure QueryId(name char, address char)
as
star number;
exec number;
begin
select count(*) into star from MovieStar where
name=name and address=address;
select count(*) into exec from MovieExec where
name=name and address=address;
if (star=1 and exec=1) then
dbms_output.put_line('the balance =' || 3);
elsif (star!=1 and exec!=1) then
dbms_output.put_line('the balance =' || 4);
elsif (star!=1 and exec=1) then
dbms_output.put_line('the balance =' || 2);
else 
dbms_output.put_line('the balance =' || 1);
end if;
exception
when no_data_found then
dbms_output.put_line('the account doesn''t exist');
end ;
```

### Q2:
