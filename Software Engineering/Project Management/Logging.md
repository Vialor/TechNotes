> python library `logging`

| 级别       | 说明     | 级别数值 |
| -------- | ------ | ---- |
| DEBUG    | 详细调试信息 | 10   |
| INFO     | 正常运行信息 | 20   |
| WARNING  | 可能发生错误 | 30   |
| ERROR    | 部分功能错误 | 40   |
| CRITICAL | 程序不可运行 | 50   |
相比于`logging.error`, `logging.exception可以打印程序的栈信息
## Basic usage
```python
import logging
logging.basicConfig(
	format="%(asctime)|%(levelname)-8s|%(filename): %(message)", # %(公共信息变量)
	filename="app.log",
	filemode="w",
	level=logging.DEBUG,
)
logging.warning("This is a warning")
```
## Procedural way: OOP
Loggers: write logs
Handlers: store logs
Filters: filter the logs to output
Formatters: format logs
```python
logger = logging.getLogger("app.loggerName")
logger.setLevel
logger.addHandler
logger.removeHandler

streamHandler = logging.StreamHandler(stream)
fileHandler = logging.FileHandler(filename, mode, encoding, delay)
fileHandler.setFormatter
fileHandler.addFilter

formatter = logging.Formatter("%(asctime): %(message)}")

flt = logging.Filter("loggerStartWithThisString")

# usage
logger = logging.getLogger("app.loggerName")
logger.warning("This is a warning")
```
## Declarative way: config files
```python
logging.config.fileConfig('logging.conf')
# OR
logging.config.dictConfig(dictionary) # more flexible than fileConfig since you can accept any format that can be loaded into a python dictionry

rootLogger = logging.getLogger()
logger = logging.getLogger("app.loggerName")
logger.warning("This is a warning")
```
`logging.conf`
```python
[loggers]
keys=root, apilog

[handlers]
keys=fileHandler, consoleHandler

[formatters]
keys=apiFormatter

[logger_root]
level=DEBUG
handlers=consoleHandler

[logger_apilog]
level=DEBUG
handlers=fileHandler
qualname=apilog
propagate=0

[handler_consoleHandler]
class=StreamHandler
args=(sys.stdout,)
level=DEBUG

[handler_fileHandler]
class=handlers.TimedRotatingFileHandler
args=('logs/api.log', 'midnight', 1, 0)
level=DEBUG
formatter=apiFormatter

[formatter_apiFormatter]
format=%(asctime)s|%(levelname)-8s|%(filename)s: %(message)s
datefmt=%Y-%m-%d %H:%M:%S
```