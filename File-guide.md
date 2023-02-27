File是对linux泛文件设计中文件的抽象，这些文件共用一个类File描述，你通过type字段区分他们。相较于很多语言库中已经出现的类似File的类，水果捞的File类从设计上考虑的应用场景也是日常的shell任务。

用类似ORM的方式访问文件，

向上呈现静态的数据结构和组织，向下转换成低级的API进行操作。
他们拥有相同的一系列属性，type,size,owner,






不同的文件类型通过File.type

File库对文件操作做了高级别的封装，以使得脚本模式能尽可能的取代命令行语法。

Example 1: Find most recently modified file 
files = cwd.files
files.sort(lambda f:f.mtime)
基本概述

文件读写
File提供了一个参数rw来提供最基本的访问保护。这个控制是File级别的，与底层的open函数的flag无关。
默认的rw = 'rwx',含义如下:
'r': 可读的 这表示readraw,readtext系列的API可以被调用
'w': 可写的 这表示writeraw,writetext系列的API可以被调用
'x': 写时创建 如果调用write系列API时，发现文件不存在，则创建 
File默认打开文件的行为，有点儿像编辑器，你可以用tabnew打开一个不存在的文件，但如果你不按':w'，它就不会创建文件并保存。

基本上，File的任何一个读写API都会在底层经过open(), read()/write(), close()阶段。File不持有打开的文件指针，不管是C语言级别的，还是python级别的。

目前只提供对文件内容的写入函数，不允许修改属性。


