c++ 参数捕获代码的说明
=============================================


c++ 参数捕获代码的命名规则
-----------------------

c++的参数捕获的命名规则与驱动一样，将driver换成paramcaputre即可。


c++ 参数捕获代码的说明
-----------------------

针对每个类，生成一个参数捕获的类，捕获类中分别对应每个函数会生成捕获参数、全局变量、以及返回值的函数。

::

	#pragma once
	#include "paramcapture.h"
	class ParamCaptureRectangle
	{
		public:
		ParamCaptureRectangle();
		 ~ParamCaptureRectangle();
				
		void ParamCapture_GetVolume0();
		void GlobalCapture_GetVolume0();
		void ReturnCapture_GetVolume0(int returnType);
					
		void ParamCapture_PrintVolume1(Rectangle *rect,int parm);
		void GlobalCapture_PrintVolume1();
		void ReturnCapture_PrintVolume1();
	};



如何捕获类的成员变量，会在类中插入一个捕获函数，来获取类的私有成员变量，如下所示：

::

	class Rectangle
	{
	private:
		Point *m_point;
		int m_z;
		std::string name;
	public:
		Rectangle();
		int GetVolume();
		void PrintVolume(Rectangle *rect, int parm);
	public:
		Rectangle(Point *m_point, int m_z, std::string name, bool wings)
		{
			this->m_point = m_point;
			this->m_z = m_z;
			this->name = name;
		}
		Json::Value W_MemberVarCaputre()
		{
			Json::Value Rectangle_Root;
			Rectangle_Root["m_point"]=m_point->W_MemberVarCaputre();
			Rectangle_Root["m_z"]=Json::Value(m_z);
			Rectangle_Root["name"]=Json::Value(name);
			return Rectangle_Root;
		}
	};


