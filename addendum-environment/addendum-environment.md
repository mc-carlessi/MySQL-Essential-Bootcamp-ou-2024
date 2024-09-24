# Addendum : Environment settings

## Introduction
This addendum contains the configuration files used for the labs. 


## Content of file my.cnf mysql1
```
[mysqld]
# General configurations
port=3307
mysqlx_port=33070
server_id=10
socket=/mysql/temp/mysql.sock
mysqlx_socket=/mysql/temp/mysqlx.sock

user=mysqluser

# File locations
basedir=/mysql/mysql-latest
plugin-dir=/mysql/mysql-latest/lib/plugin
datadir=/mysql/data
tmpdir=/mysql/temp
log-error=/mysql/log/err_log.log
general_log_file=/mysql/log/gl_log.log
slow_query_log_file=/mysql/log/sq_log.log

# Maximum limits
max-connections=200
open-files-limit=5000

# Security setting for file load
secure-file-priv=/mysql/data

# InnoDB settings
innodb_flush_log_at_trx_commit=1
innodb_buffer_pool_size=512M

# Enable GTID
gtid-mode=on
enforce-gtid-consistency=true

# Performance Monitor
performance_schema_consumer_events_statements_history_long = ON
# Plugin load example (MySQL Enterprise Thread Pool)
plugin-load=thread_pool.so
```

## Content of file my.cnf mysql2
```
[mysqld]
# General configurations
port=3307
mysqlx_port=33070
server_id=20
socket=/mysql/temp/mysql.sock
mysqlx_socket=/mysql/temp/mysqlx.sock

user=mysqluser

# File locations
basedir=/mysql/mysql-latest
plugin-dir=/mysql/mysql-latest/lib/plugin
datadir=/mysql/data
tmpdir=/mysql/temp
log-error=/mysql/log/err_log.log
general_log_file=/mysql/log/gl_log.log
slow_query_log_file=/mysql/log/sq_log.log

# Maximum limits
max-connections=200
open-files-limit=5000

# Security setting for file load
secure-file-priv=/mysql/data

# InnoDB settings
innodb_flush_log_at_trx_commit=1
innodb_buffer_pool_size=512M

# Enable GTID
gtid-mode=on
enforce-gtid-consistency=true

# Performance Monitor
performance_schema_consumer_events_statements_history_long = ON
# Plugin load example (MySQL Enterprise Thread Pool)
plugin-load=thread_pool.so;audit_log.so

audit_log_rotate_on_size=20971520
```

## Content of file my.cnf mysql3
```
[mysqld]
# General configurations
port=3307
mysqlx_port=33070
server_id=30
socket=/mysql/temp/mysql.sock
mysqlx_socket=/mysql/temp/mysqlx.sock

user=mysqluser

# File locations
basedir=/mysql/mysql-latest
plugin-dir=/mysql/mysql-latest/lib/plugin
datadir=/mysql/data
tmpdir=/mysql/temp
log-error=/mysql/log/err_log.log
general_log_file=/mysql/log/gl_log.log
slow_query_log_file=/mysql/log/sq_log.log

# Maximum limits
max-connections=200
open-files-limit=5000

# Security setting for file load
secure-file-priv=/mysql/data

# InnoDB settings
innodb_flush_log_at_trx_commit=1
innodb_buffer_pool_size=512M

# Enable GTID
gtid-mode=on
enforce-gtid-consistency=true

# Performance Monitor
performance_schema_consumer_events_statements_history_long = ON
# Plugin load example (MySQL Enterprise Thread Pool)
plugin-load=thread_pool.so;audit_log.so

audit_log_rotate_on_size=20971520
```

## Content of mysqld-advanced
```
[Unit]
Description=MySQL Server
Documentation=man:mysqld(8)
Documentation=http://dev.mysql.com/doc/refman/en/using-systemd.html
After=network.target
After=syslog.target

[Install]
WantedBy=multi-user.target

[Service]
User=mysqluser
Group=mysqlgrp

# Have mysqld write its state to the systemd notify socket
Type=notify

# Disable service start and stop timeout logic of systemd for mysqld service.
TimeoutSec=0

# Start main service
ExecStart=/mysql/mysql-latest/bin/mysqld --defaults-file=/mysql/etc/my.cnf $MYSQLD_OPTS

# Use this to switch malloc implementation
EnvironmentFile=-/etc/sysconfig/mysql-advanced-advanced

# Sets open_files_limit
LimitNOFILE = 10000

Restart=on-failure

RestartPreventExitStatus=1

# Set environment variable MYSQLD_PARENT_PID. This is required for restart.
Environment=MYSQLD_PARENT_PID=1

PrivateTmp=false
```

## Grants for mysqlbackup user
```
-- For the updated and complete list go to https://dev.mysql.com/doc/mysql-enterprise-backup/8.0/en/mysqlbackup.privileges.html
-- Minimal grants
GRANT SELECT, BACKUP_ADMIN, RELOAD, PROCESS, SUPER, REPLICATION CLIENT ON *.* TO `mysqlbackup`@`%`;
GRANT CREATE, INSERT, DROP, UPDATE ON mysql.backup_progress TO 'mysqlbackup'@'%';
GRANT CREATE, INSERT, DROP, UPDATE, SELECT, ALTER ON mysql.backup_history TO 'mysqlbackup'@'%';
-- For TTS
GRANT LOCK TABLES, CREATE, DROP, FILE ON *.* TO 'mysqlbackup'@'%';
-- For sbt
GRANT CREATE, INSERT, DROP, UPDATE ON mysql.backup_sbt_history TO 'mysqlbackup'@'%';
-- For encrypted InnoDB tables
GRANT ENCRYPTION_KEY_ADMIN ON *.* TO 'mysqlbackup'@'%';
-- For redo log archiving
GRANT INNODB_REDO_LOG_ARCHIVE ON *.* TO 'mysqlbackup'@'%';
-- For Table-Level Recovery
GRANT INSERT, ALTER ON *.* TO 'mysqlbackup'@'%';
-- For Group Replication all the servers need same account with same password and below grant
GRANT SELECT ON performance_schema.replication_group_members TO 'mysqlbackup'@'%';
```

## Acknowledgements
* **Author** - Marco Carlessi, Principal Sales Consultant
* **Contributors** -  Perside Foster, MySQL Solution Engineering, Selena Sánchez, MySQL Solutions Engineer
* **Last Updated By/Date** - Selena Sánchez, MySQL Solution Engineering, May 2023