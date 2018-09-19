今天下午要看 这个函数`page_cur_insert_rec_low`  
这个貌似是redo log 的函数:`page_cur_insert_rec_write_log`

http://mysql.taobao.org/monthly/2015/07/05/

https://blog.csdn.net/jh993627471/article/details/79239720

https://dev.mysql.com/doc/internals/en/innodb-page-header.html

http://www.cnblogs.com/jackhub/p/3830683.html

https://yq.aliyun.com/articles/55859

https://www.bbsmax.com/A/qVdeYEjAdP/

http://mysql.taobao.org/monthly/2015/05/01/

https://blog.csdn.net/yuanrxdu/article/details/41698809
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


```
btr_cur_search_to_nth_level (index=0x7f7748023b20, level=0, tuple=0x7f77480230e0, mode=PAGE_CUR_LE, latch_mode=2, cursor=0x7f77b49fee70, has_search_latch=0, 
    file=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493, mtr=0x7f77b49ff290) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/btr/btr0cur.cc:755
#1  0x0000000001b17db0 in btr_pcur_open_low (index=0x7f7748023b20, level=0, tuple=0x7f77480230e0, mode=PAGE_CUR_LE, latch_mode=2, cursor=0x7f77b49fee70, 
    file=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493, mtr=0x7f77b49ff290)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/include/btr0pcur.ic:465
#2  0x0000000001b1dc60 in row_ins_clust_index_entry_low (flags=0, mode=2, index=0x7f7748023b20, n_uniq=1, entry=0x7f77480230e0, n_ext=0, thr=0x7f774801b118, dup_chk_only=false)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:2493
#3  0x0000000001b203bf in row_ins_clust_index_entry (index=0x7f7748023b20, entry=0x7f77480230e0, thr=0x7f774801b118, n_ext=0, dup_chk_only=false)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3321
#4  0x0000000001b208c8 in row_ins_index_entry (index=0x7f7748023b20, entry=0x7f77480230e0, thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3457
#5  0x0000000001b20e59 in row_ins_index_entry_step (node=0x7f774801aea0, thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3607
#6  0x0000000001b21218 in row_ins (node=0x7f774801aea0, thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3749
#7  0x0000000001b21857 in row_ins_step (thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3934
#8  0x0000000001b4105f in row_insert_for_mysql_using_ins_graph (mysql_rec=0x7f7748010150 "\375\017", prebuilt=0x7f774801a670)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0mysql.cc:1738
#9  0x0000000001b4167d in row_insert_for_mysql (mysql_rec=0x7f7748010150 "\375\017", prebuilt=0x7f774801a670) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0mysql.cc:1862
#10 0x00000000019dba0a in ha_innobase::write_row (this=0x7f774800fe60, record=0x7f7748010150 "\375\017") at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/handler/ha_innodb.cc:7562
#11 0x0000000000fc5da6 in handler::ha_write_row (this=0x7f774800fe60, buf=0x7f7748010150 "\375\017") at /home/dinosaur/Downloads/mysql-5.7.21/sql/handler.cc:7992
#12 0x00000000018777b9 in write_record (thd=0x7f7748000b70, table=0x7f774800f4b0, info=0x7f77b4a00190, update=0x7f77b4a00210) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:1873
#13 0x000000000187462d in Sql_cmd_insert::mysql_insert (this=0x7f7748006900, thd=0x7f7748000b70, table_list=0x7f7748006370) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:769
#14 0x000000000187b6b1 in Sql_cmd_insert::execute (this=0x7f7748006900, thd=0x7f7748000b70) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:3107
#15 0x0000000001634413 in mysql_execute_command (thd=0x7f7748000b70, first_level=true) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:3566
#16 0x000000000163a31c in mysql_parse (thd=0x7f7748000b70, parser_state=0x7f77b4a01550) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:5582
#17 0x000000000162f0a3 in dispatch_command (thd=0x7f7748000b70, com_data=0x7f77b4a01e00, command=COM_QUERY) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:1458
#18 0x000000000162df32 in do_command (thd=0x7f7748000b70) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:999
#19 0x0000000001770f97 in handle_connection (arg=0x57a2a50) at /home/dinosaur/Downloads/mysql-5.7.21/sql/conn_handler/connection_handler_per_thread.cc:300
#20 0x0000000001de0b41 in pfs_spawn_thread (arg=0x5795b20) at /home/dinosaur/Downloads/mysql-5.7.21/storage/perfschema/pfs.cc:2190
#21 0x00007f77d2d1a6ba in start_thread (arg=0x7f77b4a02700) at pthread_create.c:333
#22 0x00007f77d21a341d in clone () at ../sysdeps/unix/sysv/linux/x86_64/clone.S:109
(gdb) 

```

```
// mysql-server-5.7\storage\innobase\include\lock0types.h
/* Basic lock modes */
enum lock_mode {
LOCK_IS = 0,     /* intention shared */
LOCK_IX,     /* intention exclusive */
LOCK_S,          /* shared */
LOCK_X,          /* exclusive */
LOCK_AUTO_INC,     /* locks the auto-inc counter of a table
in an exclusive mode */
LOCK_NONE,     /* this is used elsewhere to note consistent read */
LOCK_NUM = LOCK_NONE, /* number of lock modes */
LOCK_NONE_UNSET = 255
};
```

```
(gdb) bt
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
#7  0x0000000001b4105f in row_insert_for_mysql_using_ins_graph (mysql_rec=0x7f7748010150 "\375\017", prebuilt=0x7f774801a670)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0mysql.cc:1738
#8  0x0000000001b4167d in row_insert_for_mysql (mysql_rec=0x7f7748010150 "\375\017", prebuilt=0x7f774801a670) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0mysql.cc:1862
#9  0x00000000019dba0a in ha_innobase::write_row (this=0x7f774800fe60, record=0x7f7748010150 "\375\017") at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/handler/ha_innodb.cc:7562
#10 0x0000000000fc5da6 in handler::ha_write_row (this=0x7f774800fe60, buf=0x7f7748010150 "\375\017") at /home/dinosaur/Downloads/mysql-5.7.21/sql/handler.cc:7992
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

index 结构
```
$112 = (const dict_index_t *) 0x7f7748023b20
(gdb) p *index
$113 = {
  id = 41, 
  heap = 0x7f7748023988, 
  name = {
    m_name = 0x7f7748023e20 "PRIMARY"
  }, 
  table_name = 0x7f77480107d8 "test/test", 
  table = 0x7f7748022420, 
  space = 23, 
  page = 3, 
  merge_threshold = 50, 
  type = 3, 
  trx_id_offset = 4, 
  n_user_defined_cols = 1, 
  allow_duplicates = 0, 
  nulls_equal = 0, 
  disable_ahi = 0, 
  n_uniq = 1, 
  n_def = 4, 
  n_fields = 4, 
  n_nullable = 1, 
  cached = 1, 
  to_be_dropped = 0, 
  online_status = 0, 
  uncommitted = 0, 
  magic_n = 76789786, 
  fields = 0x7f7748023e28, 
  parser = 0x0, 
  is_ngram = false, 
  has_new_v_col = false, 
  index_fts_syncing = false, 
  indexes = {
    prev = 0x0, 
    next = 0x0
  }, 
  search_info = 0x7f7748023ed8, 
  online_log = 0x0, 
  stat_n_diff_key_vals = 0x7f7748023ec0, 
  stat_n_sample_sizes = 0x7f7748023ec8, 
  stat_n_non_null_key_vals = 0x7f7748023ed0, 
  stat_index_size = 1, 
  stat_n_leaf_pages = 1, 
  last_ins_cur = 0x0, 
  last_sel_cur = 0x0, 
  rec_cache = {
    rec_size = 0, 
    offsets = 0x0, 
    sz_of_offsets = 0, 
    fixed_len_key = false, 
    offsets_cached = false, 
    key_has_null_cols = false
  }, 
  rtr_ssn = {
    mutex = {
      m_impl = {
        m_lock_word = 0, 
        m_waiters = 0, 
        m_event = 0x0, 
        m_policy = {
          <MutexDebug<TTASEventMutex<GenericPolicy> >> = {
            _vptr.MutexDebug = 0x0, 
---Type <return> to continue, or q <return> to quit---
            m_magic_n = 0, 
            m_context = {
              <latch_t> = {
                _vptr.latch_t = 0x0, 
                m_id = LATCH_ID_NONE, 
                m_rw_lock = false, 
                m_temp_fsp = false
              }, 
              members of MutexDebug<TTASEventMutex<GenericPolicy> >::Context: 
              m_mutex = 0x0, 
              m_filename = 0x0, 
              m_line = 0, 
              m_thread_id = 0
            }
          }, 
          members of GenericPolicy<TTASEventMutex<GenericPolicy> >: 
          m_count = {
            m_spins = 0, 
            m_waits = 0, 
            m_calls = 0, 
            m_enabled = false
          }, 
          m_id = LATCH_ID_NONE
        }
      }, 
      m_ptr = 0x0
    }, 
    seq_no = 0
  }, 
  rtr_track = 0x0, 
  trx_id = 0, 
  zip_pad = {
    mutex = 0x0, 
    pad = 0, 
    success = 0, 
    failure = 0, 
    n_rounds = 0, 
    mutex_created = 0
  }, 
  lock = {
    <latch_t> = {
      _vptr.latch_t = 0x2dd8870 <vtable for rw_lock_t+16>, 
      m_id = LATCH_ID_INDEX_TREE, 
      m_rw_lock = true, 
      m_temp_fsp = false
    }, 
    members of rw_lock_t: 
    lock_word = 536870912, 
    waiters = 0, 
    recursive = false, 
    sx_recursive = 0, 
    writer_is_wait_ex = false, 
    writer_thread = 140151124637440, 
    event = 0x7f77480238c8, 
    wait_ex_event = 0x7f7748024408, 
    cfile_name = 0x2425ba0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/dict/dict0dict.cc", 
    last_s_file_name = 0x240a138 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/btr/btr0cur.cc", 
    last_x_file_name = 0x242cdc8 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/dict/dict0stats.cc", 
    cline = 2685, 
    is_block_lock = 0, 
    last_s_line = 1008, 
---Type <return> to continue, or q <return> to quit---
    last_x_line = 1932, 
    count_os_wait = 0, 
    list = {
      prev = 0x7f7748012098, 
      next = 0x56928a8
    }, 
    pfs_psi = 0x7f77cd44d940, 
    magic_n = 22643, 
    debug_list = {
      count = 0, 
      start = 0x0, 
      end = 0x0, 
      node = &rw_lock_debug_t::list, 
      init = 51966
    }, 
    level = SYNC_INDEX_TREE
  }
}

```

lock 堆栈打印
```
(gdb) bt
#0  rw_lock_x_lock_func (lock=0x7f77be58a328, pass=0, file_name=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/sync/sync0rw.cc:718
#1  0x0000000001c87b67 in pfs_rw_lock_x_lock_func (lock=0x7f77be58a328, pass=0, file_name=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/include/sync0rw.ic:711
#2  0x0000000001c97234 in buf_page_get_gen (page_id=..., page_size=..., rw_latch=2, guess=0x0, mode=10, file=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", 
    line=2493, mtr=0x7f77b49ff290, dirty_with_no_latch=false) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/buf/buf0buf.cc:4639
#3  0x0000000001c5bb11 in btr_cur_search_to_nth_level (index=0x7f7748023b20, level=0, tuple=0x7f77480230e0, mode=PAGE_CUR_LE, latch_mode=2, cursor=0x7f77b49fee70, has_search_latch=0, 
    file=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493, mtr=0x7f77b49ff290) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/btr/btr0cur.cc:1107
#4  0x0000000001b17db0 in btr_pcur_open_low (index=0x7f7748023b20, level=0, tuple=0x7f77480230e0, mode=PAGE_CUR_LE, latch_mode=2, cursor=0x7f77b49fee70, 
    file=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493, mtr=0x7f77b49ff290)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/include/btr0pcur.ic:465
#5  0x0000000001b1dc60 in row_ins_clust_index_entry_low (flags=0, mode=2, index=0x7f7748023b20, n_uniq=1, entry=0x7f77480230e0, n_ext=0, thr=0x7f774801b118, dup_chk_only=false)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:2493
#6  0x0000000001b203bf in row_ins_clust_index_entry (index=0x7f7748023b20, entry=0x7f77480230e0, thr=0x7f774801b118, n_ext=0, dup_chk_only=false)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3321
#7  0x0000000001b208c8 in row_ins_index_entry (index=0x7f7748023b20, entry=0x7f77480230e0, thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3457
#8  0x0000000001b20e59 in row_ins_index_entry_step (node=0x7f774801aea0, thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3607
#9  0x0000000001b21218 in row_ins (node=0x7f774801aea0, thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3749
#10 0x0000000001b21857 in row_ins_step (thr=0x7f774801b118) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3934
#11 0x0000000001b4105f in row_insert_for_mysql_using_ins_graph (mysql_rec=0x7f7748010150 "\375\020", prebuilt=0x7f774801a670)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0mysql.cc:1738
#12 0x0000000001b4167d in row_insert_for_mysql (mysql_rec=0x7f7748010150 "\375\020", prebuilt=0x7f774801a670) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0mysql.cc:1862
#13 0x00000000019dba0a in ha_innobase::write_row (this=0x7f774800fe60, record=0x7f7748010150 "\375\020") at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/handler/ha_innodb.cc:7562
#14 0x0000000000fc5da6 in handler::ha_write_row (this=0x7f774800fe60, buf=0x7f7748010150 "\375\020") at /home/dinosaur/Downloads/mysql-5.7.21/sql/handler.cc:7992
#15 0x00000000018777b9 in write_record (thd=0x7f7748000b70, table=0x7f774800f4b0, info=0x7f77b4a00190, update=0x7f77b4a00210) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:1873
#16 0x000000000187462d in Sql_cmd_insert::mysql_insert (this=0x7f7748006900, thd=0x7f7748000b70, table_list=0x7f7748006370) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:769
#17 0x000000000187b6b1 in Sql_cmd_insert::execute (this=0x7f7748006900, thd=0x7f7748000b70) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:3107
#18 0x0000000001634413 in mysql_execute_command (thd=0x7f7748000b70, first_level=true) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:3566
#19 0x000000000163a31c in mysql_parse (thd=0x7f7748000b70, parser_state=0x7f77b4a01550) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:5582
#20 0x000000000162f0a3 in dispatch_command (thd=0x7f7748000b70, com_data=0x7f77b4a01e00, command=COM_QUERY) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:1458
#21 0x000000000162df32 in do_command (thd=0x7f7748000b70) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:999
#22 0x0000000001770f97 in handle_connection (arg=0x57a2a50) at /home/dinosaur/Downloads/mysql-5.7.21/sql/conn_handler/connection_handler_per_thread.cc:300
#23 0x0000000001de0b41 in pfs_spawn_thread (arg=0x5795b20) at /home/dinosaur/Downloads/mysql-5.7.21/storage/perfschema/pfs.cc:2190
#24 0x00007f77d2d1a6ba in start_thread (arg=0x7f77b4a02700) at pthread_create.c:333
#25 0x00007f77d21a341d in clone () at ../sysdeps/unix/sysv/linux/x86_64/clone.S:109

```
```
# define rw_lock_x_lock(M)					\
	pfs_rw_lock_x_lock_func((M), 0, __FILE__, __LINE__)
```
