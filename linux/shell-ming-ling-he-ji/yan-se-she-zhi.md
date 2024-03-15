# 颜色设置

### 颜色设置示例

```bash
echo -e "\033[41;37;5m Hello \033[0m world"

echo -e "\E[41;37;5m Hello \E[0m world"

echo -e "\e[41;37;5m Hello \e[0m world"

# \033 等同于 \E 等同于 \e
```

<figure><img src="../../.gitbook/assets/color.gif" alt=""><figcaption></figcaption></figure>

以上命令设置背景成为红色，前景白色，闪烁光标，输出字符 "Hello"，然后重新设置屏幕到缺省设置，输出字符 "world"。"-e"是命令 echo 的一个可选项，它用于激活特殊字符的解析器。"\033"引导非常规字符序列。"m"意味着设置属性然后结束非常规字符序列，这个例子里真正有效的字符是 "41;37;5" 和"0"。修改"41;37;5"可以生成不同颜色的组合，数值和编码的前后顺序没有关系。



### 颜色编码

| 颜色 | 文字颜色（前景色）编码 | 背景色编码 |
| -- | ----------- | ----- |
| 黑  | 30          | 40    |
| 红  | 31          | 41    |
| 绿  | 32          | 42    |
| 棕  | 33          | 43    |
| 蓝  | 34          | 44    |
| 紫红 | 35          | 45    |
| 青  | 36          | 46    |
| 白  | 37          | 47    |

{% hint style="info" %}
更多信息可查看帮助手册 ：**man console\_codes**
{% endhint %}



### 脚本示例

```bash
#!/bin/bash

#-------- color --------
if [ -t 1 ]; then # is terminal?
    RED="\E[0;31m"
    RED_BOLD="\E[1;31m"
    GREEN="\E[0;32m"
    GREEN_BOLD="\E[1;32m"
    BROWN="\E[0;33m"
    BROWN_BOLD="\E[1;33m"
    BLUE="\E[0;34m"
    BLUE_BOLD="\E[1;34m"
    MAGENTA="\E[0;35m"
    MAGENTA_BOLD="\E[1;35m"
    CYAN="\E[0;36m"
    CYAN_BOLD="\E[1;36m"
    WHITE="\E[0;37m"
    WHITE_BOLD="\E[1;37m"
    RESET="\E[0m"
fi


echo -e "${RED}Hello${RESET}"
echo -e "${RED_BOLD}Hello${RESET}"
echo -e "${GREEN}Hello${RESET}"
echo -e "${GREEN_BOLD}Hello${RESET}"
echo -e "${BROWN}Hello${RESET}"
echo -e "${BROWN_BOLD}Hello${RESET}"
echo -e "${BLUE}Hello${RESET}"
echo -e "${BLUE_BOLD}Hello${RESET}"
echo -e "${MAGENTA}Hello${RESET}"
echo -e "${MAGENTA_BOLD}Hello${RESET}"
echo -e "${CYAN}Hello${RESET}"
echo -e "${CYAN_BOLD}Hello${RESET}"
echo -e "${WHITE}Hello${RESET}"
echo -e "${WHITE_BOLD}Hello${RESET}"
```

![](<../../.gitbook/assets/image (61).png>)



### man手册

```
# man console_codes

       param   result
       0       reset all attributes to their defaults
       1       set bold
       2       set half-bright (simulated with color on a color display)
       4       set  underscore (simulated with color on a color display)
               (the colors used to simulate dim  or  underline  are  set
               using ESC ] ...)
       5       set blink
       7       set reverse video
       10      reset  selected mapping, display control flag, and toggle
               meta flag (ECMA-48 says "primary font").
       11      select null mapping, set display control flag, reset tog‐
               gle meta flag (ECMA-48 says "first alternate font").
       12      select null mapping, set display control flag, set toggle
               meta flag (ECMA-48 says "second  alternate  font").   The
               toggle meta flag causes the high bit of a byte to be tog‐
               gled before the mapping table translation is done.
       21      set normal intensity (ECMA-48 says "doubly underlined")
       22      set normal intensity
       24      underline off
       25      blink off
       27      reverse video off
       30      set black foreground
       31      set red foreground
       32      set green foreground
       33      set brown foreground
       34      set blue foreground
       35      set magenta foreground
       36      set cyan foreground
       37      set white foreground
       38      set underscore on, set default foreground color
       39      set underscore off, set default foreground color
       40      set black background
       41      set red background
       42      set green background
       43      set brown background
       44      set blue background
       45      set magenta background
       46      set cyan background
       47      set white background
       49      set default background color
```

