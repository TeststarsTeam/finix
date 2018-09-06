Finix - Intelligent fully automatic test case drive generation system
=====================================================================

Introduction
-------------

finix是一个智能的、全自动测试用例驱动生成系统，更好的解决了单元测试遇到的诸多难点。

Finix is an intelligent, fully automated test case driver generation system that better solves many difficulties encountered in unit testing.

Finix solves the problem of unit testing
----------------------------------------
* finix是智能的全自动测试用例驱动生成系统，可以为任意复杂参数结构C语言开发的系统全自动生成测试驱动程序。

* finix可完成对于被测试函数的参数进行多层编译解析，并完成复杂参数赋值的代码的自动生成。

* 支持被测函数引用的全局变量的分析和自动赋值程序的生成。

* 能够区分系统变量和用户变量，对于复杂的系统变量可由用户自定义赋值模板，例如File类型，而不是把复杂的系统变量全部展开。

* 支持多层次的可视化的数据表格来对变量进行赋值，而无需关注驱动程序本身。数据表格可以表达任意深度和多层次的数据关系，用户只需要对表格数据进行编辑，自动生成的驱动程序会自动完成表格数据的读取和参数赋值的构造过程。

* finix支持所有C语言的数据类型（基础类型，结构体，指针，数组，枚举等）以及高层级数据结构例如链表的分析和对应的驱动和数据表格框架的生成。

* finix生成的代码可人工写的非常相近，可读性强，自带注释和按照层次的缩进和代码编排。
 

Documentation
-------------

  see https://finix.readthedocs.io/


Installation
------------

  see https://finix.readthedocs.io/zh/latest/install_finix.html

Contribution
------------

  see https://finix.readthedocs.io/zh/latest/install_finix.html

Support
-------

  星云测试提供对finix的商业支持。 有关更多信息，定价和支持级别信息，请参阅 http://www.threadingtest.com/
  
  Commercial support for Finix is available from TestStars. For more information, pricing and support levels info see http://www.threadingtest.com/.
