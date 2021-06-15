MariaDB [(none)]> show databases;

MariaDB [(none)]> USE <DB_Name>;
MariaDB [DB_Name]> SHOW FULL TABLES;

MariaDB [(none)]> SHOW FIELDS FROM <table_name> FROM <DB_Name>;

MariaDB [(none)]> SHOW COLUMNS FROM <table_name> FROM <DB_Name>;

MariaDB [DB_Name]> SELECT * FROM <table_name>;

insert into  <table_name>(column_name1,) values ('Value_here1',);

UPDATE <table_name> SET user_email = 'user1@email.com' WHERE user_name = 'User1';
UPDATE <table_name> SET user_add = '123 St.' WHERE user_name = 'User1';
