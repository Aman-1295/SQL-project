# SQL-project-link
https://skoolfi-my.sharepoint.com/:w:/p/manojyadav/EVEpyJiQQCNDv2BdUapgIPYB7PcWXvooYLfvVdNZC7e9Gg?e=MiaHTh

#Assignment

Consider a database for a Video Cd shop. The Database maintains the details collections, members, transactions .Create the following tables: Collection, Member, and Transaction with the appropriate constraints and populate the created tables with the data given below.


Table: Collection (CID, Title)

Column Name Data type and Data Size Description

CID Varchar2(5) Primary key and should start with CD

Title Varchar2(20) Must not be null


CID TITLE

CD001 TOM AND JERRY

CD002 HUMTUM

CD003 ROCKY

CD004 RAMBO

CD005 THE MYTH

Table: Member (MID, Member_Name, Address)

Column Name Data type and Data Size Description

MID Varchar2(5) Primary Key ,Should start with M

Member_Name Varchar2(15) Must not be null

Address Varchar2(25) Must not be null


MID MEMBER_Name ADDRESS

M001 S.K.Ahuja Hudson Nagar

M002 M.Shantiraman Bandra West

M003 K.L.Dsouza Bandra East

M004 James Dan Majestic

M005 Kim Jouang LBS

Table: Transaction (TId, CID,MID,Issue_date,Return_date)

Column Name Data type and Data Size Description

TId Number(6) Primary Key should be between 10000 to 20000

CID Varchar2(20) Foreign key

MID Varchar2(5) Foreign Key

Issue_date Date Must not be null

Return_date Date


TId CID MID Issue_date Return_date

10000 CD001 M001 21-JAN-2007 31-JAN -2007

10001 CD002 M004 22-JAN-2007 2-FEB -2007

10002 CD001 MOO2 1-FEB-2007 4-FEB-2007

10003 CD002 M001 3-FEB-2007 4-FEB-2007

10004 CD004 M003 19-FEB-2007 Null




Write queries to solve the following:

a. Display the Members details (mid, membername, address) who have not returned CD’s.

b. List the title names which are issued maximum number of times.

c. List all the titles and the member names for transactions with issue date on ’03-FEB-2007’


#Solutions

create	table	collection(	
cid varchar2(5)	check(cid like 'CD%'),
title varchar(20) not null,
primary key (cid));

insert	into collection	values('CD001','TOM_AND_JERRY');			
insert	into collection	values('CD002','HUMTUM');			
insert	into collection	values('CD003','ROCKY');		
insert	into collection	values('CD004','RAMBO');			
insert	into collection	values('CD005','THE_MYTH');	


create	table	member(		
mid varchar2(5)	check(mid like 'M%'),	
member_name varchar2(15)not null,	
address	varchar2(25) not null,
primary key(mid));

insert into member values('M001','S.K.Ahuja','Hudson Nagar');		
					
insert into member values('M002','M.Shantiraman','Bandra West');		
					
insert into member values('M003','K.L.Dsouza', 'Bandra	East');	
						
insert into member values('M004','James	Dan', 'Majestic');
						
insert into member values('M005','Kim	Jouang', 'LBS');


create table Transaction(			
TId number(6) check(TId between 10000 and 20000),	
cid varchar2(20),				
mid varchar2(5),				
issue_date date not null,		
return_date date,				
foreign	key (cid) references collection(cid),	
foreign	key (mid) references member(mid));	


insert	into Transaction values(10000,'CD001','M001','21-JAN-2007','31-JAN-2007');						
insert	into Transaction values(10001,'CD002','M004','22-JAN-2007','2-FEB-2007');					
insert	into Transaction values(10002,'CD001','M002','1-FEB-2007','4-FEB-2007');
insert	into Transaction values(10003,'CD002','M001','1-FEB-2007','4-FEB-2007');
insert	into Transaction values(10004,'CD004','M003','19-FEB-2007',Null);

update Transaction
set issue_date='3-FEB-2007'
where TId= 10003
    
select m.mid,m.member_name,m.address,t.return_date from member m,transaction t
where m.mid=t.mid and return_date is null

select * from(select t.cid,c.title,count(t.cid) as count from collection c,Transaction t
where c.cid=t.cid group by t.cid,c.title) order by count desc FETCH FIRST 1 ROWS ONLY;

select * from (select t.cid,c.title,count(t.cid) as count from collection c,Transaction t
where c.cid=t.cid group by t.cid,c.title order by count desc) where count=(select max(count) as count from
(select * from(select t.cid,c.title,count(t.cid) as count from collection c,Transaction t
where c.cid=t.cid group by t.cid,c.title) order by count desc))

select * from (select t.cid,c.title,count(t.cid) as count from collection c,Transaction t
where c.cid=t.cid group by t.cid,c.title order by count desc)where count=
(select max(count) as maximum from(select t.cid,c.title,count(t.cid) as count from collection c,Transaction t
where c.cid=t.cid group by t.cid,c.title order by count desc))-------same as above


select c.title,m.member_name,t.Issue_date from collection c,member m,transaction t
    where  t.cid=c.cid and t.mid=m.mid
          and ISSUE_DATE='3-FEB-2007'









