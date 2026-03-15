# Basic Commands
```bash
pwd
ls <-lhAG>
cd

mkdir
touch

rmdir
rm

mv
cp
ln <-s> file1 file2

man
help

history
`ctrl + R` # search on history

date
date +FORMAT
date +"%FORMAT"
date +"%FORMAT%FORMAT"
date +"%FORMAT-%FORMAT"
e.g date +"%m-%d-%Y"

type -a <command>
which <command executable>

# for registered extension
start file_name
# mac
open file_name
# windows
explorer file_name

sleep seconds

expr 1 + 1
```
# Variables
```Plain
foo=bar
echo "$foo"
# prints bar
echo '$foo'
# prints $foo
```
- `$0` - Name of the script
- `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on.
- `$@` - All the arguments
- `$#` - Number of arguments
- `$?` - Return the exit code of the previous command
- `$$` - Process identification number (PID) for the current script
- `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!`
- `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.`
# IO
`0 stdin, 1 stdout, 2 stderr`
### Pipes & Redirections
```bash
CMD <input_file >output_file
CMD1 | CMD2 # pipe stdout
CMD1 |& CMD2 # pipe both stderr and stdout
CMD1 2>&1 >/dev/null | CMD2 # pipe stderr
xargs # STDIN => arguments

>> # append STDOUT to, this is the short form of `1>>`
2>> # append STDERR to
> # redirect STDOUT to
2> # redirect STDERR to
>& # redirect STDERR and STDOUT to
>>file.txt 2>&1 # append both STDERR and STDOUT to
/dev/null # null register

var=$(CMD) # Command Substitution: put output in a variable
<(CMD) # Process Substitution: put output in a temporary file

CMD & # run command as a job in the background, a job can contains one or more processes
```
### Combine Commands
conditions in `||` and `&&` are based on the **Return Code** of programs.
The `true` program will always have a 0 return code and the `false` command will always have a 1 return code.
```bash
# Error detection
false || echo "will run"
true || echo "will not run"

# Success detection
true && echo "will run"
false && echo "will not run"

# Combine
echo "After this"; echo "This will always run"
```
# Quotes and Parentheses
### () (()) {} \[] and \[\[]]
```bash
# Subshell
( result=$(echo "Hello"); echo result; ) # result is not initialized outside of this subshell
# can be written like this:
(
result=$(echo "Hello")
echo result;
)

# Calculation
a=5; b=3; ((sum = a + b)); echo $sum # output: 8, variable can be used without $

# Command group
echo "Hello" > output.txt; echo "World" > output.txt
# same as:
{ echo "Hello"; echo "World"; } > output.txt

# Test
test EXPRESSION # exit code 0 when true, otherwise false
[ EXPRESSION ] # traditional way, same as `test EXPRESSION`, don't use
[[ EXPRESSION ]] # more powerful than [], support regex `=~`, can parse && and ||
if [ -f "file1.txt" ] && [ -f "file2.txt" ]; then
    echo "Both files exist."
else
    echo "One or both files do not exist."
fi

```
### '' "" and \`\`
```bash
'everything here will be preserved as it is. $var is $var'
"everything here will be parsed. $(echo hello) is hello"
`echo hello` # this is the same as $(echo hello), better to use $()
```
# **Useful Command**
- `convert`
- `tar`: tape archive
    ```Plain
    tar xvf FileName.tar # recover
    tar cvf FileName.tar DirName # create
    ```