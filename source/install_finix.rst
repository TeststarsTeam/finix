installing and running finix
=============
On Windows Systems
-----------------------
1. 下载wings的安装包，在码云下载

   see https://gitee.com/teststars/wings_release.git.

2. 解压安装包，点击wings.exe,打开wings使用界面

3. 安装vs2013及以上版本，配置系统环境变量

4. 配置windows头文件系统变量，如下

* C:\Program Files (x86)\VS15\VC\include;
* C:\Program Files (x86)\Windows Kits\10\Include\10.0.10240.0\ucrt;
* C:\Program Files (x86)\VS15\VC\atlmfc\include;
* C:\Program Files (x86)\Windows Kits\8.1\Include\um;
* C:\Program Files (x86)\Windows Kits\8.1\Include\shared;
* C:\Program Files (x86)\Windows Kits\8.1\Include\winrt;


On Unix Systems
----------------------
1. 下载wings的安装包，在码云下载

   see https://gitee.com/teststars/wings_release.git.

2. 解压安装包，点击wings.exe,打开wings使用界面

3. 解压wingsforlinux.tar.gz到linux目录下（其中/home/wings/nginx为linux下工程所在路径）执行以下命令::

   ./wings_parse /home/wings/nginx

4. 拷贝linux下生成的FunXml与GlobalXml到windows目录下

5. 完成驱动与值的生成

Run finix
----------
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