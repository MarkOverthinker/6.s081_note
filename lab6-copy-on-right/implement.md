# Implement

在创建子进程时，不为子进程创建父进程内存的拷贝，而是直接让子进程和父进程映射到相同的内存，并将这段内存中可写的内存标记为不可写。在不进行修改操作的时候对进程的运行没有影响，而在进行修改操作的时候再为进程创建这段内存的可写的拷贝，这样可以减少内存的拷贝，提高运行效率。

在usertrap中处理page fault

使用一个数组记录所有页面的引用计数，当kalloc()分配一个页面时，将它的引用计数设置为1。当fork导致子进程共享该页时，增加该页的引用计数;当任何进程将该页从其页表中删除时，减少该页的引用计数。kfree只能在一个页面的引用计数为0后才能释放它，否则只是减少一个引用计数。

copyout在内核中调用，不会进入usertrap，需要另外处理。

与lazy中修改argaddr的区别:

argaddr获取一个va,若该va还未分配pa，则进行分配

copyout涉及所有内核对用户内存的修改(包括系统调用修改内存时也会调用copyout来修改，故只需修改copyout即可覆盖内核中触发page fault的情况)，若修改的pa为cow，则不可写，需要分配内存然后再写。