c版本参数捕获代码说明
=============================================
wings参数捕获功能主要是在运行时获取函数的参数值，全局变量值以及返回值的信息


参数捕获代码的命名规则
-----------------------
wings的参数捕获代码存储在paramcaputrecode文件夹中。其中命名规则同驱动格式一样，将所有的driver替换为param即可。


参数捕获代码的示例
-----------------------

如下所示，针对函数coordinate_destroy中，会自动生成捕获参数、全局变量以及返回值的信息。

::

	#ifndef _PARAMCAPTURE_WINGS_C_DEMO_COORDINATES_H_
	#define _PARAMCAPTURE_WINGS_C_DEMO_COORDINATES_H_
	#include "ParamCapture.h"
	#include "ParamCapture_structorunion.h"
	void ReturnCapture_coordinate_create(coordinate_s returnType);

	void ParamCapture_coordinate_destroy(location *loc, size_t length, char *ch);
	void GlobalCapture_coordinate_destroy(int count);
	void ReturnCapture_coordinate_destroy(int returnType);

	void ParamCapture_func_point(double param1, int param2);
	void ReturnCapture_func_point(void returnType);
	#endif // WINGS_C_DEMO_COORDINATES
	

下图将展示捕获全局变量的自动生成代码。

.. image:: /image/figure13.png