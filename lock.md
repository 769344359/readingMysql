http://mysql.taobao.org/monthly/2015/07/05/


```
#0  btr_cur_optimistic_insert (flags=0, cursor=0x7f77b49fee70, offsets=0x7f77b49fee20, heap=0x7f77b49fee18, entry=0x7f77480230e0, rec=0x7f77b49fee28, big_rec=0x7f77b49fee10, n_ext=0, 
    thr=0x7f774801b118, mtr=0x7f77b49ff290) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/btr/btr0cur.cc:3065
#1  0x0000000001b1e168 in row_ins_clust_index_entry_low (flags=0, mode=2, index=0x7f7748023b20, n_uniq=1, entry=0x7f77480230e0, n_ext=0, thr=0x7f774801b118, dup_chk_only=false)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:2598
#2  0x0000000001b203bf in row_ins_clust_index_entry (index=0x7f7748023b20, entry=0x7f77480230e0, thr=0x7f774801b118, n_ext=0, dup_chk_only=false)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3321
#3  0x0000000001b208c8 in row_ins_index_entry (index=0x7f7748023b20, entry=0x7f77480230e0, thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3457
#4  0x0000000001b20e59 in row_ins_index_entry_step (node=0x7f774801aea0, thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3607
#5  0x0000000001b21218 in row_ins (node=0x7f774801aea0, thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3749
#6  0x0000000001b21857 in row_ins_step (thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3934
#7  0x0000000001b4105f in row_insert_for_mysql_using_ins_graph (mysql_rec=0x7f7748010150 "\375\016", prebuilt=0x7f774801a670)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0mysql.cc:1738
#8  0x0000000001b4167d in row_insert_for_mysql (mysql_rec=0x7f7748010150 "\375\016", prebuilt=0x7f774801a670) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0mysql.cc:1862
#9  0x00000000019dba0a in ha_innobase::write_row (this=0x7f774800fe60, record=0x7f7748010150 "\375\016") at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/handler/ha_innodb.cc:7562
#10 0x0000000000fc5da6 in handler::ha_write_row (this=0x7f774800fe60, buf=0x7f7748010150 "\375\016") at /home/dinosaur/Downloads/mysql-5.7.21/sql/handler.cc:7992
#11 0x00000000018777b9 in write_record (thd=0x7f7748000b70, table=0x7f774800f4b0, info=0x7f77b4a00190, update=0x7f77b4a00210) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:1873
#12 0x000000000187462d in Sql_cmd_insert::mysql_insert (this=0x7f7748006900, thd=0x7f7748000b70, table_list=0x7f7748006370) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:769
#13 0x000000000187b6b1 in Sql_cmd_insert::execute (this=0x7f7748006900, thd=0x7f7748000b70) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:3107
#14 0x0000000001634413 in mysql_execute_command (thd=0x7f7748000b70, first_level=true) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:3566
#15 0x000000000163a31c in mysql_parse (thd=0x7f7748000b70, parser_state=0x7f77b4a01550) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:5582
#16 0x000000000162f0a3 in dispatch_command (thd=0x7f7748000b70, com_data=0x7f77b4a01e00, command=COM_QUERY) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:1458
#17 0x000000000162df32 in do_command (thd=0x7f7748000b70) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:999
#18 0x0000000001770f97 in handle_connection (arg=0x57a2a50) at /home/dinosaur/Downloads/mysql-5.7.21/sql/conn_handler/connection_handler_per_thread.cc:300
#19 0x0000000001de0b41 in pfs_spawn_thread (arg=0x5795b20) at /home/dinosaur/Downloads/mysql-5.7.21/storage/perfschema/pfs.cc:2190
#20 0x00007f77d2d1a6ba in start_thread (arg=0x7f77b4a02700) at pthread_create.c:333
#21 0x00007f77d21a341d in clone () at ../sysdeps/unix/sysv/linux/x86_64/clone.S:109

```
