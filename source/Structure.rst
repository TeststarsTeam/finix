结构体引用
=============

测试的源码
-----------------------

测试的源码中考虑到了普通结构体、结构体指针、结构体引用作为参数，以及作为返回值，并且考虑了结构体中含有结构体成员的情况，构造函数中对类的结构体的引用类型的成员变量的赋值。

::

	#include"json/json.h"
	#include <iostream>
	using namespace std;
	#pragma once
	class StructQuote
	{
	public:
		StructQuote()
		{
		};
		~StructQuote() {};
		struct TestCase
		{
			int had;
		};
		struct QuoteTestCase
		{
			int one;
			char two;
			TestCase CaseQuote;
		};
	public:
		QuoteTestCase *CheckFunc;
		QuoteTestCase CheckFuncR;
		QuoteTestCase &CheckFuncRRR=CheckFuncR;
		void CheckNameTest(QuoteTestCase *CheckFunc);
		void CheckFunctionName(QuoteTestCase CheckFunc);
		void CheckFunctionTest(QuoteTestCase &CheckFunc);
		StructQuote::QuoteTestCase *CheckPointer(QuoteTestCase *CheckFunc);
		StructQuote::QuoteTestCase Checkbuiltn(QuoteTestCase CheckFunc);
		StructQuote::QuoteTestCase &CheckNameXX(QuoteTestCase CheckFunc);
	public:
	StructQuote(QuoteTestCase *CheckFunc, QuoteTestCase CheckFuncR, QuoteTestCase &CheckFuncRRR , bool Wings):CheckFunc(CheckFunc), CheckFuncR(CheckFuncR), CheckFuncRRR(CheckFuncRRR)
	{}
	};


结构体引用的值生成
-----------------------

结构体引用的值生成和结构体普通类型是一致的，直接对结构体内部的成员进行一个个赋值。

::

	{
	   "QuoteTestCase0" : {
		  "CaseQuote" : {
			 "had" : 8039
		  },
		  "one" : 143,
		  "two" : "s"
	   },
	   "StructQuote0" : {
		  "CheckFunc" : [
			 {
				"CaseQuote" : {
				   "had" : 6547
				},
				"one" : 2732,
				"two" : "v"
			 }
		  ],
		  "CheckFuncR" : {
			 "CaseQuote" : {
				"had" : 5702
			 },
			 "one" : 2401,
			 "two" : "Q"
		  },
		  "CheckFuncRRR" : {
			 "CaseQuote" : {
				"had" : 1486
			 },
			 "one" : 7740,
			 "two" : "u"
		  }
	   },
	   "TestCase0" : {
		  "had" : 1764
	   }
	}


结构体引用作为参数
-----------------------

结构体引用直接调用普通结构体的赋值函数。

**问题：** 为什么直接调用普通结构体的赋值函数？

**答：** 因为结构体引用部分的逻辑和普通结构体是一模一样的，值生成部分也是一样，所以选择在参数赋值时直接调用普通结构体类型的，这样可以不用再写一遍引用部分的驱动。

::

	int DriverStructQuote::DriverStructQuoteCheckFunctionTest2(int times)
	{
		CheckFunctionTest2Times = times;
		/* Root is the json object of the value file.CheckFunctionTest2_Root is function.CheckFunctionTest2 is json object.  */
		const char* jsonFilePath = "drivervalue/StructQuote/CheckFunctionTest2.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _CheckFunctionTest2_Root = Root["CheckFunctionTest2" + std::to_string(times)];
		/*It is the 1 parameter: CheckFunc    CheckFunctionTest2
		 *
		 * Parameters of the prototype:StructQuote::QuoteTestCase &CheckFunc     
		 */ 
		/* CheckFunc */
		Json::Value _CheckFunc_Root = _CheckFunctionTest2_Root["CheckFunc"];  
		struct StructQuote::QuoteTestCase _CheckFunc = DriverstructQuoteTestCase(_CheckFunc_Root);
		//The Function of Class    Call
		_StructQuote->CheckFunctionTest(_CheckFunc);  
		return 0;
	}


结构体引用作为函数返回值
-----------------------

结构体引用作为函数返回值同参数赋值一样，可以张家界调用普通结构体类型的Return函数进行赋值。

::

	void DriverStructQuote::ReturnDriver_CheckNameXX3(StructQuote::QuoteTestCase& returnType)
	{
		const char* JsonFilePath = "drivervalue/StructQuote/CheckNameXX3.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(JsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value CheckNameXX3_Root = Root["CheckNameXX3" + std::to_string(CheckNameXX3Times)];
		/* returnType */
		Json::Value returnType_Root;
		returnType_Root = Returnstruct_QuoteTestCase(returnType_Root, returnType);
		CheckNameXX3_Root["returnType"] = returnType_Root;
		Root["CheckNameXX3" + std::to_string(CheckNameXX3Times)] = CheckNameXX3_Root;
		std::ofstream JsonFile;
		Json::StyledWriter sw;
		JsonFile.open(JsonFilePath);
		JsonFile << sw.write(Root);
		JsonFile.close();
	}


构造函数中对结构体引用类型的成员变量的赋值
-----------------------

构造函数中赋值部分调用的也是参数赋值部分的内容，所以在结构体引用部分调用的还是普通结构体的父子函数。

::

	DriverStructQuote::DriverStructQuote(Json::Value Root, int times)
	{
		Json::Value _StructQuote_Root = Root["StructQuote" + std::to_string(times)];
		/* CheckFunc */
		Json::Value _CheckFunc_Root = _StructQuote_Root["CheckFunc"];
		int _CheckFunc_len = _CheckFunc_Root.size();
		StructQuote::QuoteTestCase* _CheckFunc = DriverstructQuoteTestCasePoint(_CheckFunc_Root, _CheckFunc_len);
		/* CheckFuncR */
		Json::Value _CheckFuncR_Root = _StructQuote_Root["CheckFuncR"];
		struct StructQuote::QuoteTestCase _CheckFuncR = DriverstructQuoteTestCase(_CheckFuncR_Root); 
		/* CheckFuncRRR */
		Json::Value _CheckFuncRRR_Root = _StructQuote_Root["CheckFuncRRR"];
		struct StructQuote::QuoteTestCase _CheckFuncRRR = DriverstructQuoteTestCase(_CheckFuncRRR_Root);
		_StructQuote = new StructQuote(_CheckFunc, _CheckFuncR, _CheckFuncRRR, false);
	}


