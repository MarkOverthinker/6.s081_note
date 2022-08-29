# A kernel page table per process

要求为每个进程创建一个独有的kernel pagetable。在每个进程执行时，切换到该进程专属的kernel pagetable执行进程。

#### 思路

在struct proc 中添加新的变量kernel\_table，用于保存该进程的内核页表。

在每个进程被创建的时候初始化该内核页表，参考kvminit函数实现一个修改版的函数用于创建一个kernel\_table并初始化（即使用mappages将内核虚拟地址映射到内存空间）。

在进程切换的时候，更改cpu中satp寄存器的值，使其指向将要切换到的进程的kernel\_table，并且清空TLB。在没有进程运行的时候，scheduler应将satp切换到全局的kernel\_pagetable。

在释放进程的时候，释放kernel\_table所占的空间。





