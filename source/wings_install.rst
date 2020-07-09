wings部署操作说明
=============
wings目前支持windows与linux平台。


系统配置要求
---------
wings客户端目前支持windows、linux操作系统平台。机器的最低配置如下：

+------------------------+------------------------+------------------------+
| CPU                    | 内存                   | 枚举类型               | 
+------------------------+------------------------+------------------------+
| 4核                    | 8G                     | 100G                   | 
+------------------------+------------------------+------------------------+


编译数据库
---------

linux下编译数据库的生成
^^^^^^^^
（1）针对makefile的工程，星云测试提供安装的bear的安装包，按照步骤操作即可。安装bear https://github.com/rizsotto/Bear（bear所需python>2.7）cmake

（3）类似的其他生成编译数据库的方式，可以参考https://sarcasm.github.io/notes/dev/compilation-database.html。

（2）针对cmake工程，设置-DCMAKE_EXPORT_COMMANDS=ON即可，会在对应的build目录下生成compile_commands.json文件。


windows下编译数据库的生成
^^^^^^^^
（1）windows下的目前支持vs2015以上版本与Msys2+mingw的编译环境。

（2）安装星云测试提供的vs插件，Sourcetrail.Extension.v2.0.3.130.vsix 双击安装之后，会在vs的菜单栏出现Sourcetrail，选择Create Compilation Database，生成对应源程序的complie_command.json文件。

（3）qt中包含有生成自带的编译数据库的文件，在编辑->Generate Copmliation Database for “工程”
