**REPL/Language Shell**: an _interactive_ programing environment  
  
_interactive_: **read evaluate print loop**
  
**Formatter**: automatically adjust the code's formatting to meet specific style guidelines  
JavaScript:  
`prettier` Python: `black`
- **Static Analyzer**:
    **Linter**: enforce coding standards, promoting best practices and consistency (also capture some styling issue; comparing to formatters, linters focus more on code logic)
    English: `writegood`
    JavaScript: `eslint`
    Python: `pyflakes`
    
- **Dynamic Analyzer**:
    **Debugger**:  
    Python:  
    `ipdb` C: `gdb`
    **Profiler**: collects program parameters while it is running  
    Python: profiler, memory_profiler, flame graph  
    C: valgrind  
    Linux: perf, htop, du(ncdu), lsof