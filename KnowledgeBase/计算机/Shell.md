---
tags: 
alias:
---

操作计算机能力的文本接口，常见的Shell有Bourne Again Shell, 简称 “bash”

https://missing-semester-cn.github.io/2020/command-line/
## 任务控制

### 结束进程

 shell 会使用 UNIX 提供的信号机制执行进程间通信。当一个进程接收到信号时，它会停止执行、处理该信号并基于信号传递的信息来改变其执行。就这一点而言，信号是一种_软件中断_。

当我们输入 `Ctrl-C` 时，shell 会发送一个`SIGINT` 信号到进程。但是如果我们在程序中手动处理SIGINT信号，则它不能实现关闭进程的功能：

```
#!/usr/bin/env python
import signal, time

def handler(signum, time):
    print("\nI got a SIGINT, but I am not stopping")

signal.signal(signal.SIGINT, handler)
i = 0
while True:
    time.sleep(.1)
    print("\r{}".format(i), end="")
    i += 1
```

为了停止上面的程序，我们需要使用`SIGQUIT` 信号，通过输入`Ctrl-\`可以发送该信号。除此之外还有Kill命令： `kill -TERM <PID>`

