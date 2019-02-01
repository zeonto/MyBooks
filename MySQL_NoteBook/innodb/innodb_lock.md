Innodb 事务锁笔记
================

常用事务查看表

```
# 当前事务
select * from information_schema.innodb_trx;
# 当前事务锁
select * from information_schema.innodb_locks;
# 当前事务锁等待
select * from information_schema.innodb_lock_waits;
```

查看某段时间前执行并且还没有关闭的事务
```
> select * from information_schema.innodb_trx as trx left join information_schema.processlist as p on trx.trx_mysql_thread_id = p.id where trx.trx_started <= date_add(now(), interval -5 second)\G;
*************************** 1. row ***************************
                    trx_id: 9988
                 trx_state: RUNNING
               trx_started: 2019-02-01 15:04:30
     trx_requested_lock_id: NULL
          trx_wait_started: NULL
                trx_weight: 4
       trx_mysql_thread_id: 9
                 trx_query: NULL
       trx_operation_state: NULL
         trx_tables_in_use: 0
         trx_tables_locked: 1
          trx_lock_structs: 2
     trx_lock_memory_bytes: 1136
           trx_rows_locked: 3
         trx_rows_modified: 2
   trx_concurrency_tickets: 0
       trx_isolation_level: REPEATABLE READ
         trx_unique_checks: 1
    trx_foreign_key_checks: 1
trx_last_foreign_key_error: NULL
 trx_adaptive_hash_latched: 0
          trx_is_read_only: 0
trx_autocommit_non_locking: 0
                        ID: 9
                      USER: root
                      HOST: localhost
                        DB: test
                   COMMAND: Sleep
                      TIME: 7885
                     STATE: 
                      INFO: NULL
                   TIME_MS: 7885094.910
                     STAGE: 0
                 MAX_STAGE: 0
                  PROGRESS: 0.000
               MEMORY_USED: 60248
             EXAMINED_ROWS: 0
                  QUERY_ID: 16
               INFO_BINARY: NULL
                       TID: 6509
```


```
> select * from information_schema.innodb_trx as trx,information_schema.processlist as p,information_schema.innodb_locks as ilock where trx.trx_mysql_thread_id = p.id and trx.trx_id = ilock.lock_trx_id and trx.trx_started <= date_add(now(), interval -5 second)\G;
*************************** 1. row ***************************
                    trx_id: 9993
                 trx_state: LOCK WAIT
               trx_started: 2019-02-01 17:10:50
     trx_requested_lock_id: 9993:4:3:2
          trx_wait_started: 2019-02-01 17:10:50
                trx_weight: 2
       trx_mysql_thread_id: 10
                 trx_query: select * from m_test where value = 11 for update
       trx_operation_state: starting index read
         trx_tables_in_use: 1
         trx_tables_locked: 1
          trx_lock_structs: 2
     trx_lock_memory_bytes: 1136
           trx_rows_locked: 1
         trx_rows_modified: 0
   trx_concurrency_tickets: 0
       trx_isolation_level: REPEATABLE READ
         trx_unique_checks: 1
    trx_foreign_key_checks: 1
trx_last_foreign_key_error: NULL
 trx_adaptive_hash_latched: 0
          trx_is_read_only: 0
trx_autocommit_non_locking: 0
                        ID: 10
                      USER: root
                      HOST: localhost
                        DB: test
                   COMMAND: Query
                      TIME: 18
                     STATE: Sending data
                      INFO: select * from m_test where value = 11 for update
                   TIME_MS: 18770.087
                     STAGE: 0
                 MAX_STAGE: 0
                  PROGRESS: 0.000
               MEMORY_USED: 60248
             EXAMINED_ROWS: 0
                  QUERY_ID: 407
               INFO_BINARY: select * from m_test where value = 11 for update
                       TID: 6624
                   lock_id: 9993:4:3:2
               lock_trx_id: 9993
                 lock_mode: X
                 lock_type: RECORD
                lock_table: `test`.`m_test`
                lock_index: PRIMARY
                lock_space: 4
                 lock_page: 3
                  lock_rec: 2
                 lock_data: 1
*************************** 2. row ***************************
                    trx_id: 9988
                 trx_state: RUNNING
               trx_started: 2019-02-01 15:04:30
     trx_requested_lock_id: NULL
          trx_wait_started: NULL
                trx_weight: 4
       trx_mysql_thread_id: 9
                 trx_query: NULL
       trx_operation_state: NULL
         trx_tables_in_use: 0
         trx_tables_locked: 1
          trx_lock_structs: 2
     trx_lock_memory_bytes: 1136
           trx_rows_locked: 3
         trx_rows_modified: 2
   trx_concurrency_tickets: 0
       trx_isolation_level: REPEATABLE READ
         trx_unique_checks: 1
    trx_foreign_key_checks: 1
trx_last_foreign_key_error: NULL
 trx_adaptive_hash_latched: 0
          trx_is_read_only: 0
trx_autocommit_non_locking: 0
                        ID: 9
                      USER: root
                      HOST: localhost
                        DB: test
                   COMMAND: Sleep
                      TIME: 7598
                     STATE: 
                      INFO: NULL
                   TIME_MS: 7598190.422
                     STAGE: 0
                 MAX_STAGE: 0
                  PROGRESS: 0.000
               MEMORY_USED: 60248
             EXAMINED_ROWS: 0
                  QUERY_ID: 16
               INFO_BINARY: NULL
                       TID: 6509
                   lock_id: 9988:4:3:2
               lock_trx_id: 9988
                 lock_mode: X
                 lock_type: RECORD
                lock_table: `test`.`m_test`
                lock_index: PRIMARY
                lock_space: 4
                 lock_page: 3
                  lock_rec: 2
                 lock_data: 1
```