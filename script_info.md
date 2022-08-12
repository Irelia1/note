shell
-----

https://blog.csdn.net/superbfly/article/details/49274889

文件表达式
-e filename 如果 filename存在，则为真
-d filename 如果 filename为目录，则为真 
-f filename 如果 filename为常规文件，则为真

perl
----

Perl 是一种弱类型语言，所以变量不需要指定类型，Perl 解释器会根据上下文自动选择匹配类型。

Perl 有三个基本的数据类型：标量、数组、哈希。以下是这三种数据类型的说明：

| **序号** | **类型和描述**                                               |
| -------- | ------------------------------------------------------------ |
| 1        | **标量**标量是 Perl 语言中最简单的一种数据类型。这种数据类型的变量可以是数字，字符串，浮点数，不作严格的区分。在使用时在变量的名字前面加上一个$，表示是标量。例如：`$myfirst=123;　#数字123　$mysecond=“123”;#字符串123　` |
| 2        | **数组**数组变量以字符@ 开头，索引从 0 开始，如：@arr=(1,2,3)`@arr=(1,2,3)` |
| 3        | **哈希**哈希是一个无序的key/value 对集合。可以使用键作为下标获取值。哈希变量以字符% 开头。`%h=(‘a’=>1,‘b’=>2);` |

YASA
----

*YASA* - Yet Another Simulation Architecture

YASA是一款跑IC软件仿真的开源框架。它支持synopsys vcs和cadence irun。支持synopsys 2-step或者3-step的仿真flow。它支持SV/UVM或者纯verilog的testbench，支持lsf作业调度系统。
它提供了一系列灵活的配置选项。用户可配置的文件有三个：userCli.cfg，build.cfg，group.cfg。

https://blog.csdn.net/zhajio/article/details/88943669