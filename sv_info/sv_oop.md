2.类中的静态方法可以声明并使用动态变量，但是不能使用类的动态成员变量
静态方法可以使用类的静态变量

3.如果没有指明访问类型，那么成员的默认类型是public，子类和外部均可以访问成员
如果指明了访问类型是protected，那么只有该类和子类可以访问成员，而外部无法访问
如果指明了访问类型是local，那么只有该类可以访问成员，子类和外部均无法访问

4.子类在定义new函数时，应该首先调用父类的new函数（super.new（））。如果父类的new函数没有参数，则可以省略，系统在编译时会自动添加。

5.对象创建初始化顺序，有如下规则：
子类的实例对象在初始化时首先会调用父类的构造函数
当父类构造函数完成时，会将子类实例对象中的各个成员变量按照它们定义时显示的默认值初始化，如果没默认值则不被初始化
在成员变量默认值赋予后，才会最后进入用户定义的new函数中执行剩余的初始化代码

```verilog
module tb4 (
);
class   Transaction;
	static int count=0;	//已创建的对象的数目
	int id;						//实例的唯一标志
	function   new();
		id=count++;			//设置标志，count递增
	endfunction
  static function void many();
    int temp=123;
    $display("temp=%0d",temp);
    $display("count=%0d,id=%0d",count ,id);
  endfunction
endclass

Transaction  t1,t2;
initial  begin
  #6;
	t1=new();			
	t2=new();
  t1.many();
  t2.many();
	$display("Second id=%d,count=%d",t1.id,t1.count);					//使用句柄来引用静态变量
	$display("Second id=%d,count=%d",t2.id,Transaction::count);			//类名加::即类作用域操作符
end
  
endmodule
/*
Error-[SV-AMC] Non-static member access
test.sv, 122
tb4, "id"
  Illegal access of non-static member 'id' from static method 
  'Transaction::many'.

2 warnings
1 error
```

8.如果在一个块内使用了一个未声明的变量，碰巧在程序块中有一个同名的变量，那么类就会使用程序块中的变量，不会给出警告。如果你将类移到一个package中，类就看不到程序一级的变量了

9.编译一个类（包含一个尚未被定义的类时），你需要使用typedef语句声明被包含的类名，否则会引起错误

11.创建一个对象，并多次传送给设计，会导致错误

12.句柄数组,不存在“对象数组”的说法