# 采样式
>定期“抓拍”程序当下的调用栈，用统计近似时间分布。    
## py-spy
```bash
pip install py-spy
```
运行你的程序（不一定要 debug）后，另开终端：
```bash
py-spy top --pid <PID>
```
或直接抓一个堆栈：
```bash
py-spy dump --pid <PID>
```
你会看到程序时间主要花在哪些函数上，I/O 等待通常也会在栈里显现（比如 socket/ssl/urllib3 等）。
## py-spy火焰图
``` bash
# 对脚本直接录制火焰图
py-spy record -o profile.svg --format flamegraph -- python your_script.py
# 看正在运行的进程
py-spy record -o profile.svg --format flamegraph -p <PID>
```
# 插桩式
## cProfile 
>在函数/行的入口与出口“打点记账”，逐次记录精确耗时与次数。
```bash
python -m cProfile -o prof.out your_script.py
python -m pstats prof.out
```
在 pstats 里输入：
- `sort cumtime`
- `stats 50`
你会看到耗时最高的调用路径。I/O 往往会体现为某些函数 cumtime 特别大。
（注意：如果大量时间在“等待”，profile 有时不会像 CPU 计算那样直观，但仍然常能定位到调用入口。）
## viztracer火焰图