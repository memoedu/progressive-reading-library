用于在控制台上打印彩色信息，在复杂系统调试中屡试不爽。代码如下：

```python
from enum import Enum
from colorama import Back, Style

class ReportType(Enum):
    Info = 'info'
    Debug = 'debug'
    Warn = 'warn'
    Error = 'error'


def color_report(message: str, type: ReportType = ReportType.Info):
    flag = ''
    if type == ReportType.Warn:
        flag = Back.YELLOW
    elif type == ReportType.Error:
        flag = Back.RED
    elif type == ReportType.Info:
        flag = Back.GREEN
    elif type == ReportType.Debug:
        flag = Back.BLUE
    tag = '[{}]'.format(type.value)
    print(flag, tag, Style.RESET_ALL, message)
```

下面是使用：

```python
# 默认情况
color_report("hello")

# 调试信息
color_report("hello", ReportType.Debug)

# 警告信息
color_report("hello", ReportType.Warn)

# 错误信息
color_report("hello", ReportType.Error)
```

控制台打印：

<div align=center>
<img src="https://picx.zhimg.com/80/v2-130e1f27b696684149db93447d1835d8_1440w.png" style="width: 40%;"/>
<p class="img-caption">console output</p>
</div>