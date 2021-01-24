# HIBERNATE INTERVIEW QUESTIONS

**Hibernate**
Hibernate is a ORM Framework, mainly used for Database Operations.

**ORM**
ORM is a programming technique which is used to map Application domain model objects to the relational database tables.It internally uses the Java API to interact with the databases.

**ORM FRAMEWORKS**
* HIBERNATE
* TOPLINK

**ADVANTAGES**
* Open Source
* LightWeight
* High Performance
* Generates Database Independent Queries
* Automatically creates tables
* Provides Query statistics and Database Status
* Caching
* Lazy Loading

**FUNCTIONALITIES**
* HQL(DB INDEPENDENT)
* Supports Auto DDL Operations
* Supports Auto Primary Key Generation Support
* Supports Cache Memory
* Exception handling is not mandatory.

**BENIFITS OF HIBERNATE OVER JDBC**
* Eliminates BoilerPlate code
* Supports Inheritance, Associations, Collections which are not present in JDBC
* Implicitly provides Transaction Management
* No need to write code for SQl Exceptions and Transaction with try-catch blocks.

**POJOs**
POJOs( Plain Old Java Objects) are java beans with proper getter and setter methods for each and every properties.

**hibernate configuration file**
Hibernate configuration file contains database specific configurations and used to initialize SessionFactory. We provide database credentials or JNDI resource information in the hibernate configuration xml file. Some other important parts of hibernate configuration file is Dialect information, so that hibernate knows the database type and mapping file or class details.


**What is hibernate mapping file**
Hibernate mapping file is used to define the entity bean fields and database table column mappings. We know that JPA annotations can be used for mapping but sometimes XML mapping file comes handy when we are using third party classes and we can’t use annotations.

**INTERFACES**

* Configuration : It is used to configure hibernate. It’s also used to bootstrap hibernate. Mapping documents of hibernate are located using this interface.

* SessionFactory : SessionFactory is the factory class that is used to get the Session objects. The SessionFactory is a heavyweight object so usually, it is created during application startup and kept for later use. This SessionFactory is a thread-safe object which is used by all the threads of an application. If you are using multiple databases then you would have to create multiple SessionFactory objects.

SessionFactory is immutable and shared by all Session. It also lives until the Hibernate is running. It also provides the second-level cache.

SessionFactory is both Immutable and thread-safe and it has just one single instance in the Hibernate application. It is used to create a Session object and it also provides caching by storing SQL queries stored by multiple sessions.

* Session : It is used to get a physical connection with the database. The Session object created is lightweight and designed to be instantiated each time an interaction is needed with the database. This Session provides methods to create, read, update and delete operations for a constant object. To get the Session, you can execute HQL queries, SQL native queries using the Session object.

A Session is a single-threaded, short-lived object. It provides the first-level cache.

the Session object is not thread-safe in Hibernate and intended to be used with-in a single thread in the application.

* Transaction

**How to complete a transaction in Hibernate**

There are two distinct actions if you’re looking to complete a transaction in hibernate:
• Commit
• Rollback

Once a transaction is committed, the transaction data is written to the database. In case there is a rollback, the data exchange is flushed and never written or updated to the database.


**Hibernate Configuration File**
Hibernate Configuration File mainly contains database-specific configurations and are used to initialize SessionFactory. Some important parts of the Hibernate Configuration File are Dialect information, so that hibernate knows the database type and mapping file or class details.



**CRITERIA API**

Criteria is a simple yet powerful API of hibernate which is used to retrieve entities through criteria object composition.

**hibernate generated SQL on console**
We need to add following in hibernate configuration file to enable viewing SQL on the console for debugging purposes:

<property name="show-sql">true</property>

**Hibernate Methods**
save() : saves a record only if it’s unique with respect to its primary key and will fail to insert if primary key already exists in the table.

saveOrUpdate() : inserts a new record if primary key is unique and will update an existing record if primary key exists in the table already.

get() : 
It will return null, if an object with an ID passed to them is not found.
It always goes to the database.
eager loading

load() : 
It will throw an exception, if an object with an ID passed to them is not found.
It can return proxy without hitting the database unless required.
lazy loading

merge()

update()



**ASSOCIATIONS**
* ONE TO ONE
* ONE TO MANY
* MANY TO ONE
* MANY TO MANY

**RELATIONSHIPS**
* UNI DIRECTIONAL
* BI DIRECTIONAL

**ANNOATATIONS**
* Entity
* Table
* Access
* Id
* EmbeddedId
* Column
* GeneratedValue
* GenericGenerator
* Parameter
* Temporal
* Cascade
* OneToOne
* OneToMany
* ManyToOne
* ManyToMany
* PrimaryKeyJoinColumn


**openSession() and getCurrentSession()**
getCurrentSession() : getCurrentSession() method returns the session bound to the context and for this to work, you need to configure it in Hibernate configuration file. Since this session object belongs to the context of Hibernate, it is okay if you don’t close it. Once the SessionFactory is closed, this session object gets closed.

openSession() : openSession() method helps in opening a new session. You should close this session object once you are done with all the database operations. And also, you should open a new session for each request in a multi-threaded environment.

**key components of a Hibernate configuration object**
The configuration provides 2 key components, namely:

Database Connection: This is handled by one or more configuration files.
Class Mapping setup: It helps in creating the connection between Java classes and database tables.

**Collections**
Hibernate provides the facility to persist the Collections.
There are five collection types in hibernate used for one-to-many relationship mappings.

Bag
Set
List
Array
Map

**Design Patterns Used in Hibernate**
There are a few design patterns used in Hibernate Framework, namely:

Domain Model Pattern: An object model of the domain that incorporates both behavior as well as data.
Data Mapper: A layer of the map that moves data between objects and a database while keeping it independent of each other and the map itself.
Proxy Pattern: It is used for lazy loading.
Factory Pattern: Used in SessionFactory.

**Dirty Checking in Hibernate**
Hibernate incorporates Dirty Checking feature that permits developers and users to avoid time-consuming write actions. This Dirty Checking feature changes or updates fields that need to be changed or updated, while keeping the remaining fields untouched and unchanged.

**How would you define automatic dirty checking**

Automatic dirty checking can be defined as a feature that helps us in saving the effort of explicitly asking Hibernate to update the database every time we modify or make changes to the state of an object inside a transaction.

**How can we reduce database write action times in Hibernate**
Hibernate provides dirty checking feature which can be used to reduce database write times. Dirty checking feature of hibernate updates only those fields which require a change while keeps others unchanged.

**What the four ORM levels are in hibernate**
Following are the four ORM levels in hibernate:
a. Pure Relational
b. Light Object Mapping
c. Medium Object Mapping
d. Full Object Mapping

**Hibernate QBC API**
Hibernate Query By Criteria (QBC) API is used to create queries by manipulation of criteria objects at runtime.

**In how many ways, objects can be fetched from database in hibernate**
Hibernate provides following four ways to fetch objects from database:
a. Using HQL
b. Using identifier
c. Using Criteria API
d. Using Standard SQL

**Light Object Mapping**

The means that the syntax is hidden from the business logic using specific design patterns. This is one of the valuable levels of ORM quality and this Light Object Mapping approach can be successful in case of applications where there are very fewer entities, or for applications having data models that are metadata-driven.

**Hibernate tuning**
Optimizing the performance of Hibernate applications is known as Hibernate tuning.

The performance tuning strategies for Hibernate are:

SQL Optimization
Session Management
Data Caching

**Transaction Management**

Transaction Management is a process of managing a set of commands or statements. In hibernate, Transaction Management is done by transaction interface. It maintains abstraction from the transaction implementation (JTA, JDBC). A transaction is associated with Session and is instantiated by calling session.beginTransaction().

**STATES OF PERSISTENT ENTITY**

Transient: 
New objects are created in the Java program and those are not associated with the Session and has no representation in the database.

Persistent: 
You can make a transient instance persistent by associating it with a Session.
An object which is associated with a Hibernate session is called Persistent object. While an object which was earlier associated with Hibernate session but currently it’s not associate is known as a detached object. You can call save() or persist() method to store those object into the database and bring them into the Persistent state.

Detached: 
If you close the Hibernate Session, the persistent instance will become a detached instance.
You can re-attach a detached object to Hibernate sessions by calling either update() or saveOrUpdate() method.

**How can we reattach any detached objects in Hibernate**

Objects which have been detached and are no longer associated with any persistent entities can be reattached by calling session.merge() method of session class.

**Hibernate Proxy and how it helps in Lazy loading**

Hibernate uses a proxy object in order to support Lazy loading.
When you try loading data from tables, Hibernate doesn’t load all the mapped objects.
After you reference a child object through getter methods, if the linked entity is not present in the session cache, then the proxy code will be entered to the database and load the linked object.
It uses Java assist to effectively and dynamically generate sub-classed implementations of your entity objects.

**CACHES**
* FIRST LEVEL : 
  Session Level cache



* SECOND LEVEL : 
SessionFactory Level cache
Shared by all Sessions


* QUERY : Hibernate implements a separate cache region for queries resultset that integrates with the Hibernate second-level cache. This is also an optional feature and requires a few more steps in code.
Note: This is only useful for queries that are run frequently with the same parameters.

**What are different ways to disable hibernate second level cache**

Hibernate second level cache can be disabled using any of the following ways:
a. By setting use_second_level_cache as false.
b. By using CACHEMODE.IGNORE
c. Using cache provider as org.hibernate.cache.NoCacheProvider

**What is ORM metadata**
All the mapping between classes and tables, properties and columns, Java types and SQL types etc is defined in ORM metadata.

**NATIVE SQL QUERY**
Hibernate provides an option to execute Native SQL queries through the use of the SQLQuery object. For normal scenarios, it is however not the recommended approach because you might lose other benefits like Association and Hibernate first-level caching.

Native SQL Query comes handy when you want to execute database-specific queries that are not supported by Hibernate API such query hints or the Connect keyword in Oracle Database.

**Named SQL Query**

Hibernate provides another important feature called Named Query using which you can define at a central location and use them anywhere in the code.

You can create named queries for both HQL as well as for Native SQL. These Named Queries can be defined in Hibernate mapping files with the help of JPA annotations @NamedQuery and @NamedNativeQuery.

**sorted and ordered collection in Hibernate**

sorted collection sort the data in JVM’s heap memory using Java’s collection framework sorting methods. The ordered collection is sorted using order by clause in the database itself.

Note: A sorted collection is more suited for small dataset but for a large dataset, it’s better to use ordered collection to avoid

**lazy loading**

Lazy loading is defined as a technique in which objects are loaded on an on-demand basis. It has been enabled by default since the advent of Hibernate 3 to ensure that child objects are not loaded when the parent is.

**managed associations and Hibernate associations**

Managed associations:  Relate to container management persistence and are bi-directional.

Hibernate Associations: These associations are unidirectional.

**ways to express joins in HQL**

HQL allows you to express joins in four ways:
• An implicit association join
• A fetch join in the FROM clause
• A theta-style join in the WHERE clause
• An ordinary join in the FROM clause

**different ways Hibernate manages concurrency**

Hibernate has numerous ways of managing concurrency. They are as listed below:
• Automatic versioning
• Detached object
• Extended user sessions


**What is the N+1 SELECT problem in Hibernate? (detailed answer)**
The N+1 SELECT problem is a result of lazy loading and load on demand fetching strategy. In this case, Hibernate ends up executing N+1 SQL queries to populate a collection of N elements.

For example, if you have a List of N Items where each Item has a dependency on a collection of Bid object. Now if you want to find the highest bid for each item then Hibernate will fire 1 query to load all items and N subsequent queries to load Bid for each item.

So in order to find the highest bid for each item your application ends up firing N+1 queries.  It's one of the important Hibernate interview questions and I suggest reading chapter 13 of Java Persistence with Hibernate to understand this problem in more detail.

**What are some strategies to solve the N+1 SELECT problem in Hibernate? (detailed answer)**
This is the follow-up question of the previous Hibernate interview question. If you answer the last query correctly then you would be most likely asked this one.

Here are some strategies to solve the N+1 problem:
1) pre-fetching in batches, will reduce the N+1 problem to N/K + 1 problem where  K is the size of the batch
2) subselect fetching strategy
3) disabling lazy loading

Read more: https://www.java67.com/2016/02/top-20-hibernate-interview-questions.html#ixzz6jgrhDZ00


**Which one is the default transaction factory in hibernate**
With hibernate 3.2, default transaction factory is JDBCTransactionFactory.

**What’s the role of JMX in hibernate**
Java Applications and components are managed in hibernate by a standard API called JMX API. JMX provides tools for development of efficient and robust distributed, web based solutions.

**How can we bind hibernate session factory to JNDI**
Hibernate session factory can be bound to JNDI by making configuration changes in hibernate.cfg file.

**In how many ways objects can be identified in Hibernate**
Object identification can be done in hibernate in following three ways:
a. Using Object Identity: Using == operator.
b. Using Object Equality: Using equals() method.
c. Using database identity: Relational database objects can be identified if they represent same row.

**What different fetching strategies are of hibernate**
Following fetching strategies are available in hibernate:
1. Join Fetching
2. Batch Fetching
3. Select Fetching
4. Sub-select Fetching

**What’s the use of version property in hibernate**
Version property is used in hibernate to know whether an object is in transient state or in detached state.

**What is attribute oriented programming**
In Attribute oriented programming, a developer can add Meta data (attributes) in the java source code to add more significance in the code. For Java (hibernate), attribute oriented programming is enabled by an engine called XDoclet.

**What’s the use of session.lock() in hibernate**
session.lock() method of session class is used to reattach an object which has been detached earlier. This method of reattaching doesn’t check for any data synchronization in database while reattaching the object and hence may lead to lack of synchronization in data.

**Does hibernate support polymorphism**
Yes, hibernate fully supports polymorphism. Polymorphism queries and polymorphism associations are supported in all mapping strategies of hibernate.

**What the three inheritance models are of hibernate**
Hibernate has following three inheritance models:
a. Tables Per Concrete Class
b. Table per class hierarchy
c. Table per sub-class

**How can we map the classes as immutable**
If we don’t want an application to update or delete objects of a class in hibernate, we can make the class as immutable by setting mutable=false

**What’s general hibernate flow using RDBMS**
General hibernate flow involving RDBMS is as follows:
a. Load configuration file and create object of configuration class.
b. Using configuration object, create sessionFactory object.
c. From sessionFactory, get one session.
d. Create HQL query.
e. Execute HQL query and get the results. Results will be in the form of a list.

**What is Light Object Mapping**
Light Object Mapping is one of the levels of ORM quality in which all entities are represented as classes and they are mapped manually.

**What’s difference between managed associations and hibernate associations**
Managed associations relate to container management persistence and are bi-directional while hibernate associations are unidirectional.




**best practices that Hibernate recommends for persistent classes**

All Java classes that will be persisted need a default constructor.
All classes should contain an ID in order to allow easy identification of your objects within Hibernate and the database. This property maps to the primary key column of a database table.
All attributes that will be persisted should be declared private and have getXXX and setXXX methods defined in the JavaBean style.
A central feature of Hibernate, proxies, depends upon the persistent class being either non-final, or the implementation of an interface that declares all public methods.
All classes that do not extend or implement some specialized classes and interfaces required by the EJB framework.

**best practices to follow with Hibernate framework**

Always check the primary key field access, if it’s generated at the database layer then you should not have a setter for this.
By default hibernate set the field values directly, without using setters. So if you want Hibernate to use setters, then make sure proper access is defined as @Access(value=AccessType.PROPERTY).
If access type is property, make sure annotations are used with getter methods and not setter methods. Avoid mixing of using annotations on both filed and getter methods.
Use native sql query only when it can’t be done using HQL, such as using the database-specific feature.
If you have to sort the collection, use ordered list rather than sorting it using Collection API.
Use named queries wisely, keep it at a single place for easy debugging. Use them for commonly used queries only. For entity-specific query, you can keep them in the entity bean itself.
For web applications, always try to use JNDI DataSource rather than configuring to create a connection in hibernate.
Avoid Many-to-Many relationships, it can be easily implemented using bidirectional One-to-Many and Many-to-One relationships.
For collections, try to use Lists, maps and sets. Avoid array because you don’t get benefit of lazy loading.
Do not treat exceptions as recoverable, roll back the Transaction and close the Session. If you do not do this, Hibernate cannot guarantee that the in-memory state accurately represents the persistent state.
Prefer DAO pattern for exposing the different methods that can be used with entity bean
Prefer lazy fetching for associations

