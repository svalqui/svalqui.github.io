Back up DB

```
#cli
mysqldump db-name > db1-bkp.sql
```
# Maria
```

MariaDB [(none)]> show databases;

MariaDB [(none)]> USE <DB_Name>;
MariaDB [DB_Name]> SHOW FULL TABLES;

MariaDB [(none)]> SHOW FIELDS FROM <table_name> FROM <DB_Name>;

MariaDB [(none)]> SHOW COLUMNS FROM <table_name> FROM <DB_Name>;

MariaDB [DB_Name]> SELECT * FROM <table_name>;

insert into  <table_name>(column_name1,) values ('Value_here1',);

UPDATE <table_name> SET user_email = 'user1@email.com' WHERE user_name = 'User1';
UPDATE <table_name> SET user_add = '123 St.' WHERE user_name = 'User1';

Comparing schemas
mysqldump --single-transaction --no-data db1 > db1-schema.sql
mysqldump --single-transaction --no-data db2 > db2-schema.sql
diff db1-schema.sql db2.schame.sql

```

# Postgres
```
# use the postgress admin account
# su - postgres

# get into the environment
$ psql

# Connect to DB
postgres=# \c seed_db;

# List a table
seed_db=# select * from auth_user;
```

# Mysql
```
mysql> show databases;
mysql> use <db-name>;
mysql> show tables;
mysql> describe <table-name>;

mysql> INSERT INTO Table-name (Column-name1, Column-name2, Column-name3)
VALUES ('Val1', 'Val2', 'Val3');

# delete DB
mysql> drop database <db_name>;
```
List DB Users

```
SELECT User FROM mysql.user;
```

When was lat updated

Ref https://stackoverflow.com/questions/307438/how-can-i-tell-when-a-mysql-table-was-last-updated
```
cd /var/lib/mysql/<mydatabase>
ls -lhtr *.ibd
```
