c++驱动代码 
=============================================
c++中主要是针对每个类进行测试，wings会自动生成每个类的驱动代码以及对应的gtest代码。


 c++驱动代码的命名规则
-----------------------

每个类生成驱动对应的文件名为：driver+类名.cc 以及.h，驱动类的名字为：Driver+ClassName(类名)，c++中存在很多运算符重载的函数，为了保证函数的唯一性，针对类的函数从0开始进行编号处理，即Driver+函数名+编号。


c++ 驱动代码的详细介绍 
-----------------------

c++中会针对每个类生成对应的测试类代码，如下图1所示，Rectangle为被测试类，包含3个成员变量的类型分别为 Point、 int 、std::string,假设Rectangle作为函数参数，则此类的对象构造需要对3个成员变量进行赋值操作，wings会自动针对每个类在源代码中插入一个构造函数如下图2所示，对成员变量进行赋值操作。

.. image:: /image/figure14.png

.. image:: /image/figure15.png

针对每个驱动类，对应的会生成一个构造函数，来初始化被测试类，如下图所示：

.. image:: /image/figure16.png

驱动类中，针对每个函数生成一个驱动函数。如下图所示：

.. image:: /image/figure17.png

每个驱动类，对应一个gtest调用类，来进行期望的对比，如下图所示

.. image:: /image/figure18.png

gtest函数的对比如下图所示：

.. image:: /image/figure18.png
