```
(gdb) bt
#0  execute_sqlcom_select (thd=0x7fff68000b70, all_tables=0x7fff68006718)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:5111
#1  0x0000000001632405 in mysql_execute_command (thd=0x7fff68000b70, first_level=true)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:2792
#2  0x000000000163a31c in mysql_parse (thd=0x7fff68000b70, parser_state=0x7fffd92a1550)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:5582
#3  0x000000000162f0a3 in dispatch_command (thd=0x7fff68000b70, com_data=0x7fffd92a1e00, 
    command=COM_QUERY) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:1458
#4  0x000000000162df32 in do_command (thd=0x7fff68000b70)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:999
#5  0x0000000001770f97 in handle_connection (arg=0x4952cb0)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/conn_handler/connection_handler_per_thread.cc:300
#6  0x0000000001de0b41 in pfs_spawn_thread (arg=0x4a0a4c0)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/perfschema/pfs.cc:2190
#7  0x00007ffff757d6ba in start_thread (arg=0x7fffd92a2700) at pthread_create.c:333
#8  0x00007ffff6a1241d in clone () at ../sysdeps/unix/sysv/linux/x86_64/clone.S:109
```
