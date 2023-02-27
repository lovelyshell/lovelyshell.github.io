preload.(py\|js\|rb ...)是导入shuiguolao库的入口文件，导入这个文件即导入整个shuiguolao库。

这个文件初始化了一些全局变量:

**current**  
current.user   
:[User](./User-guide.md)  
current login user

current.dir  
:[File](./File-guide.md)  
current work directory

**cwd**  
:[File](./File-guide.md)  
current work directory, equilavant to current.dir

