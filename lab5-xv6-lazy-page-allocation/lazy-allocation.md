# Lazy allocation

由于未分配物理内存，只改变了内存大小，在访问的内存位于这段内存时便会出现page fault,引发trap。

所以需要在usertrap()函数中进行修改，处理page fault。调用r\_scause读取引起trap的原因，当值为13或15的时候说明存在page\_fault，使用kalloc申请一块物理内存，调用r\_stval读取引发page fault的内存地址并使用mappages将这段地址映射到申请的物理内存中。

由于延迟分配，可能出现释放或复制pagetable中不存在的pte的情况，在原来的代码中将这种情况是为错误，现在可以认为这不是错误，需要修改一下代码。

另外，在进行系统调用时，此时处于内核空间,若处罚page fault不会进入usertra()中，无法使用usertrap中的page fault处理代码，因此需要修改argaddr()函数，在某个系统调用调用此函数的时候可以检查获得的地址是否已分配物理空间，然后对未分配的进行分配，就像usertrap里处理page fault时一样。
