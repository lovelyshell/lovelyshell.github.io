File is an abstraction of file in the unix concept _everything is file_

1 Access file information  
```python
f=File('/etc/passwd')
print(f.type)
print(f.size)
print(f.mtime)
print(f.owner)
print(f.perm)
print(File('.').type)
print(File('/bin/sh').type)
```

2 Access file content    
Data of different types of files are accessed by different getters.  

.dict  
data area of directory is described as a dictionary, subfile name as key, and corresponding File object as value.

.target  
data area of symbol link is described as a File object reference, constructed from link path.

.raw, .text, .lines  
data area of regular files can be read out as bytes, str or List, utf8 encoding required for the latter two.

```python  
#Example file lines statistic under current directory
d = File('.')
for f in d.dict.values():
    try:
        print(len(f.lines), f.name)
    except:
        print('----------skip ', f.name)
```

```python
#Example get final target of a symbol link
f = File('/usr/bin/gcc')
while f.isLink():
    f=f.target
    print('==>', f.path)
```

3 Recursively traverse the directory tree
```python
#Example find a file named 'foobar' recursively.
def search_foobar(d):
    for f in d.files:
        if f.name == 'foobar':
            print(f.path)
        if f.type == 'directory':
            search_foobar(f)
search_foobar(File('.')) 
```
`.files` above is another getter, which is a wrapper of `.dict.values()`.

Or use static method File.R():
```python
File.R('.', lambda f,r: print(f.path) if f.name == 'foobar' else None)
```

4 Modify file
Class File has a serial of methods to modify file information and content, like chmod(), writelines(), but you may be more insterested in their setters.
```python
#Modify file permission
sgl = File('/usr/bin/shuiguolao')
f = sgl.target
f.enable_setters()

print('in the begining ', f.perm)
p = f.perm
p.grp.w = False
p.oth.w = False
f.perm = p
print('we changed! ', f.perm)
#Use a second way
p.grp = 'rwx'
p.oth = 'rwx'
f.perm=p
print('we are back! ', f.perm)
#Use a third way
f.perm = 0o755
print('we changed! ', f.perm)

#Modify file owner
f.owner = User('root')
print('we changed! ', f.owner)
#fetch current user from global variable initialized by shuiguolao.
f.owner = current.user
print('we are back! ', f.owner)

f.disable_setters()
```
By design 
Using setters or methods depends on you, 


目录类型的File结构体，其数据区被解释成一个dict，其类型是Dict<str, File>，即以子文件名为键值，以对应File对象为值的一个字典。
链接类型的文件，其数据区被解释成一个target











     
