---
tags: 
alias:
---


# 使用
```shell
adb shell simpleperf record -e cpu-clock -g --duration 10 -o /data/local/tmp/perf.data --app com.wejoy.weplay.jp


./stackcollapse-perf.pl /Users/wepalzhangjin/Desktop/Trace/perf.data > /Users/wepalzhangjin/Desktop/Trace/out.folded

./flamegraph.pl /Users/wepalzhangjin/Desktop/Trace/out.folded > /Users/wepalzhangjin/Desktop/Trace/flamegraph.svg

```