#### $timeformat

的语法如下：

```verilog
 $timeformat(units_number, precision_number, suffix_string, minimum_field_wdith);
```

其中：

units_number 是 0 到-15 之间的整数值，表示打印的时间值的单位：0 表示秒，-3 表示毫秒，-6 表示微秒，-9 表示纳秒， -12 表示皮秒， -15 表示飞秒；中间值也可以使用：例如-10表示以100ps为单位。其默认值为`timescalse所设置的仿真时间单位。
precision_number 是在打印时间值时，小数点后保留的位数。其默认值为0。
suffix_string 是在时间值后面打印的一个后缀字符串。其默认值为空字符串。
MinFieldWidth 是时间值字符串与后缀字符串合起来的这部分字符串的最小长度，若这部分字符串不足这个长度，则在这部分字符串之前补空格。其默认值为20。

#### $fsdbDumpvars

有三个参数：depth，scope和parameter。下面重点记录下各个parameter

depth：用来设置hierarchy tree层级结构的最大深度

scope：用来设置从哪个scope开始

parameter：用来控制某些特殊功能的打开或者关闭

"+functions"
Set to enable dumping the signals in function and task

"+Reg_Only"
Only reg type signals are dumped

"+packedmda"
dumps the packed signals in the desing. see example in table above

"+all"
dumps all

"+struct"
Dumps all structs in all scopes specified in fsdbDumpvars or in the entire design if no scope is specified

#### \$value\$plusargs（）

这个函数，前一个参数是要传变量的格式（%s），后一个参数是要传的变量具体是谁（Pengyuyan），括号里的 BLOGGER_NAME_IS =  和验证平台中的 +BLOGGER_NAME_IS =  需要保持一致。

函数就是去验证平台中拿 BLOGGER_NAME_IS = Pengyuyan ，然后把Pengyuyan 传递进去到验证平台中去。

再来说说它的兄弟：

\$test\$plusargs(string)

举个例子：

if( $test$plusargs ( “BLOGGER_IS_COOL”) )

$display(“blogger is so cool !”);

else

$display(“blooger is handsome !”);

在验证环境里加上 +BLOGGER_IS_COOL 

验证平台会打印出来啥呢？

打印出来  blogger is so cool !

\$test​\$plusargs(string) 意味着我们可以通过外面的标记修改平台的逻辑，比如我想给某个代码加个使能。

**\$test\$plusargs()其实是由括号里往外匹配，只要外面传的命令\**有相同的字符串\**，就算匹配成功！这个字符串可能是外面命令完整的字符串，也可能是外传字符串的\**子串\**！**

1、\$display,\$write用于信息的显示和输出。其中，

   %b或%B  二进制

   %o或%O  八进制

   %d或%D  十进制

   %h或%H  十六进制

   %e或%E  实数

   %c或%C  字符

   %s或%S  字符串

   %v或%V  信号强度

   %t或%T  时间

   %m或%M  层次实例

​    \n  换行

​    \t  制表符

​    \\  反斜杠\

​    \"  引号”

​    \%%  百分号%

调用方式：eg：$display("%b+%b=%b",a,b,sum);

​             $write("%b+%b=%b",a,b,sum);

注：如果没有在指定变量的显示格式，不会输出数值。如果没有指定变量显示的位置，变量值会在字符串部分之后直接显示出来，变量之间是没有间隔的，只是一次简单的显示。

显示任务$display默认显示的格式是十进制的，还有$displayb,$displayo,$displaybh的显示格式分别是二进制，八进制，十六进制。同理有$write，$writeo，$writeb，$writeh。

$display与$write的区别是：$display会在每次显示信息后自动换行，$write不会换行。

2、$strobe探测任务

探测任务的语法和显示任务完全相同，也是把信息显示出来。也有$strobe,$strobeb,$strobeo,$strobeh四种。

两者的区别在于：$strobe命令会在当前时间部结束时完成；而$display是只要仿真器看到就会立即执行。

3、$monitor监测任务

监测任务用于持续监测指定变量，只要这些变量发生了变化，就会立即显示对应的输出语句。

eg：

initial

begin

$monitor("x=%b,y=%b,cin=%b",x,y,cin);

end

同理，有$monitor,$monitorb$monitoro$monitorh。

可用$monitoroff,monitoeron关闭监事和打开监视。

4、$stop,$finish仿真控制任务

区别：$stop暂停当前方针，$finish中值当前方针。

$size（x，1） 查看size大小，参数，1第一个维度，2为第二个维度 