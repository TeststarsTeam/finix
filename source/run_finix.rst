run finix
==========
1.解压wings安装包，点击wings.exe ，打开wings界面图如下

.. image:: /image/finix_logo.png

2.点击文件，设置被测程序所在的路径

.. image:: /image/setting.png

3.点击工程操作中的分析生成，对工程目录下的.c文件进行解析，保存为PSD的格式

.. image:: /image/parse.png

生成的文件保存在工程目录下的FunXml与GlobalXml中，分别是函数信息与全局变量的信息


.. image:: /image/fun_global.png

4.点击工程操作中的驱动生成操作，生成对应.c文件的驱动文件

.. image:: /image/drive.png

驱动生成完成之后，点击驱动文件结构图，则可以看到生成对应的驱动文件列表

.. image:: /image/drive1.png

5.点击工程操作的值生成，生成对应的测试数据，如下图

.. image:: /image/json.png