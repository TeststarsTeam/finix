特殊赋值类型处理
===================
在处理不同类型的时候，会遇到一些特殊操作的类型，例如void*、函数指针、系统类型等特殊的类型，针对这些特殊的类型，wings提供了不同的操作来进行特殊处理。


函数参数为void *与函数指针
-----------------
针对函数参数为void*与函数指针的类型，wings首先会利用静态分析技术，获取函数参数为void *与函数指针时的具体赋值类型。例如下所示函数：

::

	void func(void *p);
	callFunc(int *p);
	int callFunc(int *p)
	{
	  char *s ="abc";
	  func(s);
	  return 0;
	}
	int func(int(*f)(int));
	void functest();
	void functest()
	{
	 func(fun);
	}


Wings在静态解析时，会分析到func在被调用处的赋值类型为char *，赋值过程中将会对func的参数赋值为char *，wings在数据表格界面会标记void *处的具体赋值类型。Wings在静态分析过程中，会解析到func在被调用出的赋值函数为fun，在赋值过程中将会对func的参数赋值为fun。

针对一些利用分析技术无法确定的类型，wings会将所有有关的函数展示在界面上，由用户自己选择需要的类型，驱动会处理相对应的类型。如下图所示：

.. image:: /image/figure23.png


结构体链表
----------------
针对链表类型，采用比较灵活的赋值方式，考虑到实际应用中的一些因素，我们针对链表类型，默认赋值两层结构，在实际测试过程中，用户可依据需要自动添加节点。

.. image:: /image/figure24.png


结构体、类为系统变量
----------------
wings针对一些c++标准库的头文件，以及一些第三方库的，做特殊模版处理。下图中，是一些特殊系统变量的定义。

.. image:: /image/figure25.png

**举例说明**

假设遇到特殊类型的赋值，比如下图中的sockaddr_in,首先讲此类型标记为系统头文件中的类型，需要特殊处理。

.. image:: /image/figure26.png

sockaddr_in中成员变量需要用户自定义赋值
* 首先wings检测到sockaddr_in需要特殊处理，会将此变量显示在模板类的界面

* 然后针对需要特殊处理的变量，例如sin_family等，需要用户针对需要的类型进行配置。

* 配置完之后，返回对应的sockaddr_in的结构体对象即可。

* 生成驱动代码的时候，会调用用户编写的函数，而需要填写的变量，例如sin_family值有我们自动生成。

wings针对特殊模版，有简单的界面进行配置处理即可。

.. image:: /image/figure27.png


stl标准模版库
----------------
针对c++中的标准容器，我们将容器中的类型进行展开赋值，默认生成一组值，可以依据需要，在界面上点击进行添加即可。

.. image:: /image/figure28.png

c++自定义模版类
----------------
针对参数为自定义的模板类型，我们会分析模板类的具体类型，例如下图中GetTest中模板类的类型为int与double，GetTestDemo中类型为std::string与double，我们针对模板类同样插入一个构造函数CustomTemplateClass，实际构造模板类对象的时，调用具体的赋值类型进行构造。

.. image:: /image/figure29.png

