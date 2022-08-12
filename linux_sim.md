### vivado/xili_IP+vcs+uvm+verdi

https://mbb.eet-china.com/blog/1854387-424639.html

file-export-exportsimulation从vivado导出vcs工程脚本和源代码,

改tb.sh，simulate.do,构建makefile，调用verdi

vcs编译过后可以产生simv.daidir这个文件夹，其中存放的是包含了编译信息的中间数据，这样verdi就可以用这个中间数据直接导入编译后的工程，不需要再手动导入filelist重新编译一边。

```shell
***************tb.sh增加编译选项，编译文件前先编译uvm
# Command line options
vlogan_opts="-full64 -kdb -ntb_opts uvm-1.1 "
vhdlan_opts="-full64"
vcs_elab_opts="-full64 -kdb -ntb_opts uvm-1.1 -fsdb -debug_pp -t ps -licqueue -l elaborate.log"
vcs_sim_opts="-ucli -licqueue -l simulate.log"
...
# RUN_STEP: <compile>
compile()
{
  # Compile design files
  vlogan $vlogan_opts -sverilog
... 
******************simulate.do删除该文件的内容

******************Makefile
verdi:
	verdi -full64 -sv -ssf sim.fsdb -dbdir ./sim_tb_top_simv.daidir 
```

对于仿真过程，在不对tb的shell文件传参的情况下，脚本会默认运行creat_lib_mappings函数，构建synopsys_sim.setup文件，再执行creat_lib_dir函数创建运行lib，vcs再elab过程中会用到synopsys_sim.setup中的库，其中使用OTHERS=/home/ICer/fpgaproj/vcs_xili_lib/synopsys_sim.setup链接到提前编译好的xili库。对于调用的xili的IP,比如fifo的ip，需要编译如下等文件

```shell
/kcu_migtiming.srcs/sources_1/ip/*fifo*/sim/*fifo*.v
```



###  covergroup report

https://xueying.blog.csdn.net/article/details/107168875

https://blog.csdn.net/cy413026/article/details/113879302

```verilog
urg -dir simv.vdb/
cd urgReport/
firefox dashboard.html

merge:
        @echo ${DB_FILE}
        urg -full64 -dir ${DB_FILE} -dbname ${PATH}/merged.vdb  
cov:
        bp dve -full64 -cov -covdir ${PATH}/merged.vdb
```



### vcs编译过程makefile

https://blog.csdn.net/Starry__/article/details/124077835

```makefile
FLIST=vcs.f

vcs:
	vcs -full64 -sverilog -ntb_opts uvm-1.1 -ntb_opts svp -fsdb -debug_all -timescale=1ns/1ps \
	-F $(FLIST) -l vcs_comp.log
	./simv -timescale=1ns/1ps -l vcs_run.log

verdi:
	verdi -full64 -sv -F $(FLIST) -ssf sim.fsdb &

clean:
	rm -rf simv*
	rm -rf csrc ucli* vc_hdrs* vcs_*
	rm -rf novas* *.fsdb verdi*
#若使用uvm，需要加上-ntb_opts uvm-1.1 -ntb_opts svp，-fsdb
initial begin//在top_tb.sv中加入此段代码以生成fsdb文件，并在makefile的verdi下注明打开改波形文件
  $fsdbDumpfile("sim.fsdb");//生成波形文件
  $fsdbDumpvars(0,top_tb,"+all");//确定将top_tb下的所有信号都进行dump
end
```

#### 一、Analysis

代码如下所示，make vlog分析verilog代码，make vhdl分析vhdl的代码。

```bash
vlog:
	vlogan -l vlogan.log -sverilog -assert svaext +v2k -loc -F verilog.f
vhdl:
	vhdlan -l vhdlan.log -F vhdl.f
	
```

#### 二、Elaboration

代码如下所示，make uvm分析并生成uvm的库文件；make testbench分析并生成顶层testbench的库文件；make elab编译生成simv文件，其中如果环境中用到了VIP，需要指定VIP自带的so库文件。

```bash
uvm:
	vlogan -l uvm.log -ntb_opts uvm -full64 +define+UVM_NO_DPI +define+UVM_NO_DEPRECATED

testbench:
	vlogan -l testbench.log -ntb_opts uvm -full64 -sverilog -debug_pp +define+UVM_NO_DPI +define+NSYS_UVM +plusarg_save +v2k -lca -F tb.f

elab:
	vcs -l elab.log -full64 -debug_pp -debug_all -lca -fsdb -timescale=1ns/1ps ./vip/common/latest/C/lib/amd64/VipCommonNtb.so -top ${top_name}
```

其中tb.f文件如下所示:

```bash
./top_tb.sv
-F ./vip/latest/sim/vip.f    #vip的filelist
```

其中top_tb.sv文件如下所示：

```cpp
`include "uvm_pkg.sv"
`include "vip.sv"     //VIP的env文件

module top_tb();
    `include "uvm_macros.svh"
    import uvm_pkg::*;

    `include "vip_testbench.sv"  //VIP的testbench文件

	***
	***
	***
	
endmodule
```

#### 三、Simulation

代码如下所示，执行仿真。

```bash
run:
	./simv -l run.log +UVM_VERBOSITY=UVM_FULL +UVM_TESTNAME=${testname}
	
```

### tips

comp elab run ,把vcs的home改为了vcs-mx

直接启用Verdi运行即，./simv -verdi ，Verdi中加上dump -add /* -depth 0

 ./simv \${SOPT} +ntb_random_seed=19
 ./simv ${SOPT} +ntb_random_seed_automatic

行覆盖率  line coverage 要求百分之99-100

状态机覆盖率 FSM coverage

条件覆盖率 conditional coverage

翻转覆盖率 Toggle coverage ：0->1 ,1->0

路径覆盖率 Path coverage ：initial 和always里的语句

分支覆盖率 branch coverage

**-cm line+cond+fsm+branch+tgl**为生成什么条件的覆盖率