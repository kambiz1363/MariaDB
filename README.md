# MariaDB
## What is MariaDB
MariaDB is a relational database management system. It stores data in various tables. Primary keys and foreign keys are used to establish relationship between these tables.
MarinaDB provides the following features:
* RDBMS(Relational Database Management System) facilitates you to implement a data source with tables, columns, and indices.
* RDBMS provides integrity of references across rows of multiple tables.
* It is used to automatically update indices.
* It is used to interpret SQL queries and operations in manipulating or sourcing data from tables.
### Terms used in RDBMS
Following is a list of terms used in relational database management systems i.e. MariaDB :

**Database**: A database is a data store which contains table and holds related data.

**Table**: A table is a matrix like structure which contains data.

**Column**: A column is s data element. It is a structure which holds same type of data. For example name.

**Row**: A row is a structure which stores related data. For example: data for a customer. It is also known as a tuple, entry, or record.

**Redundancy**: The term redundancy specifies how to store data twice in order to accelerate the system.

**Primary Key**: Primary key is a unique identifying value. This value cannot appear twice within a table, and there is only one row associated with it.

**Foreign Key**: A foreign key is used as a a link between two tables.

**Compound Key**: A compound key, or composite key, is a key that refers to multiple columns. It refers to multiple columns due to a column lacking a unique quality.

**Index**: An index is virtually identical to the index of a book.

**Referential Integrity**: This term refers to ensuring all foreign key values point to existing rows.

 ### Features of MariaDB

MariaDB provides the same features of MySQL with some extensions. It is relatively new and advance.
Following is a list of features of MariaDB:

* MariaDB is licenced under GPL, LGPL, or BSD.
* MariaDB includes a wide selection of storage engines, including high-performance storage engines, for working with other RDBMS data sources.
* MariaDB uses a standard and popular querying language.
* MariaDB runs on a number of operating systems and supports a wide variety of programming languages.
* MariaDB offers support for PHP, one of the most popular web development languages.
* MariaDB offers [Galera](https://github.com/kambiz1363/MariaDB/blob/master/README.md#mariadb-closter) cluster technology.
* MariaDB also offers many operations and commands unavailable in MySQL, and eliminates/replaces features impacting performance negatively.
## Introducing MariaDB data Types
To store persistent information in a database, we will use **tables** that store rows of data. Often, two or more tables will be related to each other in some way. That is part of the organization that characterizes the use of relational databases.
tables are database objects where we will keep persistent information. Each table consists of two or more fields (also known as **columns**) of a given data type (the type of information) that such field can store.
The most common data types in MariaDB are the following (you can consult the complete list in the [official MariaDB online documentation](https://mariadb.com/kb/en/library/data-types/)):
#### Numeric:
* BOOLEAN considers 0 as false and any other values as true.
* TINYINT, if used with SIGNED, covers the range from -128 to 127, whereas the UNSIGNED range is 0 to 255.
* SMALLINT, if used with SIGNED, covers the range from -32768 to 32767. The UNSIGNED range is 0 to 65535.
* INT, if used with UNSIGNED, covers the range from 0 to 4294967295, and -2147483648 to 2147483647 otherwise.
* DOUBLE(M, D), where M is the total number of digits and D is the number of digits after the decimal point, represents a double-precision floating-point number. If UNSIGNED is specified, negative values are not be allowed.
#### String:
* VARCHAR(M) represents a string of variable length where M is the maximum allowed column length in bytes (65,535 in theory). In most cases, the number of bytes is identical to the number of characters, except for some characters that can take up as much as 3 bytes. For example, the Spanish letter ñ represents one character but takes up 2 bytes.
* TEXT(M) represents a column with a maximum length of 65,535 characters. However, as it happens with VARCHAR(M), the actual maximum length is reduced if multi-byte characters are stored. If M is specified, the column is created as the smallest type that can store such number of characters.
* MEDIUMTEXT(M) and LONGTEXT(M) are similar to TEXT(M), only that the maximum allowed lengths are 16,777,215 and 4,294,967,295 characters, respectively.
#### Date and Time:
* DATE represents the date in YYYY-MM-DD format.
* TIME represents the time in HH:MM:SS.sss format (hour, minutes, seconds, and milliseconds).
* DATETIME is the combination of DATE and TIME in YYYY-MM-DD HH:MM:SS format.
* TIMESTAMP is used to define the moment a row was added or updated.
After having reviewed these data types, you will be in a better position to determine which data type you need to assign to a given column in a table.
## EXAMPLE 
### Creating a New Database
```
MariaDB [(none)]> CREATE DATABASE BookstoreDB;
Query OK, 1 row affected (0.714 sec)
```
### Creating Tables with Primary and Foreign Keys
Before we dive into creating tables, there are two fundamental concepts about relational databases that we need to review: primary and foreign keys.
A primary key contains a value that uniquely identifies each row, or record, in the table. On the other hand, a foreign key is used to create a link between the data in two tables, and to control the data that can be stored in the table where the foreign key is located. Both primary and foreign keys are generally INTs.

To illustrate, let’s use the ```BookstoreDB``` and create two tables named ```AuthorsTBL``` and ```BooksTBL``` as follows. The NOT NULL constraint indicates that the associated field requires a value other than NULL.

Also, AUTO_INCREMENT is used to increase by one the value of INT primary key columns when a new record is inserted into the table.
```
MariaDB [(none)]> USE BookstoreDB;
Database changed
MariaDB [BookstoreDB]> CREATE TABLE AuthorsTBL (
    -> AuthorID INT NOT NULL AUTO_INCREMENT,
    -> AuthorName VARCHAR(100),
    -> PRIMARY KEY(AuthorID)
    -> );
Query OK, 0 rows affected (2.840 sec)

MariaDB [BookstoreDB]> CREATE TABLE BooksTBL (
    -> BookID INT NOT NULL AUTO_INCREMENT,
    -> BookName VARCHAR(100) NOT NULL,
    -> AuthorID INT NOT NULL,
    -> BookPrice DECIMAL(6,2) NOT NULL,
    -> BookLastUpdated TIMESTAMP,
    -> BookIsAvailable BOOLEAN,
    -> PRIMARY KEY(BookID),
    -> FOREIGN KEY (AuthorID) REFERENCES AuthorsTBL(AuthorID)
    -> );
Query OK, 0 rows affected (4.914 sec)

```
### MariaDB Closter
Clusters come in two general configurations, active-passive and active-active. In active-passive clusters, all writes are done on a single active server and then copied to one or more passive servers that are poised to take over only in the event of an active server failure. Some active-passive clusters also allow SELECT operations on passive nodes. In an active-active cluster, every node is read-write and a change made to one is replicated to all.
 Galera is a database clustering solution that enables you to set up multi-master clusters using synchronous replication. Galera automatically handles keeping the data on different nodes in sync while allowing you to send read and write queries to any of the nodes in the cluster.
## How To Configure a Galera Cluster with MariaDB
Clustering adds high availability to your database by distributing changes to different servers. In the event that one of the instances fails, others are quickly available to continue serving.
#### Prerequisites
Three Ubuntu 18.04 Droplets with private networking enabled
### Installing MariaDB on All Servers
In this step, you will install the actual MariaDB packages on your three servers.
```
$ sudo apt install mariadb-server
```
logging into MariaDB:
```
$sudo mysql -uroot
```
```
MariaDB [(none)]> set password = password("your_password");
```
### Configuring galera
By default, MariaDB is configured to check the /etc/mysql/conf.d directory to get additional configuration settings from files ending in .cnf. Create a file in this directory with all of your cluster-specific directives:
```
$ sudo vim /etc/mysql/conf.d/galera.cnf
```
Add the following configuration into the file.
```
[mysqld]
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0

# Galera Provider Configuration
wsrep_on=ON
wsrep_provider=/usr/lib/galera/libgalera_smm.so

# Galera Cluster Configuration
wsrep_cluster_name="test_cluster"
wsrep_cluster_address="gcomm://192.168.43.40,192.168.43.41,192.168.43.42"

# Galera Synchronization Configuration
wsrep_sst_method=rsync

# Galera Node Configuration
wsrep_node_address="192.168.43.40" # ip for local server
wsrep_node_name="galera1" # name for localserver
```
### Starting the Cluster
In this step, you will start your MariaDB cluster. To begin, you need to stop the running MariaDB service so that you can bring your cluster online.
```
$ sudo systemctl stop mysql
```
#### Bring Up
sudo galera_new_cluster
```
$ mysql -u root -p -e "SHOW STATUS LIKE 'wsrep_cluster_size'"
```
```
Output
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| wsrep_cluster_size | 3     |
+--------------------+-------+
```
