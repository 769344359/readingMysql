```
(gdb) bt
#0  rr_sequential (info=0x7fff6801beb8)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/records.cc:510
#1  0x00000000015ec46f in join_init_read_record (tab=0x7fff6801be68)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_executor.cc:2478
#2  0x00000000015e94b9 in sub_select (join=0x7fff6801ba00, 
    qep_tab=0x7fff6801be68, end_of_records=false)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_executor.cc:1271
#3  0x00000000015e8dcc in do_select (join=0x7fff6801ba00)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_executor.cc:944
#4  0x00000000015e6bc8 in JOIN::exec (this=0x7fff6801ba00)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_executor.cc:199
#5  0x00000000016892ba in handle_query (thd=0x7fff68000b70, 
    lex=0x7fff68002e78, result=0x7fff68007020, added_options=0, 
    removed_options=0)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_select.cc:184
#6  0x000000000163939e in execute_sqlcom_select (thd=0x7fff68000b70, 
    all_tables=0x7fff68006718)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:5156
#7  0x0000000001632405 in mysql_execute_command (thd=0x7fff68000b70, 
    first_level=true)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:2792
#8  0x000000000163a31c in mysql_parse (thd=0x7fff68000b70, 
    parser_state=0x7fffd92a1550)
---Type <return> to continue, or q <return> to quit---
   l_parse.cc:5582
#9  0x000000000162f0a3 in dispatch_command (thd=0x7fff68000b70, 
    com_data=0x7fffd92a1e00, command=COM_QUERY)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:1458
#10 0x000000000162df32 in do_command (thd=0x7fff68000b70)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:999
#11 0x0000000001770f97 in handle_connection (arg=0x49e1b60)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/conn_handler/connection_handler_per_thread.cc:300
#12 0x0000000001de0b41 in pfs_spawn_thread (arg=0x495b0a0)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/perfschema/pfs.cc:2190
#13 0x00007ffff757d6ba in start_thread (arg=0x7fffd92a2700)
    at pthread_create.c:333
#14 0x00007ffff6a1241d in clone ()
    at ../sysdeps/unix/sysv/linux/x86_64/clone.S:109
```
