# APUE

### 文件I/O

#### 文件描述符

每个进程在内核中都有一个进程控制块（PCB），通过一个结构体来维护进程的相关信息，在结构体中一个指针指向file_struct结构体，称之为文件描述符表，数据结构为数组，记录下这个进程打开的所有文件。文件描述符是**文件描述符表**的index，从0开始分配，也可以由dup2函数指定，文件描述符是open或create系统调用自动分配的，分配最小的可用文件描述符。文件描述符表的value是文件表指针，文件表指针指向**文件表项**， 内核为所有打开的文件维护一张文件表项，在文件表项中记录文件状态、**当前文件偏移量**、指向该文件inode的指针等重要信息。通过inode指针指向inode节点，linux通过inode来对文件进行管理，inode能够唯一的标识一个文件。inode节点中包含文件的字节数，文件的读写执行权限，以及**文件数据block的位置**，通过block的位置实现对数据的读、写。

#### 硬链接与软链接

硬链接是指两个文件的文件表项指向同一个inode，软链接是指文件A的内容是文件B的路径，读写文件A时系统会自动将访问者导向文件B，也称之为symbolic link。对于硬链接，如果删除文件B，不会影响文件A的读写，因为只是unlink，只有在链接数为0时才会被删除，而对于软链接，如果删除文件B，则无法访问文件A。

### 进程环境
