#### FontColorUtil工具类

    /**
    *
    * 控制台打印彩色字体
    * 语法格式: \033[44;37;5m ME \033[0m COOL
    *
    */
    public class FontColorUtil {

        private static FontColor defaultFontColor = FontColor.WHITE;

        /**
        * @param text
        */
        public static void print(String text)
        {
            print(text, defaultFontColor);
        }

        /**
        * @param text
        */
        public static void println(String text)
        {
            println(text, defaultFontColor);
        }

        /**
        * @param text
        * @param fontColor
        */
        public static void print(String text, FontColor fontColor)
        {
            printInternal(text, fontColor == null?defaultFontColor:fontColor, false);
        }

        /**
        * @param text
        * @param fontColor
        */
        public static void println(String text, FontColor fontColor)
        {
            printInternal(text, fontColor == null?defaultFontColor:fontColor, true);
        }

        /**
        * @param text
        * @param fontColor
        * @param newLine
        */
        private static void printInternal(String text, FontColor fontColor, boolean newLine)
        {
            if(text == null || text.trim().length() == 0)
            {
                throw new IllegalArgumentException("text can't empty!");
            }
            if(illegalColor(fontColor))
            {
                throw new IllegalArgumentException("illegal Color!");
            }

            String finalText = "";
            switch (fontColor)
            {
                case RED:
                    finalText = "\033[31m"+text+"\033[0m";
                    break;
                case GREEN:
                    finalText = "\033[32m"+text+"\033[0m";
                    break;
                case WHITE:
                    finalText = "\033[37m"+text+"\033[0m";
                    break;
                case YELLOW:
                    finalText = "\033[33m"+text+"\033[0m";
            }

            if(newLine)
            {
                System.out.println(finalText);
            }
            else
            {
                System.out.print(finalText);
            }
        }

        /**
        * @param fontColor
        * @return
        */
        private static boolean illegalColor(FontColor fontColor)
        {
            for (FontColor fc : FontColor.values()) {
                if(fc.equals(fontColor))
                {
                    return false;
                }
            }
            return true;
        }
    }

    /**
    * 颜色定义
    */
    enum FontColor
    {
        RED, YELLOW, GREEN, WHITE
    }

#### test case.

    FontColorUtil.println("this is a crazy world...");
    FontColorUtil.println("this is a crazy world...", FontColor.WHITE);
    FontColorUtil.println("this is a crazy world...", FontColor.GREEN);
    FontColorUtil.println("this is a crazy world...", FontColor.RED);
    FontColorUtil.println("this is a crazy world...", FontColor.YELLOW);

---

### principle

#### 终端字符颜色设置

    终端的字符颜色是用转义序列控制的,是文本模式下的系统显示功能,和具体的语言无关,shell,python,perl,C,C++等均可以调用
    转义序列是以ESC开头,可以用 \033 完成相同的工作(ESC 的 ASCII 码用十进制表示就是 27,等价于用八进制表示的 33)

#### 显示格式

    \033[显示方式;前景色;背景色;动作m;

    其中显示方式,前景色或背景色无顺序关系,其中任何一项也不是必须的
    系统默认颜色：
        \033[0m
    文本终端的颜色可以使用“ANSI非常规字符序列”来生成,举例：
        echo -e "\033[44;37;5m ME \033[0m COOL"
    以上命令设置背景成为蓝色,前景白色,闪烁光标,输出字符“ME”,然后重新设置屏幕到缺省设置,输出字符“COOL”

    “e”是命令 echo的一个可选项,它用于激活特殊字符的解析器
    “\033”引导非常规字符序列
    “m”意味着设置属性然后结束非常规字符序列
    这个例子里真正有效的字符是“44;37;5” 和“0”

    修改“44;37;5”可以生成不同颜色的组合,数值和编码的前后顺序没有关系

#### 可以选择的编码

    编码 颜色/动作
    0 重新设置属性到缺省设置
    1 设置粗体
    2 设置一半亮度（模拟彩色显示器的颜色）
    4 设置下划线（模拟彩色显示器的颜色）
    5 设置闪烁
    7 设置反向图象
    22 设置一般密度
    24 关闭下划线
    25 关闭闪烁
    27 关闭反向图象
    30 设置黑色前景
    31 设置红色前景
    32 设置绿色前景
    33 设置棕色前景
    34 设置蓝色前景
    35 设置紫色前景
    36 设置青色前景
    37 设置白色前景
    38 在缺省的前景颜色上设置下划线
    39 在缺省的前景颜色上关闭下划线
    40 设置黑色背景
    41 设置红色背景
    42 设置绿色背景
    43 设置棕色背景
    44 设置蓝色背景
    45 设置紫色背景
    46 设置青色背景
    47 设置白色背景
    49 设置缺省黑色背景

#### 其他有趣的代码还有

    \033[2J 　清除屏幕
    \033[0q 　关闭所有的键盘指示灯
    \033[1q 　设置“滚动锁定”指示灯 (Scroll Lock)
    \033[2q 　设置“数值锁定”指示灯 (Num Lock)
    \033[3q 　设置“大写锁定”指示灯 (Caps Lock)
    \033[15:40H 把关闭移动到第15行，40列
    \007 　　发蜂鸣生beep

#### 字背景颜色范围:40----49 

    40:黑 
    41:深红 
    42:绿 
    43:黄色 
    44:蓝色 
    45:紫色 
    46:深绿 
    47:白色 

#### 字颜色:30-----------39
 
    30:黑 
    31:红 
    32:绿 
    33:黄 
    34:蓝色 
    35:紫色 
    36:深绿 
    37:白色 

---

[给终端文字加点颜色和特效](https://mozillazg.com/2013/08/ansi-escape-sequences.html)

