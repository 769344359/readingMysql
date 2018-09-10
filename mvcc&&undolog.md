```
(gdb) bt
#0  ReadView::changes_visible (this=0x4e410a8, id=6406, name=...)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/include/read0types.h:166
#1  0x0000000001a633f6 in lock_clust_rec_cons_read_sees (
    rec=0x7f77bf078099 "\200", index=0x7f7748023b20, offsets=0x7f77b49ff2c0, 
    view=0x4e410a8)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/lock/lock0lock.cc:379
#2  0x0000000001b82940 in row_search_mvcc (buf=0x7f7748010150 "\375\001", 
    mode=PAGE_CUR_GE, prebuilt=0x7f774801a670, match_mode=1, direction=0)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0sel.cc:5616
#3  0x00000000019de3f4 in ha_innobase::index_read (this=0x7f774800fe60, 
    buf=0x7f7748010150 "\375\001", key_ptr=0x7f7748007720 "\002", key_len=4, 
    find_flag=HA_READ_KEY_EXACT)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/handler/ha_innodb.cc:8704
#4  0x0000000000fc8c8e in handler::index_read_map (this=0x7f774800fe60, 
    buf=0x7f7748010150 "\375\001", key=0x7f7748007720 "\002", keypart_map=1, 
    find_flag=HA_READ_KEY_EXACT)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/handler.h:2785
#5  0x0000000000fc488b in handler::index_read_idx_map (this=0x7f774800fe60, 
    buf=0x7f7748010150 "\375\001", index=0, key=0x7f7748007720 "\002", 
---Type <return> to continue, or q <return> to quit---
    keypart_map=1, find_flag=HA_READ_KEY_EXACT)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/handler.cc:7519
#6  0x0000000000fba15e in handler::ha_index_read_idx_map (this=0x7f774800fe60, 
    buf=0x7f7748010150 "\375\001", index=0, key=0x7f7748007720 "\002", 
    keypart_map=1, find_flag=HA_READ_KEY_EXACT)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/handler.cc:3083
#7  0x00000000015eb141 in read_const (table=0x7f774800f4b0, ref=0x7f774801b9f8)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_executor.cc:2007
#8  0x00000000015eabf7 in join_read_const_table (tab=0x7f7748007668, 
    pos=0x7f774801baa0)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_executor.cc:1892
#9  0x00000000016182ee in JOIN::extract_func_dependent_tables (
    this=0x7f774801b5f0)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_optimizer.cc:5580
#10 0x0000000001616cc0 in JOIN::make_join_plan (this=0x7f774801b5f0)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_optimizer.cc:5044
#11 0x000000000160ace1 in JOIN::optimize (this=0x7f774801b5f0)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_optimizer.cc:368
#12 0x000000000168adff in st_select_lex::optimize (this=0x7f77480057b0, thd=
    0x7f7748000b70)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_select.cc:1009
#13 0x0000000001689244 in handle_query (thd=0x7f7748000b70, 
    lex=0x7f7748002e78, result=0x7f7748007020, added_options=0, 
---Type <return> to continue, or q <return> to quit---
    removed_options=0)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_select.cc:164
#14 0x000000000163939e in execute_sqlcom_select (thd=0x7f7748000b70, 
    all_tables=0x7f7748006718)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:5156
#15 0x0000000001632405 in mysql_execute_command (thd=0x7f7748000b70, 
    first_level=true)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:2792
#16 0x000000000163a31c in mysql_parse (thd=0x7f7748000b70, 
    parser_state=0x7f77b4a01550)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:5582
#17 0x000000000162f0a3 in dispatch_command (thd=0x7f7748000b70, 
    com_data=0x7f77b4a01e00, command=COM_QUERY)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:1458
#18 0x000000000162df32 in do_command (thd=0x7f7748000b70)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:999
#19 0x0000000001770f97 in handle_connection (arg=0x57a2a50)
    at /home/dinosaur/Downloads/mysql-5.7.21/sql/conn_handler/connection_handler_per_thread.cc:300
#20 0x0000000001de0b41 in pfs_spawn_thread (arg=0x5795b20)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/perfschema/pfs.cc:2190
#21 0x00007f77d2d1a6ba in start_thread (arg=0x7f77b4a02700)
    at pthread_create.c:333
---Type <return> to continue, or q <return> to quit---
#22 0x00007f77d21a341d in clone ()
    at ../sysdeps/unix/sysv/linux/x86_64/clone.S:109

```
