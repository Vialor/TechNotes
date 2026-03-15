# Linux 三剑客
### grep 文本查找
```Plain
grep [OPTION]... 'PATTERN' [FILE]...
    -i --ignore-case
    -r --recursive
    -v --invert-match
    -c --count
    -o --only-matching
    --color
    -NUM # show context
    -A # print NUM lines of trailing context
    -E #extended-regexp
    -P --perl-regexp
```
See whole file with highlight: `grep --color 'pattern\|' file`
### sed 流编辑，按行编辑
This is an awful tool, but good to know. sed only supports POSIX regexp.
```Plain
sed 's/regexp/replace/g'
    -i --in-place # edit the file
    
get data from first part to second part
s/()()/\1\2/
```
### awk 编辑结构化文件，按列编辑
```Plain
awk '$1 == 1 && $2 ~ /^b.*e$/ {print $0}'
    # print lines who stose first col is 1, second col start with b and ends with e
    -F "<separator>" # default is space
    BEGIN, END can be used as condition, for example:
    awk '
    BEGIN { action_begin } 
    /pattern1/ { action1 }
    /pattern2/ { action2 }
    END { action_end } 
    ' filename
    
    sequence: 
        actions_begin
        sequentially match each line with pattern1 and pattern2, do action1     and action2 if necessary
        actions_end
```
# Find
|工具|查找方式|速度|是否实时|适用场景|
|---|---|---|---|---|
|`whereis`|查预设路径|很快|是|查命令、man页、源码路径|
|`locate`|查数据库|非常快|否|快速全系统查找已知文件名|
|`find`|实时遍历文件系统|慢|是|精确查找文件，条件复杂时使用|
### find
```shell
find [FOLDER]
    -name <target name> OR -iname <target name but case insensitive>
    -regextype posix-extended -regex ".*spr_portrait.*" # use regex
    -path <target path, eg. '**/*.pathext'> 
    -type <d/f> 
    -size <+min size/-max size>
    -mtime <last modified time, eg. -1 means "within a day">
    -exec
        <eg:
            convert {} {}.jpg
            rm {}
        >
        \;
        
# example
find . -type f \( -iname "*.jpg" -o -iname "*.png" \)
```
### fd
Install: `brew install fd`

> Main Usage: `fd <target file/folder> <in this folder> 'RegEx'`
- `e` extension
- `H` show hidden
- `I` show gitignore files
- `E` exclude
# Other commands
```Plain
sort filename
    -u
    -n
uniq filename
    -c
tail -n10 filename
    show last ten line
less
wc [OPTION]... [FILE]...
	-l count lines
	-w count words
	-m count chars
	-c count bytes
```