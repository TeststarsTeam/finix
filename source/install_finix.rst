install finix
=============
finix在windows下的配置
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


finix在linux下的配置
----------------------
1. 下载wings的安装包，在码云下载

see https://gitee.com/teststars/wings_release.git.

2. 解压安装包，点击wings.exe,打开wings使用界面

3. 解压wingsforlinux.tar.gz到linux目录下（其中/home/wings/nginx为linux下工程所在路径）执行以下命令::

./wings_parse /home/wings/nginx

4. 拷贝linux下生成的FunXml与GlobalXml到windows目录下

5. 完成驱动与值的生成
