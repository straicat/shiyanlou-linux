if语句：

``` Bash
# 单分支
if 条件 ; then
    操作
fi

# 多分支
if 条件1 ; then
    操作1
elif 条件2 ; then
    操作2
else
    操作3
fi
```

case语句：

``` Bash
# 单分支
case 值 in
值1|值2|值3)
    操作
esac
# 多分支
case 值 in
值11|值12|值13)
    操作1
;;
值21|值22|值23)
    操作2
;;
*)
    操作
esac
```

---

for循环：

``` Bash
# 列表for循环
for 循环变量 in 列表
do
    操作
done
# 不带列表的for循环（遍历$@）
for 循环变量
do
    操作
done
# C语言风格的for循环
for ((i=0;i<10;i++))
do
    操作
done
```

while循环：

``` Bash
while 条件
do
    操作
done
```

until循环：

``` Bash
until 条件
do
    操作
done
```

---

select：

``` Bash
select 变量名 in 菜单取值列表
do
    操作
done
```

``` Bash
PS3="Please select: "
select lan in Java Python Javascript
do
    echo $lan
done
```

