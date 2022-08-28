# pingpong

#### pipe函数

用于创建一个管道，如下：

```
int p[2];
pipe(p);
```

通常，p\[1]用于输入，p\[0]用于输出

管道的创建会尽可能选择小的fd作为管道口

#### read/write函数

从指定管道读取/向指定管道输入n字节数据，返回值为成功读取/写入的数据，当没有更多的字节可读时，`read`返回0来表示文件的结束。当管道上没有数据时，管道上的read操作将会阻塞。

```clike
// 从fd->4读取5个字节存入buf中
char* buf;
read(4, buf, 5);
// 向fd->5输入3个字节字符串aaa
write(5, "aaa", 3);
```

#### 思路

创建两个管道p,q，使用fork创建一个子线程，子线程中先调用read读管道p再调用write写q，父进程先调用write写p再调用read读q，由于没有数据时管道上的read会阻塞，因此一定是按照子线程先读，父进程再读的顺序执行。同时要注意关闭不需要用的管道。