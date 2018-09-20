##  Compare And Set 

#### 前言

##### at&t 语法
http://csiflabs.cs.ucdavis.edu/~ssdavis/50/att-syntax.htm

For example, the general format of a basic data movement instruction in INTEL-syntax is,
```
mnemonic	destination, source
```
whereas, in the case of AT&T, the general format is
```
mnemonic	source, destination
```
at&t 的语法会是 源操作数(source) 在前面,目的操作数(destination)在后面;而对于intel的汇编语法,则是相反

##### cmpxchg 指令
https://www.intel.com/content/www/us/en/architecture-and-technology/64-ia-32-architectures-software-developer-vol-2a-manual.html  
pdf 的283页 一共有三页讲述cmpxchg 指令  


https://stackoverflow.com/questions/15017659/how-to-read-the-intel-opcode-notation


cas 指令

```
(gdb) display /20i $pc
5: x/20i $pc
=> 0x1bcd694 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+38>:	mov    -0x8(%rbp),%rax
   0x1bcd698 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+42>:	sub    -0x20(%rbp),%rax
   0x1bcd69c <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+46>:	mov    %rax,%rdx
   0x1bcd69f <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+49>:	mov    -0x8(%rbp),%rax
   0x1bcd6a3 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+53>:	mov    -0x18(%rbp),%rcx
   0x1bcd6a7 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+57>:	add    $0x10,%rcx
   0x1bcd6ab <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+61>:	lock cmpxchg %rdx,(%rcx)
   0x1bcd6b0 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+66>:	sete   %al
   0x1bcd6b3 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+69>:	test   %al,%al
   0x1bcd6b5 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+71>:	je     0x1bcd6be <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+80>
   0x1bcd6b7 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+73>:	mov    $0x1,%eax
   0x1bcd6bc <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+78>:	jmp    0x1bcd6d1 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+99>
   0x1bcd6be <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+80>:	mov    -0x18(%rbp),%rax
   0x1bcd6c2 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+84>:	mov    0x10(%rax),%rax
   0x1bcd6c6 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+88>:	mov    %rax,-0x8(%rbp)
   0x1bcd6ca <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+92>:	jmp    0x1bcd68a <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+28>
   0x1bcd6cc <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+94>:	mov    $0x0,%eax
   0x1bcd6d1 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+99>:	pop    %rbp
   0x1bcd6d2 <rw_lock_lock_word_decr(rw_lock_t*, ulint, lint)+100>:	retq   
   0x1bcd6d3 <rw_lock_set_writer_id_and_recursion_flag(rw_lock_t*, bool)>:	push   %rbp
```

- 读锁
```
/******************************************************************//**
Low-level function which tries to lock an rw-lock in s-mode. Performs no
spinning.
@return TRUE if success */
UNIV_INLINE
ibool
rw_lock_s_lock_low(
/*===============*/
	rw_lock_t*	lock,	/*!< in: pointer to rw-lock */
	ulint		pass MY_ATTRIBUTE((unused)),
				/*!< in: pass value; != 0, if the lock will be
				passed to another thread to unlock */
	const char*	file_name, /*!< in: file name where lock requested */
	ulint		line)	/*!< in: line where requested */
{
	if (!rw_lock_lock_word_decr(lock, 1, 0)) {
		/* Locking did not succeed */
		return(FALSE);
	}
...
	return(TRUE);	/* locking succeeded */
}
```

- 写锁
```
/******************************************************************//**
Low-level function for acquiring an exclusive lock.
@return FALSE if did not succeed, TRUE if success. */
UNIV_INLINE
ibool
rw_lock_x_lock_low(
/*===============*/
	rw_lock_t*	lock,	/*!< in: pointer to rw-lock */
	ulint		pass,	/*!< in: pass value; != 0, if the lock will
				be passed to another thread to unlock */
	const char*	file_name,/*!< in: file name where lock requested */
	ulint		line)	/*!< in: line where requested */
{
	if (rw_lock_lock_word_decr(lock, X_LOCK_DECR, X_LOCK_HALF_DECR)) {  // cas 设置为lock_word - 0x2000000

		/* lock->recursive also tells us if the writer_thread
		field is stale or active. As we are going to write
		our own thread id in that field it must be that the
		current writer_thread value is not active. */
		ut_a(!lock->recursive);

		/* Decrement occurred: we are writer or next-writer. */
		rw_lock_set_writer_id_and_recursion_flag(
			lock, !pass);

		rw_lock_x_lock_wait(lock, pass, 0, file_name, line);

	} else {
      ...
	}

	...

	return(TRUE);
}
```

整个堆栈
```
(gdb) bt
#0  rw_lock_x_lock_wait_func (lock=0x7ffb6f9e5e60, pass=0, threshold=0, file_name=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/sync/sync0rw.cc:467
#1  0x0000000001bce1d2 in rw_lock_x_lock_low (lock=0x7ffb6f9e5e60, pass=0, file_name=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/sync/sync0rw.cc:566
#2  0x0000000001bce620 in rw_lock_x_lock_func (lock=0x7ffb6f9e5e60, pass=0, file_name=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/sync/sync0rw.cc:729
#3  0x0000000001c87b67 in pfs_rw_lock_x_lock_func (lock=0x7ffb6f9e5e60, pass=0, file_name=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/include/sync0rw.ic:711
#4  0x0000000001c97234 in buf_page_get_gen (page_id=..., page_size=..., rw_latch=2, guess=0x0, mode=10, file=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", 
    line=2493, mtr=0x7ffb7cd2c290, dirty_with_no_latch=false) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/buf/buf0buf.cc:4639
#5  0x0000000001c5bb11 in btr_cur_search_to_nth_level (index=0x7ffaf4023ac0, level=0, tuple=0x7ffaf4022f30, mode=PAGE_CUR_LE, latch_mode=2, cursor=0x7ffb7cd2be70, has_search_latch=0, 
    file=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493, mtr=0x7ffb7cd2c290) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/btr/btr0cur.cc:1107
#6  0x0000000001b17db0 in btr_pcur_open_low (index=0x7ffaf4023ac0, level=0, tuple=0x7ffaf4022f30, mode=PAGE_CUR_LE, latch_mode=2, cursor=0x7ffb7cd2be70, 
    file=0x23c61d0 "/home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc", line=2493, mtr=0x7ffb7cd2c290)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/include/btr0pcur.ic:465
#7  0x0000000001b1dc60 in row_ins_clust_index_entry_low (flags=0, mode=2, index=0x7ffaf4023ac0, n_uniq=1, entry=0x7ffaf4022f30, n_ext=0, thr=0x7ffaf401b298, dup_chk_only=false)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:2493
#8  0x0000000001b203bf in row_ins_clust_index_entry (index=0x7ffaf4023ac0, entry=0x7ffaf4022f30, thr=0x7ffaf401b298, n_ext=0, dup_chk_only=false)
    at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3321
#9  0x0000000001b208c8 in row_ins_index_entry (index=0x7ffaf4023ac0, entry=0x7ffaf4022f30, thr=0x7ffaf401b298) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3457
#10 0x0000000001b20e59 in row_ins_index_entry_step (node=0x7ffaf401b020, thr=0x7ffaf401b298) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3607
#11 0x0000000001b21218 in row_ins (node=0x7ffaf401b020, thr=0x7ffaf401b298) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3749
#12 0x0000000001b21857 in row_ins_step (thr=0x7ffaf401b298) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0ins.cc:3934
#13 0x0000000001b4105f in row_insert_for_mysql_using_ins_graph (mysql_rec=0x7ffaf4011ea0 "\375d", prebuilt=0x7ffaf401aa80) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0mysql.cc:1738
#14 0x0000000001b4167d in row_insert_for_mysql (mysql_rec=0x7ffaf4011ea0 "\375d", prebuilt=0x7ffaf401aa80) at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/row/row0mysql.cc:1862
#15 0x00000000019dba0a in ha_innobase::write_row (this=0x7ffaf4011bb0, record=0x7ffaf4011ea0 "\375d") at /home/dinosaur/Downloads/mysql-5.7.21/storage/innobase/handler/ha_innodb.cc:7562
#16 0x0000000000fc5da6 in handler::ha_write_row (this=0x7ffaf4011bb0, buf=0x7ffaf4011ea0 "\375d") at /home/dinosaur/Downloads/mysql-5.7.21/sql/handler.cc:7992
#17 0x00000000018777b9 in write_record (thd=0x7ffaf4000b70, table=0x7ffaf4011200, info=0x7ffb7cd2d190, update=0x7ffb7cd2d210) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:1873
#18 0x000000000187462d in Sql_cmd_insert::mysql_insert (this=0x7ffaf4006910, thd=0x7ffaf4000b70, table_list=0x7ffaf4006380) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:769
#19 0x000000000187b6b1 in Sql_cmd_insert::execute (this=0x7ffaf4006910, thd=0x7ffaf4000b70) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_insert.cc:3107
#20 0x0000000001634413 in mysql_execute_command (thd=0x7ffaf4000b70, first_level=true) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:3566
#21 0x000000000163a31c in mysql_parse (thd=0x7ffaf4000b70, parser_state=0x7ffb7cd2e550) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:5582
#22 0x000000000162f0a3 in dispatch_command (thd=0x7ffaf4000b70, com_data=0x7ffb7cd2ee00, command=COM_QUERY) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:1458
#23 0x000000000162df32 in do_command (thd=0x7ffaf4000b70) at /home/dinosaur/Downloads/mysql-5.7.21/sql/sql_parse.cc:999
#24 0x0000000001770f97 in handle_connection (arg=0x4e941e0) at /home/dinosaur/Downloads/mysql-5.7.21/sql/conn_handler/connection_handler_per_thread.cc:300
#25 0x0000000001de0b41 in pfs_spawn_thread (arg=0x4f64e60) at /home/dinosaur/Downloads/mysql-5.7.21/storage/perfschema/pfs.cc:2190
#26 0x00007ffb863676ba in start_thread (arg=0x7ffb7cd2f700) at pthread_create.c:333
#27 0x00007ffb857f041d in clone () at ../sysdeps/unix/sysv/linux/x86_64/clone.S:109

```
