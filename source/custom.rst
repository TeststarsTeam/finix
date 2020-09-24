自定义模板类
=============

思想：自定义模板类本质上还是类，只是它没有固定的类型,只有在模板类实例化时，才能知道具体的类型；我们的解决方案是将模板类的信息存储好，标记为模板类，在实际调用到该模板类类型时，如模板类作为参数，对该模板类的中的信息替换为当前传入的信息，即主要替换模板类的参数类型信息，生成一个新的模板类，其实这里已经是一个新的普通类了，没有了模板类的类型限制，通过走类的逻辑进行参数赋值，但最后在进行模板类的实例化时和类的有所不同，这里是对其进行判断之后进行模板类的类型实例化。实际遇到模板类时，根据实际的参数通过在缓存中替换数据构造一个新的“模板类”。


测试的源码
-----------------------

源码测试考虑到了模板类型的普通成员变量、一级指针、二级指针；以及模板类型作为函数参数和模板类型作为模板类型的内部类型（即模板类型的嵌套），对于复杂的模板类暂未处理，后续会持续增加。
 
::

	#pragma warning(disable:4996)
	#pragma once
	#include <iostream>
	#include <string>
	#include <fstream>
	#include <vector>
	#include "json/json.h"
	template <typename T>
	class NestingTem
	{
	public:
		NestingTem(T t,bool wings) :Test(t)
		{};
		~NestingTem()
		{};
	private:
		T Test;
	};
	// 类模版
	template <typename T1,typename T2, typename T3, typename T4, typename T5>
	class ClassTemplate
	{
	public:
		ClassTemplate() {};
		ClassTemplate(T1 t,T2 tt,T3 *t3,T4 t4[],T5 **t5,bool wings) :name(t),sex(tt),Pointer(t3),TwoPinter(t5)
		{
			for (unsigned int size = 0; size < 10; size++)
			{
				t4[size] = Arrayss[size];
			}
		}
	private:
		T1 name;
		T2 sex;
		T3 *Pointer;
		T4 Arrayss[10];
		T5 **TwoPinter;
	};

	class Templatecl
	{
	public:
		Templatecl(ClassTemplate<int, double, char, int, int> _ts, ClassTemplate<int, double, char, int, int> *_Pointer,
			ClassTemplate<int, double, char, int, int> **_TwoPointer,bool wings) :ts(_ts),Pointer(_Pointer),TwoPointer(_TwoPointer){
		}
		void fprinTemplate(ClassTemplate<int, double,char,int,int> &m);
		void NestingTemFun1(ClassTemplate<NestingTem<int>, double, char, int, int > &nesting);
		void complexTemplate(ClassTemplate <NestingTem<char>,TemplateTwo<NestingTem<int>,double>,char, int, int> &test2);
	private:
		ClassTemplate<int, double,char,int,int> ts;
		ClassTemplate<int, double, char, int, int> *Pointer;
		ClassTemplate<int, double, char, int, int> **TwoPointer;
	};


自定义模板类类型的成员变量
----------------------

自定义模板类的成员变量在驱动中主要是在构造函数中初始化，即赋值，通过对模板类中的类型进行替换成实际的类型。其中包含普通类型、一级指针、二级指针。

::

	普通类型、一级指针、二级指针的构造函数的赋值
	DriverTemplatecl::DriverTemplatecl(Json::Value Root,int times)
	 {
		Json::Value _Templatecl_Root = Root["Templatecl"+std::to_string(times)];
		Json::Value _tsts_Root=_Templatecl_Root["ts"];
	/* name */
		int _tsname = _tsts_Root["name"].asInt();           
	/* sex */
		double _tssex = _tsts_Root["sex"].asDouble();          
	/* Pointer */
		char* _tsPointer;
		{
			std::string _tsPointer_str = _tsts_Root["Pointer"].asString();
			_tsPointer=new char[_tsPointer_str.size()];
			memcpy(_tsPointer,_tsPointer_str.c_str(),_tsPointer_str.size());
		}          
	/* Arrayss */
		int _tsArrayss[10];
		for(int len = 0; len < 10; len++)
		{
			_tsArrayss[len] = _tsts_Root["Arrayss"][len].asInt();
		}            
	/* TwoPinter */
		int **_tsTwoPinter;
		{
			int W_x = _tsts_Root["TwoPinter"].size();
			_tsTwoPinter = new int*[W_x];
			for(int i = 0; i < W_x; i++)
			{
				int W_y = _tsts_Root["TwoPinter"][i].size();
				_tsTwoPinter[i] = new int[W_y];
				for(int j = 0; j < W_y; j++)
				{
					_tsTwoPinter[i][j] = _tsts_Root["TwoPinter"][i][j].asInt();
				}
			}
		}            
	ClassTemplate<int, double, char, int, int> _ts(_tsname, _tssex, _tsPointer, _tsArrayss, _tsTwoPinter, false);               
	int _PointerpointSize=0;
	Json::Value _PointerPointer_Root=_Templatecl_Root["Pointer"][_PointerpointSize];
	/* name */
		int _Pointername = _PointerPointer_Root["name"].asInt();          
	/* sex */
		double _Pointersex = _PointerPointer_Root["sex"].asDouble();          
	/* Pointer */
		char* _PointerPointer;
		{
			std::string _PointerPointer_str = _PointerPointer_Root["Pointer"].asString();
			_PointerPointer=new char[_PointerPointer_str.size()];
			memcpy(_PointerPointer,_PointerPointer_str.c_str(),_PointerPointer_str.size());
		}            
	/* Arrayss */
		int _PointerArrayss[10];
		for(int len = 0; len < 10; len++)
		{
			_PointerArrayss[len] = _PointerPointer_Root["Arrayss"][len].asInt();
		}          
	/* TwoPinter */
		int **_PointerTwoPinter;
		{
			int W_x = _PointerPointer_Root["TwoPinter"].size();
			_PointerTwoPinter = new int*[W_x];
			for(int i = 0; i < W_x; i++)
			{
				int W_y = _PointerPointer_Root["TwoPinter"][i].size();
				_PointerTwoPinter[i] = new int[W_y];
				for(int j = 0; j < W_y; j++)
				{
					_PointerTwoPinter[i][j] = _PointerPointer_Root["TwoPinter"][i][j].asInt();
				}
			}
		}
				
		ClassTemplate<int, double, char, int, int> *_Pointer=new ClassTemplate<int, double, char, int, int>(_Pointername, 
			_Pointersex, _PointerPointer, _PointerArrayss, _PointerTwoPinter, false);            
		Json::Value _TwoPointerTwoPointer_Root=_Templatecl_Root["TwoPointer"];
	/* name */
		int _TwoPointername = _TwoPointerTwoPointer_Root["name"].asInt();            
	/* sex */
		double _TwoPointersex = _TwoPointerTwoPointer_Root["sex"].asDouble();            
	/* Pointer */
		char* _TwoPointerPointer;
		{
			std::string _TwoPointerPointer_str = _TwoPointerTwoPointer_Root["Pointer"].asString();
			_TwoPointerPointer=new char[_TwoPointerPointer_str.size()];
			memcpy(_TwoPointerPointer,_TwoPointerPointer_str.c_str(),_TwoPointerPointer_str.size());
		}          
	/* Arrayss */
		int _TwoPointerArrayss[10];
		for(int len = 0; len < 10; len++)
		{
			_TwoPointerArrayss[len] = _TwoPointerTwoPointer_Root["Arrayss"][len].asInt();
		}           
	/* TwoPinter */
		int **_TwoPointerTwoPinter;
		{
			int W_x = _TwoPointerTwoPointer_Root["TwoPinter"].size();
			_TwoPointerTwoPinter = new int*[W_x];
			for(int i = 0; i < W_x; i++)
			{
				int W_y = _TwoPointerTwoPointer_Root["TwoPinter"][i].size();
				_TwoPointerTwoPinter[i] = new int[W_y];
				for(int j = 0; j < W_y; j++)
				{
					_TwoPointerTwoPinter[i][j] = _TwoPointerTwoPointer_Root["TwoPinter"][i][j].asInt();
				}
			}
		}          
	ClassTemplate<int, double, char, int, int> *_TwoPointer_ClassTemplate=new ClassTemplate<int, double, char, int, int>(_TwoPointername,_TwoPointersex, _TwoPointerPointer, _TwoPointerArrayss, _TwoPointerTwoPinter, false);
		ClassTemplate<int, double, char, int, int> **_TwoPointer=&_TwoPointer_ClassTemplate;               
		_Templatecl=new Templatecl(_ts, _Pointer, _TwoPointer, false);
	}


自定义模板类类型的函数参数
----------------------

普通的自定义模板类类型作为函数参数进行赋值驱动函数。

::

	测试函数：
	void fprinTemplate(ClassTemplate<int, double,char,int,int> & m);
	驱动代码：
	int DriverTemplatecl:: DriverTemplateclfprinTemplate0(int times)
	{
		fprinTemplate0Times = times;
		const char*jsonFilePath="drivervalue/Templatecl/fprinTemplate0.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _fprinTemplate0_Root = Root["fprinTemplate0"+std::to_string(times)];
		Json::Value _mm_Root=_fprinTemplate0_Root["m"];
	/* name */
		int _mname = _mm_Root["name"].asInt();            
	/* sex */
		double _msex = _mm_Root["sex"].asDouble();           
	/* Pointer */
		char* _mPointer;
		{
			 std::string _mPointer_str = _mm_Root["Pointer"].asString();
			 _mPointer=new char[_mPointer_str.size()];
			 memcpy(_mPointer,_mPointer_str.c_str(),_mPointer_str.size());
		}          
	/* Arrayss */
		int _mArrayss[10];
		for(int len = 0; len < 10; len++)
		{
			 _mArrayss[len] = _mm_Root["Arrayss"][len].asInt();
		}            
	/* TwoPinter */
	   int **_mTwoPinter;
	   {
			int W_x = _mm_Root["TwoPinter"].size();
			_mTwoPinter = new int*[W_x];
			for(int i = 0; i < W_x; i++)
			{
				int W_y = _mm_Root["TwoPinter"][i].size();
				_mTwoPinter[i] = new int[W_y];
				for(int j = 0; j < W_y; j++)
				{
					_mTwoPinter[i][j] = _mm_Root["TwoPinter"][i][j].asInt();
				}
			 }
	   }           
	   ClassTemplate<int, double, char, int, int> _m(_mname, _msex, _mPointer, _mArrayss, _mTwoPinter, false);                
	//The Function of Class    Call
	   _Templatecl->fprinTemplate( _m);                
	  return 0;
	}


模板类作为自定义模板类的参数类型
----------------------

模板类嵌套模板类，即模板类中的参数类型是其他的模板类，形成嵌套，下列代码是测试嵌套了三层的模板类参数。

::

	测试的函数：
	void complexTemplate(ClassTemplate <NestingTem<char>,TemplateTwo<NestingTem<int>,double>,char, int, int> &test2);
	驱动代码：
	int DriverTemplatecl:: DriverTemplateclcomplexTemplate2(int times)
	{
		complexTemplate2Times = times;
		const char*jsonFilePath="drivervalue/Templatecl/complexTemplate2.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _complexTemplate2_Root = Root["complexTemplate2"+std::to_string(times)];
		Json::Value _test2test2_Root=_complexTemplate2_Root["test2"];
		Json::Value _test2test2_0test2_0_Root=_test2test2_Root["test2_0"];
	/* Test */
		std::string _test2test2_0TestStr = _test2test2_0test2_0_Root["Test"].asString();
		char _test2test2_0Test=_test2test2_0TestStr[0];           
		NestingTem<char> _test2test2_0(_test2test2_0Test, false);               
		Json::Value _test2test2_1test2_1_Root=_test2test2_Root["test2_1"];
		Json::Value _test2test2_1test2_1_0test2_1_0_Root=_test2test2_1test2_1_Root["test2_1_0"];
	/* Test */
		int _test2test2_1test2_1_0Test = _test2test2_1test2_1_0test2_1_0_Root["Test"].asInt();          
		NestingTem<int> _test2test2_1test2_1_0(_test2test2_1test2_1_0Test, false);              
	/* Test2 */
		double _test2test2_1Test2 = _test2test2_1test2_1_Root["Test2"].asDouble();           
		TemplateTwo<NestingTem<int>, double> _test2test2_1(_test2test2_1test2_1_0, _test2test2_1Test2, false);                
	/* Pointer */
		char* _test2Pointer;
		{
			std::string _test2Pointer_str = _test2test2_Root["Pointer"].asString();
			_test2Pointer=new char[_test2Pointer_str.size()];
			memcpy(_test2Pointer,_test2Pointer_str.c_str(),_test2Pointer_str.size());
		}           
	/* Arrayss */
		int _test2Arrayss[10];
		for(int len = 0; len < 10; len++)
		{
			_test2Arrayss[len] = _test2test2_Root["Arrayss"][len].asInt();
		}           
	/* TwoPinter */
		int **_test2TwoPinter;
		{
			int W_x = _test2test2_Root["TwoPinter"].size();
			_test2TwoPinter = new int*[W_x];
			for(int i = 0; i < W_x; i++)
			{
				int W_y = _test2test2_Root["TwoPinter"][i].size();
				_test2TwoPinter[i] = new int[W_y];
				for(int j = 0; j < W_y; j++)
				{
					_test2TwoPinter[i][j] = _test2test2_Root["TwoPinter"][i][j].asInt();
				}
			}
		}           
	ClassTemplate<NestingTem<char>, TemplateTwo<NestingTem<int>, double>, char, int, int> _test2(_test2test2_0, _test2test2_1, _test2Pointer, _test2Arrayss, _test2TwoPinter, false);              
	//The Function of Class    Call
	   _Templatecl->complexTemplate( _test2);             
	   return 0;
	}
