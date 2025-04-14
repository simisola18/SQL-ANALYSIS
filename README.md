# SQL-ANALYSIS
Star schema design, Derivation of logical relations, Creation of tables with integrity rules, Data source identification, data extraction, transformation and loading, Justification of the data mart design and comparison of data mart and OLTP
# Deliverables:   
# 1.	Star schema design
1.1	Star schema
Please draw a star schema here
1.2	Explanation on your star schema 
a.	What are the fact and dimensions and why they are chosen Ans: Dimensions;
P2686827_DmTheatre for Theatre
P2686827_DmProduction for Production
P2686827_DmClient for Client
P2686827_DmTime for Time
     Fact:
P2686827_FactTable for Fact
b.	Why their attributes are chosen ANS: Based on the business analysis requirements, the attributes below from the star schema above are those needed in answering the 3 business questions given.
c.	What is the granularity   ANS: The granularity which is the resolution of our data for each row in the fact table, it represents the total sale amount of one client for one theatre for one production in a month. The grains are Each client, Each theatre, Each production and Each month.
<img width="125" alt="image" src="https://github.com/user-attachments/assets/75110f48-2e43-46bd-aba6-16c2b8536d95" />

# 2.	Derivation of logical relations
2.1	A list of logical relations: 
ANS:  P2686827_DmTheatre (Theatre_id (PK), Theatre#, Name);
P2686827_DmProduction (Prod_id (Pk), P#, Title);
P2686827_DmClient (Client_id (PK), Client#, Title, Name);
P2686827_DmTime (Time_id (PK), Month, Year);
P2686827_FactTable (Theatre_id (FK), Prod_id (FK), Time_id(FK), Client_id  (FK), TotalSale)
Theatre_id is a FK referring to P2686827_DmTheatre, Client_id is a FK referring to P2686827_DmClient, Prod_id is a FK referring to P2686827_DmProduction, and Time_id is a FK referring to P2686827_DmTime.

2.2	Explanation on the mapping from your star schema to the logical relations 
a.	How is the logical relations mapped from your star schema (which relations correspond to which entity in your star schema)
ANS: The Dimensions only contain attributes that are required and not all the attributes from the Tables in the OLTP Database. However, the Fact table contains only the fact i.e total amount and the associated primary keys from the other Dimension table as foreign keys to the associated table.
b.	What are the Primary Keys (PK) and Foreign Keys (FK) in the fact and dimensions.  For each FK, what does it refer to? 
ANS: For star schema, please see the figure above.  Theatre#,P#,Client# are kept as nature keys for dimensions. Theatre_id, Prod_id, Client_id and Time_id are surrogate keys for dimensions.  For the dimension P2686827_DmTheatre, the PK is Theatre_id. For the dimension P2686827_DmProduction, the PK is Prod_id. For the dimension P2686827_DmClient, the PK is Client_id. For the dimension P2686827_DmTime, the PK is Time_id. 
For the fact table P2686827_FactTable, the PK is a composite key consisting of Theatre_id, Prod_id, Client_id and Time_id.  Theatre_id is a FK referring to P2686827_DmTheatre, Client_id is a FK referring to P2686827_DmClient, Prod_id is a FK referring to P2686827_DmProduction, and Time_id is a FK referring to P2686827_DmTime.


# 3.	Creation of tables with integrity rules
For each table:
3.1	SQL code for each table creation
3.2	Evidence of the code working and table created 
A 4-dimension table namely P2686827_DmTheatre, P2686827_DmProduction, P2686827_DmClient, and P2686827_DmTime will be created with each having its own primary key. Nature keys are not used in creating a data mart but rather surrogate keys. This is done to keep the source data untouched and also avert the potential for repetition. Therefore, after creating each table we create a sequence in oracle that will add a unique value for each row in our table. The SQL command and a screenshot of Oracle's result are below:
select * from ops$yyang00.theatre;
select * from ops$yyang00.production;
select * from ops$yyang00.performance;
select * from ops$yyang00.client;
select * from ops$yyang00.ticketpurchase;

desc ops$yyang00.theatre;
desc ops$yyang00.production;
desc ops$yyang00.performance;
desc ops$yyang00.client;
desc ops$yyang00.ticketpurchase;

1. create table P2686827_DmTheatre(Theatre_id number(5) primary key, Theatre# number(6) not null, Name varchar2(25) not null);
CREATE SEQUENCE  P2686827_DmTheatre_seq
 START WITH     1
 INCREMENT BY   1
 NOCACHE
 NOCYCLE; 
![image](https://github.com/user-attachments/assets/c6da2085-d4c4-4924-92e4-3e92c989d17b)
![image](https://github.com/user-attachments/assets/cf13bf9f-7ced-4462-8073-78ee2d90118f)

create table P2686827_DmProduction(Prod_id number (5) primary key, P# number (5) not null, Title varchar2 (25) not null);
CREATE SEQUENCE  P2686827_DmProduction_seq
 START WITH     1
 INCREMENT BY   1
 NOCACHE
 NOCYCLE;
![image](https://github.com/user-attachments/assets/ade2e987-88dd-4c8c-8a38-d3103bb39cfa)
![image](https://github.com/user-attachments/assets/e44cbfe5-4696-4f0d-81d9-2f3070a92fcc)
3. create table P2686827_DmClient(Client_id number (5) primary key, Client# number (5) not null, Title varchar2 (25), Name varchar2(35) not null);
CREATE SEQUENCE  P2686827_DmClient_seq
START WITH     1
INCREMENT BY   1
NOCACHE
NOCYCLE;
![image](https://github.com/user-attachments/assets/718df623-489d-40ab-9e87-e1425861cfa8)
![image](https://github.com/user-attachments/assets/11550a3a-8336-41ed-a836-688b45c59778)
4. create table P2686827_DmTime(Time_id number (5) primary key, Year number (4) not null, Month number (2) not null);
CREATE SEQUENCE  P2686827_DmTime_seq
 START WITH     1
 INCREMENT BY   1
 NOCACHE
 NOCYCLE;
![image](https://github.com/user-attachments/assets/45783f75-47e7-41e7-8928-40814a8b95e8)
![image](https://github.com/user-attachments/assets/3365357f-1707-4e72-a6c1-6a79724921b5)
5. create table P2686827_FactTable(
  Theatre_id number(5) CONSTRAINT fk10 REFERENCES P2686827_DmTheatre,
   Prod_id number(5) CONSTRAINT fk11 REFERENCES P2686827_DmProduction,
   Client_id number(5) CONSTRAINT fk12 REFERENCES P2686827_DmClient,
   Time_id  number(5) CONSTRAINT fk13 REFERENCES P2686827_DmTime,
   TotalSale number(25,2) not null,
   CONSTRAINT pk14 primary key (Theatre_id,Prod_id,Client_id,Time_id));
![image](https://github.com/user-attachments/assets/d5a9da85-855e-4ad7-80b1-c82e054b8978)
![image](https://github.com/user-attachments/assets/c7e2748e-dc41-427e-99ec-236b4c67bc76)

# 4.	Data source identification, data extraction, transformation and loading
4.1	Data source mapping
Please draw the map between data source and their destination in your Data Mart
For each ETL operation:
Data source mapping aids in locating the sources of important attributes. When a data analyst must perform ELT, it is highly beneficial as it informs the user of the source of the data and what needs to be prepared. We refer to this as transformation. We can combine the data for analysis thanks to this technique.
![image](https://github.com/user-attachments/assets/757c7f39-4879-4c4c-bbcb-6070c9a6ee0b)
![image](https://github.com/user-attachments/assets/ac9a5740-9e35-4ab4-b2ca-123bb4f618d9)
![image](https://github.com/user-attachments/assets/c5538503-c69e-41b7-a632-93c8c11ef027)

4.2	ETL code
4.3	Evidence of ETL code works
Implementation of ETL for P2686827_DmTheatre:
insert into P2686827_DmTheatre select P2686827_DmTheatre_seq.nextval, Theatre#, Name from 
 (select distinct Theatre#, upper(trim(Name)) Name
 from
 (select thtr.Theatre#, thtr.Name
 from ops$yyang00.theatre thtr, ops$yyang00.performance perf, ops$yyang00.ticketpurchase tpur where thtr.Theatre#=perf.Theatre# and perf.Per#=tpur.Per#));
![image](https://github.com/user-attachments/assets/9c012260-04c8-43b8-a024-f876e5a9ca84)
Implementation of ETL for P2686827_DmProduction:
insert into P2686827_DmProduction select P2686827_DmProduction_seq.nextval, P#, Title from 
 (select distinct P#, upper(trim(Title)) Title
 from
 (select prod.P#, prod.Title
 from ops$yyang00.production prod, ops$yyang00.performance perf, ops$yyang00.ticketpurchase tpur where prod.P#=perf.P# and perf.Per#=tpur.Per#));
![image](https://github.com/user-attachments/assets/179b7403-6568-45f5-b8e4-a97d7c8c1db0)
Implementation of ETL for P2686827_DmClient:
insert into P2686827_DmClient select P2686827_DmClient_seq.nextval, Client#, Title, Name from 
 (select distinct Client#, upper(trim(Title)) Title, upper(trim(Name)) Name
 from
 (select cln.Client#, cln.Title, cln.Name
 from ops$yyang00.client cln, ops$yyang00.ticketpurchase tpur where cln.Client#=tpur.Client#));
![image](https://github.com/user-attachments/assets/71d4eb39-c945-4f96-afb4-a996f5d228d1)
Implementation of ETL for P2686827_DmTime:
insert into P2686827_DmTime select P2686827_DmTime_seq.nextval, Year, Month from 
 (select distinct extract(Year from Pdate) Year, extract(Month from Pdate) Month
 from ops$yyang00.performance);
![image](https://github.com/user-attachments/assets/23796a4b-6e37-4ee3-bad8-cb3358efbbb5)
Implementation of ETL for P2686827_FactTable:
insert into P2686827_FactTable select Theatre_id, Prod_id, Client_id, Time_id, TotalSale 
 from
 (select pdmth.Theatre_id, pdmpd.Prod_id, pdmcln.Client_id, pdmt.Time_id, sum(TotalAmount) TotalSale
 from 
 ops$yyang00.performance perf, ops$yyang00.ticketpurchase tpur, 
 P2686827_DmTheatre pdmth, P2686827_DmProduction pdmpd, P2686827_DmClient pdmcln, P2686827_DmTime pdmt
where
 pdmth.Theatre#=perf.Theatre# and perf.Per#=tpur.Per# and pdmpd.P#=perf.P# and pdmcln.Client#=tpur.Client#
 and
 extract(Year from perf.Pdate)=pdmt.Year and extract(Month from perf.Pdate)=pdmt.Month
 group by pdmth.Theatre_id, pdmpd.Prod_id, pdmcln.Client_id, pdmt.Time_id);
![image](https://github.com/user-attachments/assets/f0b0a124-30cf-440e-af39-8f1897c6df8e)

# 5.	Justification of the data mart design and comparison of data mart and OLTP
For each required analysis query
5.1	SQL code for the required queries (both Data Mart and the relational model)
5.2	Evidence of the SQL code working and their results
For the comparison 
5.3	Please compare your two sets of queries and comment on how they satisfy the requirements of the data analysis and the comparison between the Data Mart and the relational model. 
ANS Having established the online transaction processing database, Ms. Heritage wants more intelligence information from the available data and she is looking for a potential data warehouse for MT. The queries to be satisfied for Ms. Heritage requirements are as follows
•	The total sale value of each production. 
/Relational model for analysing the Total sale value of each production/
select prod.P#, prod.title, sum(TotalAmount)
from ops$yyang00.ticketPurchase tpur, ops$yyang00.production prod, ops$yyang00.performance perf
where prod.P#=perf.p# and perf.per#=tpur.per#
group by prod.P#, prod.title
order by prod.title asc;
![image](https://github.com/user-attachments/assets/f1d7600b-bbcd-476b-b55b-db77fa35b68b)
![image](https://github.com/user-attachments/assets/3f286afb-e3c1-417e-a912-a84cbeade03c)
/Data Mart for analysing the Total sale value of each production /
select pdmpd.Prod_id, pdmpd.Title, sum(TotalSale)
from  P2686827_DmProduction pdmpd,  P2686827_FactTable pfth
where pdmpd.Prod_id=pfth.Prod_id
group by  pdmpd.Prod_id, pdmpd.Title
order by pdmpd.Title asc;
![image](https://github.com/user-attachments/assets/864034d7-2573-4b6f-bbb7-1b5fdd45a8ea)
![image](https://github.com/user-attachments/assets/ce322240-b033-4508-9106-286ad8ec3376)
For the comparison 
5.3 Please compare your two sets of queries and comment on how they satisfy the requirements of the data analysis and the comparison between the Data Mart and the relational model (no more than one page). 
•	Monthly sale value of each theatre.
/Relational model for analysing the Monthly sale value of each theatre/
select thtr.Theatre#, thtr.Name,extract(Year from Pdate),extract(Month from Pdate), sum(totalamount)
from ops$yyang00.ticketPurchase tpur, ops$yyang00.performance perf, ops$yyang00.theatre thtr
where thtr.theatre#=perf.theatre# and perf.per#=tpur.per#
group by thtr.Theatre#, thtr.Name, extract(Year from Pdate),extract(Month from Pdate)
order by thtr.Name asc;
![image](https://github.com/user-attachments/assets/075822b6-463a-4b54-a96b-139906566fe2)
![image](https://github.com/user-attachments/assets/1cd0ebe3-1914-44dc-a980-1c8f4347130c)

/Data Mart for analysing the Monthly sale value of each theatre/
select pdmth.Theatre_id, pdmth.Name, pdmt.Year, pdmt.Month, sum(TotalSale)
from  P2686827_DmTime pdmt,  P2686827_FactTable pfth, P2686827_DmTheatre pdmth
where pdmth.Theatre_id=pfth.Theatre_id and  pdmt.Time_id=pfth.Time_id
group by pdmth.Theatre_id, pdmth.Name, pdmt.Year, pdmt.Month
order by pdmth.Name asc;
![image](https://github.com/user-attachments/assets/78137a88-9bfd-47c2-bc61-d1b0cd505f2d)
![image](https://github.com/user-attachments/assets/4e523183-3111-4d5b-b819-2fe2568eabb8)
5.3	Please compare your two sets of queries and comment on how they satisfy the requirements of the data analysis and the comparison between the Data Mart and the relational model.
The Managing Director of MT, Ms. Heritage, developed a computerized booking system for MT to get more intelligent information from the available data to help manage the business properly, she aims to analyze The total sale value of each production, the Monthly sale value of each theatre and The theatre name and the names of clients who have the highest spending in that theatre to set up a data mart for ticket sales as the first step in her data warehouse for MT. The granularity was achieved and the data mart was able to examine all the queries. With this MT will be able to make more informed decisions on its target market, plan better for production and flows into its theatres, know the revenue being generated and maximize profit generation.
The advantages of a data mart in analysis operations is that it helps accelerate the business processes, has a lower cost, and is easier implementation & maintenance. It also provides faster insights and decisions for the Midland Theatre.

