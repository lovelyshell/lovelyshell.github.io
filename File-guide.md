File是对linux泛文件设计中文件的抽象，这些文件共用一个类File描述，通过type字段区分他们。File类向上呈现静态的数据结构和组织，向下转换成低级的API进行操作。相较于很多语言库中已经出现的类似File的类，水果捞的File类从设计上考虑的应用场景也是日常的shell任务。

1 Access file information  
'''python
f=File('/etc/passwd')
print(f.type)
print(f.size)
print(f.mtime)
print(f.owner)
print(f.perm)
print(File('.').type)
print(File('/bin/sh').type)
'''


2 Access file data  
data area of files of different types are accessed by different getters.
.dict 
data area of directory is described by a dictionary, subfile names as key, and corresponding File object as value.
.target 
data area of symbol link is described by a File object reference, constructed from link path.
.raw, .text, .lines 
data area of regular files can be read out as bytes, str or List, utf8 encoding required for the latter two.

'''python  
#Example statistic file lines under current directory
d = File('.')
for f in d.files:
    try:
        print(len(f.lines), f.name)
    except:
        print('----------skip ', f.name)
'''

'''python
#get final target of a symbol link
f = File('/usr/bin/gcc')
while f.isLink():
    print('==>', f.target.path)
    f=f.target
'''

'''python
#通过setter直接设置对象的属性。
f = File('/usr/bin/shuiguolao').target
f.enable_setters()
f.owner = current.user

print(f.perm)
p = f.perm
p.grp.w = False
p.oth.w = False
f.perm = p
print('we changed! ', f.perm)
#use a second way
f.perm = 0o777
print('we are back! ', f.perm)
#use a third way
p.grp = 'rx'
p.oth = 'rx'
f.perm=p
print('we changed! ', f.perm)
f.disable_setters()
'''


'''python
#Example 递归寻找名为foorbar的文件
def search_foobar(d):
    for f in d.files:
        if f.name == 'foobar':
            print(f.path)
        if f.type == 'directory':
            search_foobar(f)
search_foobar(File('.')) 
#或者使用File.R()
File.R('.', lambda f,r: print(f.path) if f.name == 'foobar' else None)
'''

目录类型的File结构体，其数据区被解释成一个dict，其类型是Dict<str, File>，即以子文件名为键值，以对应File对象为值的一个字典。
链接类型的文件，其数据区被解释成一个target
     
