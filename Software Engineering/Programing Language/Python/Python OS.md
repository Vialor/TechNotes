`import os`

# Files
## Read files
```python
try: # In case of IOError, make sure f is closed
    f = open('/path/to/file', 'r')
    print(f.read())
finally:
    if f:
        f.close()
# In short:
with open('/path/to/file', 'r') as f:
    print(f.read())
```

```python
# use read(size) to avoid overusing the memory.
f.read()
# read binary files
open('/path/to/file', 'rb')
# read non-UFT8 files (gbk for example)
open('/Users/michael/gbk.txt', 'r', encoding='gbk')
# write
with open('/path/to/file', 'w') as f: # 'w': overwrite, 'a': append
    f.write('Hello, world!')
# read by line
for line in f.readlines():
    print(line.strip()) # 把末尾的'\n'删掉

f.seek(num) # 修改当前读取位置， num bytes
f.seek(num, 1) # 从当前位置开始算
f.seek(-num, 2) # 从后开始算
f.tell() # 查看当前读取位置
```