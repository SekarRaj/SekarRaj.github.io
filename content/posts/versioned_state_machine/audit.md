

Auditing

Auditing is a requiusite ion many complicane critical application such as healthcare and finance. Information update and access has to be audited. In this example we will implement the auditing based on Javers. 

Temporal tables 
Temporal tables is one of the ways we can create audit by leveraging the tools provided by the DBMS. The database systems provide SQL specific tooling and support to enbale this feature. 

1. Create tables with Sytem versioning 
2. Upserting records 
3. Deleting records
   Deleting records inversioned tables is a bit tricking. You have to turn off system version to delete a record. After deletion the versioning can be turned on again 


If you are familiar with Hibernate Envers, then Javers offers functionality similar to it. However, Envers is primarly for relational databases and Javers support Relational and NoSQL databases. Since the audit state is saved in JSON format, document database such as Mongo will be a good choice for Javers.

In this example, we are using SQL variant of Javers. Though Javers creates the required tables and index via hibrate ORM, I prefer creating and managing entities intead of auto-creation. For illustration of the example, I have created a schema `wellness` in MS SQL server and another schema named `wellness_audit` for the auditing functionallity. 

Tangent
In this application we are using a Hexagonal based architecure desgin with Spring JPA for database access. 

In this application the Patient is the main domain aggregate, screening and medications revolve around the patient. Auditing is enabled on the JPA reposiotyr layer using the AOP annotation `` provided by Javers. 


Concepts
The auditing happens at a Entity level. 

1. Creating a new patient, 
   