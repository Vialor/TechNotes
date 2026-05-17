# Start
## Runtime
1. 命令式调用：你主动调用时执行，比如 Redis EVAL  
2. 钩子 / 回调式：软件运行到某个生命周期点时执行，比如游戏事件、Nginx 请求阶段  
3. 配置扩展式：软件启动或加载配置时执行 Lua，用来定义规则或行为
# Grammar
## Variables
```lua
global_var = 1
local local_var = "hello"

-- string operations
multi_line = [[multi
line
end
]]
a = tostring(10)
n = tonumber("10")
print("string length is "..#a)

-- bool: 只有nil和false代表假
a=true
b=false
print(1~=2)
print(a and b)
print(a or b)
print(not a)
print(n > 10 and "yes" or "no")
```
### Functions
```lua
function func_name(a, b)
	return b, a
end
```
### Table
```lua
t = {1, "hello", {}, function() end}
t[1] -- table is 1-indexed
table.insert(t, "d") -- 插入到结尾
table.insert(t, 2, "d") -- 从第二个插入
deleted_elem = table.remove(t, 2)

t = {a=1, b=function() end, [",;"]=123123} -- 以字符串为下标
t["a"]
t.b
t[",;"]
t["c"]="world"
```
所有全局变量都在`_G`这个table里，`table`本身也是一个全局table变量
## Flow Control
```lua
if 1>10 then
	print("1>10")
elseif 1<10 then
	print("1<10")
else
	print("no")
end

for i=1,10,2 do
	print(i)
	if i == 5 then break end
end

local n = 10
while n > 1 do
print(n)
	n = n - 1
end
```