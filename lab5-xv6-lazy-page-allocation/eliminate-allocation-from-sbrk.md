# Eliminate allocation from sbrk()

删去原来函数中分配物理内存的代码，只用改变myproc()->sz（即内存大小）即可。
