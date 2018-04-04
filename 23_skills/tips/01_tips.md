# tips -01

[TOC]

## bash shell colors



bash shell定义文字颜色有三个参数: ==style==, ==frontground==, 和==background==, 每个参数有7个值，意义如下:

其中, +30表示前景色，+40表示背景色

| color           | foreground | background |
| --------------- | ---------- | ---------- |
| Black           | 30         | 40         |
| Red             | 31         | 41         |
| Green           | 32         | 42         |
| Yellow          | 33         | 43         |
| Blue            | 34         | 44         |
| Magenta（品红） | 35         | 45         |
| Cyan（青）      | 36         | 46         |
| White           | 37         | 47         |
|                 |            |            |

添加颜色到终端的语法:

- \e[ : start color scheme.
- x;y: color pairs to use(x;y)
- $P1: your shell prompt variable.
- \e[m: stop color scheme.

```shell
# set a red color prompt
export PS1="\e[0;31m[\u@\h \W]\$ \e[m "
```





```shell
#/bin/bash
for STYLE in 0 1 2 3 4 5 6 7; do
  for FG in 30 31 32 33 34 35 36 37; do
    for BG in 40 41 42 43 44 45 46 47; do
      CTRL="\033[${STYLE};${FG};${BG}m"
      echo -en "${CTRL}"
      echo -n "${STYLE};${FG};${BG}"
      echo -en "\033[0m"
    done
    echo
  done
  echo
done
# Reset
echo -e "\033[0m"
```

