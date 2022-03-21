# 0x01 常用mysql数据操作函数
* 查询所有存在多的数据库
    ```

        mysql> SHOW DATABASES;
        +--------------------+
        | Database           |
        +--------------------+
        | information_schema |
        | challenges         |
        | dedecmsv57utf8sp1  |
        | mysql              |
        | performance_schema |
        | security           |
        +--------------------+
        6 rows in set (0.00 sec)
    ```
* 选择某一数据库
    ```
    mysql> USE mysql;
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A

    Database changed
    ```
* 查询当前数据库所有表
    ```
    mysql> SHOW TABLES;
    +---------------------------+
    | Tables_in_mysql           |
    +---------------------------+
    | columns_priv              |
    | db                        |
    | event                     |
    | func                      |
    | general_log               |
    | help_category             |
    | help_keyword              |
    | help_relation             |
    | help_topic                |
    | host                      |
    | ndb_binlog_index          |
    | plugin                    |
    | proc                      |
    | procs_priv                |
    | proxies_priv              |
    | servers                   |
    | slow_log                  |
    | tables_priv               |
    | time_zone                 |
    | time_zone_leap_second     |
    | time_zone_name            |
    | time_zone_transition      |
    | time_zone_transition_type |
    | user                      |
    +---------------------------+
    24 rows in set (0.00 sec)
    ```

* 查看某一数据库中某表结构
    ```
    mysql> DESC user;
    +------------------------+-----------------------------------+------+-----+---------+-------+
    | Field                  | Type                              | Null | Key | Default | Extra |
    +------------------------+-----------------------------------+------+-----+---------+-------+
    | Host                   | char(60)                          | NO   | PRI |         |       |
    | User                   | char(16)                          | NO   | PRI |         |       |
    | Password               | char(41)                          | NO   |     |         |       |
    | Select_priv            | enum('N','Y')                     | NO   |     | N       |       |
    | Insert_priv            | enum('N','Y')                     | NO   |     | N       |       |
    | Update_priv            | enum('N','Y')                     | NO   |     | N       |       |
    | Delete_priv            | enum('N','Y')                     | NO   |     | N       |       |
    | Create_priv            | enum('N','Y')                     | NO   |     | N       |       |
    | Drop_priv              | enum('N','Y')                     | NO   |     | N       |       |
    | Reload_priv            | enum('N','Y')                     | NO   |     | N       |       |
    | Shutdown_priv          | enum('N','Y')                     | NO   |     | N       |       |
    | Process_priv           | enum('N','Y')                     | NO   |     | N       |       |
    | File_priv              | enum('N','Y')                     | NO   |     | N       |       |
    | Grant_priv             | enum('N','Y')                     | NO   |     | N       |       |
    | References_priv        | enum('N','Y')                     | NO   |     | N       |       |
    | Index_priv             | enum('N','Y')                     | NO   |     | N       |       |
    | Alter_priv             | enum('N','Y')                     | NO   |     | N       |       |
    | Show_db_priv           | enum('N','Y')                     | NO   |     | N       |       |
    | Super_priv             | enum('N','Y')                     | NO   |     | N       |       |
    | Create_tmp_table_priv  | enum('N','Y')                     | NO   |     | N       |       |
    | Lock_tables_priv       | enum('N','Y')                     | NO   |     | N       |       |
    | Execute_priv           | enum('N','Y')                     | NO   |     | N       |       |
    | Repl_slave_priv        | enum('N','Y')                     | NO   |     | N       |       |
    | Repl_client_priv       | enum('N','Y')                     | NO   |     | N       |       |
    | Create_view_priv       | enum('N','Y')                     | NO   |     | N       |       |
    | Show_view_priv         | enum('N','Y')                     | NO   |     | N       |       |
    | Create_routine_priv    | enum('N','Y')                     | NO   |     | N       |       |
    | Alter_routine_priv     | enum('N','Y')                     | NO   |     | N       |       |
    | Create_user_priv       | enum('N','Y')                     | NO   |     | N       |       |
    | Event_priv             | enum('N','Y')                     | NO   |     | N       |       |
    | Trigger_priv           | enum('N','Y')                     | NO   |     | N       |       |
    | Create_tablespace_priv | enum('N','Y')                     | NO   |     | N       |       |
    | ssl_type               | enum('','ANY','X509','SPECIFIED') | NO   |     |         |       |
    | ssl_cipher             | blob                              | NO   |     | NULL    |       |
    | x509_issuer            | blob                              | NO   |     | NULL    |       |
    | x509_subject           | blob                              | NO   |     | NULL    |       |
    | max_questions          | int(11) unsigned                  | NO   |     | 0       |       |
    | max_updates            | int(11) unsigned                  | NO   |     | 0       |       |
    | max_connections        | int(11) unsigned                  | NO   |     | 0       |       |
    | max_user_connections   | int(11) unsigned                  | NO   |     | 0       |       |
    | plugin                 | char(64)                          | YES  |     |         |       |
    | authentication_string  | text                              | YES  |     | NULL    |       |
    +------------------------+-----------------------------------+------+-----+---------+-------+
    42 rows in set (0.00 sec)
    ```
* 查询某一数据库下数据表中数据
    ```
    mysql> SELECT Host,User,password FROM user;
    +--------------+------------------+-------------------------------------------+
    | Host         | User             | password                                  |
    +--------------+------------------+-------------------------------------------+
    | localhost    | root             | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 |
    | 77cb914f1328 | root             | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 |
    | 127.0.0.1    | root             | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 |
    | ::1          | root             | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 |
    | localhost    | debian-sys-maint | *7C2B63E7C94D3B94269E85187170A5D3033CD60D |
    +--------------+------------------+-------------------------------------------+
    5 rows in set (0.00 sec)
    ```

# 0x02 渗透测试常用的mysql数据库操作
* 需要注意一个特殊的数据库 `information_schema`，此库记录所有的数据库存储结构内容。
    ```
    mysql> SHOW DATABASES;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | challenges         |
    | dedecmsv57utf8sp1  |
    | mysql              |
    | performance_schema |
    | security           |
    +--------------------+
    6 rows in set (0.00 sec)
    ```
* 选择数据库`information_schema`
    ```
    mysql> USE information_schema;
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A

    Database changed
    ```
* 查看数据库`information_schema`，所有表。这里有几个特殊表需要记住：`SCHEMATA`，`COLUMNS `，`TABLES`
    ```
    mysql> SHOW TABLES;
    +---------------------------------------+
    | Tables_in_information_schema          |
    +---------------------------------------+
    | CHARACTER_SETS                        |
    | COLLATIONS                            |
    | COLLATION_CHARACTER_SET_APPLICABILITY |
    | COLUMNS                               |
    | COLUMN_PRIVILEGES                     |
    | ENGINES                               |
    | EVENTS                                |
    | FILES                                 |
    | GLOBAL_STATUS                         |
    | GLOBAL_VARIABLES                      |
    | KEY_COLUMN_USAGE                      |
    | PARAMETERS                            |
    | PARTITIONS                            |
    | PLUGINS                               |
    | PROCESSLIST                           |
    | PROFILING                             |
    | REFERENTIAL_CONSTRAINTS               |
    | ROUTINES                              |
    | SCHEMATA                              |
    | SCHEMA_PRIVILEGES                     |
    | SESSION_STATUS                        |
    | SESSION_VARIABLES                     |
    | STATISTICS                            |
    | TABLES                                |
    | TABLESPACES                           |
    | TABLE_CONSTRAINTS                     |
    | TABLE_PRIVILEGES                      |
    | TRIGGERS                              |
    | USER_PRIVILEGES                       |
    | VIEWS                                 |
    | INNODB_BUFFER_PAGE                    |
    | INNODB_TRX                            |
    | INNODB_BUFFER_POOL_STATS              |
    | INNODB_LOCK_WAITS                     |
    | INNODB_CMPMEM                         |
    | INNODB_CMP                            |
    | INNODB_LOCKS                          |
    | INNODB_CMPMEM_RESET                   |
    | INNODB_CMP_RESET                      |
    | INNODB_BUFFER_PAGE_LRU                |
    +---------------------------------------+
    40 rows in set (0.00 sec)
    ```
* 在数据库`information_schema`中`SCHEMATA`，查看其表结构与一些数据。这里我们发现在`SCHEMATA`表中的`SCHEMA_NAME`就是存放所有库名称。后面在进行爆库名的时候我们就会常常使用这个SQL查询语句
    ```
    mysql> DESC SCHEMATA;
    +----------------------------+--------------+------+-----+---------+-------+
    | Field                      | Type         | Null | Key | Default | Extra |
    +----------------------------+--------------+------+-----+---------+-------+
    | CATALOG_NAME               | varchar(512) | NO   |     |         |       |
    | SCHEMA_NAME                | varchar(64)  | NO   |     |         |       |
    | DEFAULT_CHARACTER_SET_NAME | varchar(32)  | NO   |     |         |       |
    | DEFAULT_COLLATION_NAME     | varchar(32)  | NO   |     |         |       |
    | SQL_PATH                   | varchar(512) | YES  |     | NULL    |       |
    +----------------------------+--------------+------+-----+---------+-------+
    5 rows in set (0.00 sec)

    mysql> SELECT SCHEMA_NAME FROM SCHEMATA;
    +--------------------+
    | SCHEMA_NAME        |
    +--------------------+
    | information_schema |
    | challenges         |
    | dedecmsv57utf8sp1  |
    | mysql              |
    | performance_schema |
    | security           |
    +--------------------+
    6 rows in set (0.00 sec)
    ```
* 通过`information_schema` 查询所有存在的数据库。-----所谓的`爆库名`
    ```

    ```