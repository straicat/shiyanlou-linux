数学函数：

- `sin(x)`
- `cos(x)`
- `exp(x)`
- `int(x)`
- `rand()`：返回随机数（0<=r<1）

字符串函数：

- `sub(r,s,t)` 在字符串t（默认为`$0`）中用s替换正则表达式r的首次匹配
- `gsub(r,s,t)` 在字符串t（默认为`$0`）中用s替换正则表达式r的所有匹配
- `index(s,t)` 返回t在s中的位置
- `length(s)`
- `match(s,r)` 正则表达式r匹配s，返回出现的起始位置，没出现则返回0
- `split(s,a,sep)` 以sep（默认使用FS）分割s到数组a中，返回元素个数
- `sprintf("fmt", expr)` 格式化字符串
- `substr(s,p,n)` s中从位置p开始最大长度为n的子串；若没给出n返回从p开始剩余子串
- `tolower(s)`, `toupper(s)` 大小写转换，返回新字符串

时间函数：

- `mktime()`  生成时间格式：`mktime("2018 01 01 12 30 40")`
- `strftime()` 格式化时间输出
- `systime()` 得到时间戳，即1970-01-01到现在的秒数

---

`getline` 读取下一行

读取命令输出：

``` awk
# 读取command执行结果到变量output
"command" | getline output;
```

自定义函数：

``` awk
function 函数名(参数){
    内容
    return 返回值
}
```

```
