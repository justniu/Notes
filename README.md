# 目录
* [2020-10-27](2020-10-27)
* [2020-11-10](2020-11-10)


## 2020-11-10

根据主码创建的索引，称为主索引
索引第一个位置是索引字段，第二个位置是指向的`数据块`

代价
I/O代价
内存
CPU



## 2020-10-27 高级数据库技术
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
### Q1:判断是否是影星or制片人
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
//原来where后面作为条件的变量名不能和字段名相同，而且这里是不区分大小写的。但是作为update和insert into的参数确是可以的
// 更新 create or replace ...
create procedure queryId(fullname char, addr char) 
as
star number;
exec number;
begin
select count(*) into star from MovieStar where
name=fullname and address=addr;
select count(*) into exec from MovieExec where
name=fullname and address=addr;
if (star=1 and exec=1) then
dbms_output.put_line('the identifier is: ' || 3);
elsif (star!=1 and exec!=1) then
dbms_output.put_line('the identifier is: ' || 4);
elsif (star!=1 and exec=1) then
dbms_output.put_line('the identifier is: ' || 2);
else 
dbms_output.put_line('the identifier is: ' || 1);
end if;
exception
when no_data_found then
dbms_output.put_line('the record doesn''t exist');
end ;
```

### Q2:给定制片厂名称，给出它的两部最长的电影的标题。如果没有这样的电影，输出
`NULL`。  

```
declare
total number;
field char(35);
num number :=0;
CURSOR cur IS
select title from title where studioName='Disney' order by length(title) desc;
begin
select count(*) into total from title where studioName='Disney';
if (total <1 ) then
dbms_output.put_line('this studio no matchs!'); 
elsif (total =1 ) then
select title into field from title where studioName='Disney';
dbms_output.put_line('movie: ' || field);
else
OPEN cur;
LOOP 
FETCH cur into field;
EXIT WHEN (num = 2);
dbms_output.put_line('movie: ' || field);
num := num + 1;
END LOOP;
CLOSE cur;
end if;
exception
when no_data_found then
dbms_output.put_line('the account doesn''t exist');
end ;

//create a procedure
create procedure queryTitle(studio char)
as
total number;
field char(35);
num number :=0;
CURSOR cur IS
select title from title where studioName=studio order by length(title) desc;
begin
select count(*) into total from title where studioName=studio;
if (total <1 ) then
dbms_output.put_line('this studio no matchs!'); 
elsif (total =1 ) then
select title into field from title where studioName=studio;
dbms_output.put_line('movie: ' || field);
else
OPEN cur;
LOOP 
FETCH cur into field;
EXIT WHEN (num = 2);
dbms_output.put_line('movie: ' || field);
num := num + 1;
END LOOP;
CLOSE cur;
end if;
exception
when no_data_found then
dbms_output.put_line('the account doesn''t exist');
end ;
```
