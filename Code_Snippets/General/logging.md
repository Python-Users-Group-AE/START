# Logging

Logging can be pretty useful either to track automatic code executions or to debug a faulty run. Following method will create `.log` file in the script directory and keep track of whatever you wish to document. 

Contents of `logger.py` (Place this in the same folder as  your script): 

```python 
import inspect 
import logging 
import getpass 
from pathlib import Path 
import time 

class Log(object): 
    def __init__(self): 
        user=getpass.getuser() 
        self.logger=logging.getLogger(user) 
        self.logger.setLevel(logging.DEBUG) 
        format='%(asctime)s-%(levelname)s: %(message)s' 
        formatter=logging.Formatter(format, datefmt='%Y%m%d-%H%M%S') 
        streamhandler=logging.StreamHandler() 
        streamhandler.setFormatter(formatter) 
        self.logger.addHandler(streamhandler) 
        log_dir: Path = Path(__file__).parent / 'logs' 
        log_dir.mkdir(exist_ok=True) 
        logfile = log_dir / f'{user}{time.strftime("-%Y-%b")}.log' 
        self.logfile = logfile 
        filehandler=logging.FileHandler(logfile, encoding="utf-8") 
        filehandler.setFormatter(formatter) 
        self.logger.addHandler(filehandler) 
         
    def debug(self, msg): 
        self.logger.debug(msg) 
         
    def info(self, msg): 
        self.logger.info(msg) 
         
    def warning(self, msg): 
        try: 
            suffix = f'Warning in {Path(inspect.stack()[1].filename).name} -> {inspect.currentframe().f_back.f_code.co_name}: ' 
        except Exception: 
            suffix = f'Warning in {Path(inspect.stack()[1].filename).name}: ' 
        self.logger.warning(suffix + msg) 
         
    def error(self, msg): 
        try: 
            suffix = f'Error in {Path(inspect.stack()[1].filename).name} -> {inspect.currentframe().f_back.f_code.co_name}: ' 
        except Exception: 
            suffix = f'Error in {Path(inspect.stack()[1].filename).name}: ' 
        self.logger.error(suffix + str(msg)) 
        self.logger.exception(msg) 
         
    def critical(self, msg): 
        try: 
            suffix = f'Critical in {Path(inspect.stack()[1].filename).name} -> {inspect.currentframe().f_back.f_code.co_name}: ' 
        except Exception: 
            suffix = f'Error in {Path(inspect.stack()[1].filename).name}: ' 
        self.logger.critical(suffix + str(msg)) 
        self.logger.exception(msg) 
         
    def log(self, level, msg): 
        self.logger.log(level, msg) 
         
    def setLevel(self, level): 
        self.logger.setLevel(level) 
         
    def disable(self): 
        logging.disable(50) 
         
    def contents(self, lines: int=9999) -> list[str]: 
        with open(self.logfile, 'r') as f: 
            data = f.readlines() 
            _len = len(data) 
            return data[_len - min(lines, _len):]         
log = Log() 
```

Now in your actual script (say, `script.py`):

```python  
from logger import log 
 
# You can now use `log` instead of `print` 
log.info("This will be logged as an INFO") 
log.debug("This will be logged as an DEBUG") 
log.warning("This will be logged as an WARN") 
log.error("This will be logged as an ERROR") 
```