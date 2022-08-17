linux安装XDMAhttps://zhuanlan.zhihu.com/p/483896215

xdma的简单使用http://xilinx.eetrend.com/content/2021/100113259.html

基于Xilinx FPGA 的PCIE开发教程（1）https://zhuanlan.zhihu.com/p/493072708

pcie xdma实例5 https://www.cnblogs.com/yuzeren48/p/13755651.html

pcie xdma ddr读写https://copyfuture.com/blogs-details/20211201195633646k

Block Design的方式，适用于快速构建比较复杂的设计，例如包含DDR4，Datamover等各种基于AXI互联的IP。

在win下使用pcie，xdma，首先将mcs或bin加载到QSPI Flash，因为Pcie需要电脑关机重启来识别设备，所以需要固化程序保证断电不丢失。并且需要吧windows的快速启动关闭让其扫描pcie设备。对于windows的xdma驱动安装，使用管理员权限开启powershell，输入bcdedit /set testsigning on再重启电脑开启测试模式，再安装证书和驱动，并使用预编译的例程进行测试，但是在linux系统下，需要编译。

### pcie

驱动安装

