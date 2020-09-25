类的二级指针
=============

测试的源码
-----------------------

测试的源码中考虑到了类的普通类型、类的一级指针、类的二级指针的参数赋值以及作为返回值，构造函数中类中类的二级指针类型的成员变量的赋值。

::

	#include"json/json.h"
	#include <iostream>
	using namespace std;
	#pragma once
	namespace PointerCase {
		class PointerPair
		{
		public:
			PointerPair();
			~PointerPair();

		private:
			int Test;
			char pair;

	public:
	PointerPair(int Test, char pair, bool Wings):Test(Test), pair(pair)
	{}
	};
		class tooltipPoints
		{
		public:
			tooltipPoints();
			~tooltipPoints();
		private:
			int a;
			char b;

	public:
	tooltipPoints(int a, char b, bool Wings):a(a), b(b)
	{}
	};
		class value_TestClass
		{
		public:
			value_TestClass();
			~value_TestClass();
		private:
			PointerPair buitinClass;
			PointerPair *PointerClass;
			tooltipPoints **TwoPointerClass;
		public:
			void gawp_pointer_options(PointerPair buitinClass);
			void testing_prng_seed(PointerPair *PointerClass);
			void TestSuite_AddLive(tooltipPoints **TwoPointerClass);
			PointerCase::tooltipPoints TestClassReturn(PointerPair buitinClass);
			PointerCase::tooltipPoints *TestClassReturnPointer(PointerPair buitinClass);
			PointerCase::tooltipPoints **TestClassReturnTwoPointer(PointerPair buitinClass);
	public:
	value_TestClass(PointerCase::PointerPair buitinClass, PointerCase::PointerPair *PointerClass, PointerCase::tooltipPoints **TwoPointerClass, bool Wings):buitinClass(buitinClass), PointerClass(PointerClass), TwoPointerClass(TwoPointerClass)
	{}
	};
	}


类的二级指针的值生成
-----------------------

类的二级指针直接作为普通类类型进行值生成。

**问题：**为什么类的二级指针直接作为普通类类型进行值生成？

**答：**因为在类的二级指针进行赋值时，传递的时引用，所以只需要实例化一次就够了，即new一个该类的指针。

::

	{
	   "PointerPair0" : {
		  "Test" : 7443,
		  "pair" : "C"
	   },
	   "tooltipPoints0" : {
		  "a" : 3804,
		  "b" : "V"
	   },
	   "value_TestClass0" : {
		  "PointerClass" : [
			 {
				"Test" : 2292,
				"pair" : "j"
			 }
		  ],
		  "TwoPointerClass" : {
			 "a" : 5836,
			 "b" : "x"
		  },
		  "buitinClass" : {
			 "Test" : 2673,
			 "pair" : "k"
		  }
	   }
	}


类的二级指针驱动生成
-----------------------

通过从json中取值，然后进行对类的成员变量利用插装的构造函数进行赋值，然后实例化该类，将该类的引用赋给定义的一个类的二级指针，进行传参实现对原函数的调用。

**问题：**为什么使用引用？

**答：**因为要调用原函数，我们只需要传一个类的二级指针即可，并且对应值生成部分的内容，只需要new一个指针，然后传引用就可以实现目的。

:: 

	int Divervalue_TestClass::Drivervalue_TestClassTestSuite_AddLive2(int times)
	{
		TestSuite_AddLive2Times = times;
		/* Root is the json object of the value file.TestSuite_AddLive2_Root is function.TestSuite_AddLive2 is json object.  */
		const char* jsonFilePath = "drivervalue/value_TestClass/TestSuite_AddLive2.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _TestSuite_AddLive2_Root = Root["TestSuite_AddLive2" + std::to_string(times)];
		/*It is the 1 parameter: TwoPointerClass    TestSuite_AddLive2
		 *
		 * Parameters of the prototype:PointerCase::tooltipPoints **TwoPointerClass     
		 */ 
		Json::Value _TwoPointerClassTwoPointerClass_Root = _TestSuite_AddLive2_Root["TwoPointerClass"];
		/* a */
		int _TwoPointerClassa = _TwoPointerClassTwoPointerClass_Root["a"].asInt(); 
		/* b */
		string _TwoPointerClassbStr = _TwoPointerClassTwoPointerClass_Root["b"].asString();
		char _TwoPointerClassb = _TwoPointerClassbStr[0]; 
		PointerCase::tooltipPoints* TwoPointerClass_value = new PointerCase::tooltipPoints(_TwoPointerClassa, _TwoPointerClassb, false);
		PointerCase::tooltipPoints** _TwoPointerClass = &TwoPointerClass_value;
		//The Function of Class    Call
		_value_TestClass->TestSuite_AddLive(_TwoPointerClass); 
		return 0;
	}


类的二级指针作为返回值
-----------------------

类的二级指针作为返回值调用的是参数捕获中插装的函数（W_MemberVarCaputer())，需要注意的是当没有进行参数捕获代码生成时，是没有该函数的。

:: 

	void Drivervalue_TestClass::ReturnDriver_TestClassReturnTwoPointer5(PointerCase::tooltipPoints** returnType)
	{
		const char* JsonFilePath = "drivervalue/value_TestClass/TestClassReturnTwoPointer5.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(JsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value TestClassReturnTwoPointer5_Root = Root["TestClassReturnTwoPointer5" + std::to_string(TestClassReturnTwoPointer5Times)];
		/* returnType */
		TestClassReturnTwoPointer5_Root["returnType"] = returnType[0]->W_MemberVarCaputer();  
		Root["TestClassReturnTwoPointer5" + std::to_string(TestClassReturnTwoPointer5Times)] = TestClassReturnTwoPointer5_Root;
		std::ofstream JsonFile;
		Json::StyledWriter sw;
		JsonFile.open(JsonFilePath);
		JsonFile << sw.write(Root);
		JsonFile.close();

	}


构造函数中对类的二级指针的成员变量赋值
-----------------------

构造函数中对类的二级指针的成员变量赋值部分调用的是参数赋值的逻辑，所以与之相比并没有什么改变。

:: 

	Drivervalue_TestClass::Drivervalue_TestClass(Json::Value Root, int times)
	{
		Json::Value _value_TestClass_Root = Root["value_TestClass" + std::to_string(times)];
		Json::Value _buitinClassbuitinClass_Root = _value_TestClass_Root["buitinClass"];
		/* Test */
		int _buitinClassTest = _buitinClassbuitinClass_Root["Test"].asInt();
		/* pair */
		string _buitinClasspairStr = _buitinClassbuitinClass_Root["pair"].asString();
		char _buitinClasspair = _buitinClasspairStr[0];   
		PointerCase::PointerPair _buitinClass(_buitinClassTest, _buitinClasspair, false);  
		int _PointerClasspointSize = 0;
		Json::Value _PointerClassPointerClass_Root = _value_TestClass_Root["PointerClass"][_PointerClasspointSize];
		/* Test */
		int _PointerClassTest = _PointerClassPointerClass_Root["Test"].asInt();  
		/* pair */
		string _PointerClasspairStr = _PointerClassPointerClass_Root["pair"].asString();
		char _PointerClasspair = _PointerClasspairStr[0];  
		PointerCase::PointerPair* _PointerClass = new PointerCase::PointerPair(_PointerClassTest, _PointerClasspair, false);  
		Json::Value _TwoPointerClassTwoPointerClass_Root = _value_TestClass_Root["TwoPointerClass"];
		/* a */
		int _TwoPointerClassa = _TwoPointerClassTwoPointerClass_Root["a"].asInt();  
		/* b */
		string _TwoPointerClassbStr = _TwoPointerClassTwoPointerClass_Root["b"].asString();
		char _TwoPointerClassb = _TwoPointerClassbStr[0];   
		PointerCase::tooltipPoints* TwoPointerClass_value = new PointerCase::tooltipPoints(_TwoPointerClassa, _TwoPointerClassb, false);
		PointerCase::tooltipPoints** _TwoPointerClass = &TwoPointerClass_value;   
		_value_TestClass = new PointerCase::value_TestClass(_buitinClass, _PointerClass, _TwoPointerClass, false);
	}
	