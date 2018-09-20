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
