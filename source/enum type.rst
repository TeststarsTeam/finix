枚举类型
=============

测试的源码
-----------------------

测试的源码中考虑到了普通枚举、强枚举、枚举指针、枚举引用的值生成、驱动生成以及参数赋值驱动生成，以及构造函数中对类的枚举类型的成员变量的赋值。

::

	#include "json/json.h"
	#pragma once
	namespace wings_grammars_test {
		enum Code {
			kOk = 0,
			kNotFound = 1,
			kCorruption = 2,
			kNotSupported = 3,
			kInvalidArgument = 4,
			kIOError = 5
		};
		enum class EnumBase
		{
			kNoCompression = 0x0,
			kSnappyCompression = 0x1
		};
		class EnumClassTesting
		{
		private:
			EnumBase enumBaseType;
			EnumBase &enumBaseTypeR;
			Code codeType;
			Code *codePointType;
		public:
			void CodeFunc(Code codeType);
			void EnumBaseFunc(EnumBase enumBaseType);
			void EnumBaseFuncR(EnumBase &enumBaseType);
			void CodeFuncPoint(Code *codeType);
		public:
			EnumClassTesting(wings_grammars_test::EnumBase enumBaseType,
			wings_grammars_test::EnumBase &enumBaseTypeR,
			wings_grammars_test::Code codeType, wings_grammars_test::Code *codePointType,
			bool Wings) :enumBaseType(enumBaseType), enumBaseTypeR(enumBaseTypeR), codeType(codeType), codePointType(codePointType)
			{}
		};
	}


枚举的测试用例生成
-----------------------

鉴于枚举驱动部分需要传入字符串进行比较然后赋值，所以值生成部分需要获取所有枚举的具体值（字符串类型的）；

如（KOK等，非0、1、2）：

::

	<wings_grammars_test::Code>
		<kOk value="0" />
		<kNotFound value="1" />
		<kCorruption value="2" />
		<kNotSupported value="3" />
		<kInvalidArgument value="4" />
		<kIOError value="5" />
	</wings_grammars_test::Code>


**问题：** 为什么只能使用字符串进行赋值？

**答：** 因为c++11存在强枚举类型，如：enum class Enumeration{ };

此种枚举为类型安全的，枚举类型不能隐式地转换为整数，也无法与整数数值做比较，所以只能赋值枚举类型相应的字符串。

**问题：** 一个枚举类型存在多个值，怎么确定是哪一个值？

**答：** 枚举类型存在多个值，同时也意味着每个值都可以赋值给原函数，所以采用随机数的方式，随机生成该枚举类型中的任一值进行赋值。

**问题：** 枚举指针、枚举引用、基本枚举的值分别是什么形式？

**答：** 枚举指针、基本枚举都是一样的，随机生成一个该类型的枚举值（字符串形式），将该字符串存储到json中；枚举指针视为数组类型，存储多个该类型的枚举值（字符串形式）。


**生成之后的结果：**

::

	{
	   "EnumClassTesting0" : {
		  "codePointType" : [ "kInvalidArgument", "kIOError", "kCorruption" ],
		  "codeType" : "kNotSupported",
		  "enumBaseType" : "kSnappyCompression",
		  "enumBaseTypeR" : "kNoCompression"
	   }
	}



普通枚举
-----------------------

普通枚举直接进行字符串比较，然后返回枚举名即可。

**驱动代码：**

::

	wings_grammars_test::EnumBase DriverenumEnumBase(string Enumvalue)
	{
		wings_grammars_test::EnumBase _EnumBase_value;
		if (Enumvalue == "kNoCompression") {
			_EnumBase_value = wings_grammars_test::EnumBase::kNoCompression;
		}
		else if (Enumvalue == "kSnappyCompression") {
			_EnumBase_value = wings_grammars_test::EnumBase::kSnappyCompression;
		}   
		return _EnumBase_value;
	}


**普通枚举类型赋值代码：**

::

	int DriverEnumClassTesting::DriverEnumClassTestingEnumBaseFunc0(int times)
	{
		EnumBaseFunc0Times = times;
		/* Root is the json object of the value file.EnumBaseFunc0_Root is function.EnumBaseFunc0 is json object.  */
		const char* jsonFilePath = "drivervalue/EnumClassTesting/EnumBaseFunc0.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _EnumBaseFunc0_Root = Root["EnumBaseFunc0" + std::to_string(times)];
		/*It is the 1 parameter: enumBaseType    EnumBaseFunc0
		 *
		 * Parameters of the prototype:wings_grammars_test::EnumBase enumBaseType     
		 */
		/* enumBaseType */
		string _EnumBase_enumBaseType = _EnumBaseFunc0_Root["enumBaseType"].asString();
		wings_grammars_test::EnumBase _enumBaseType = DriverenumEnumBase(_EnumBase_enumBaseType);
		//The Function of Class    Call
		_EnumClassTesting->EnumBaseFunc(_enumBaseType);
		return 0;
	}


枚举指针
-----------------------

枚举指针当做数组处理，进行相应的参数赋值，和值生成部分相对应起来。

**问题：** 枚举指针为什么当做数组处理？

**答：** 因为大部分的指针都可以当成数组处理，而且普通枚举值本身可以理解为和int是一样的，所以枚举指针的赋值可以作为数组处理。

**驱动代码：**

::

	wings_grammars_test::EnumBase* DriverenumEnumBasePoint(string Enumvalue)
	{
		wings_grammars_test::EnumBase _EnumBase_value;
		if (Enumvalue == "kNoCompression") {
			_EnumBase_value = wings_grammars_test::EnumBase::kNoCompression;
		}
		else if (Enumvalue == "kSnappyCompression") {
			_EnumBase_value = wings_grammars_test::EnumBase::kSnappyCompression;
		}
		wings_grammars_test::EnumBase* _EnumBasePointer = &_EnumBase_value;
		return _EnumBasePointer;
	}


**枚举指针赋值驱动代码：**

::

	int DriverEnumClassTesting::DriverEnumClassTestingCodeFuncPoint3(int times)
	{
		CodeFuncPoint3Times = times;
		/* Root is the json object of the value file.CodeFuncPoint3_Root is function.CodeFuncPoint3 is json object.  */
		const char* jsonFilePath = "drivervalue/EnumClassTesting/CodeFuncPoint3.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _CodeFuncPoint3_Root = Root["CodeFuncPoint3" + std::to_string(times)];
		/*It is the 1 parameter: codeType    CodeFuncPoint3
		 *
		 * Parameters of the prototype:wings_grammars_test::Code *codeType     
		 */
		/* codeType */
		wings_grammars_test::Code* _codeType;
		{
			int W_index = _CodeFuncPoint3_Root["codeType"].size();
			_codeType = new wings_grammars_test::Code[W_index];
			for (int i = 0; i < W_index; i++) {
				string _EnumValuecodeType = _CodeFuncPoint3_Root["codeType"][i].asString();
				_codeType[i] = DriverenumCode(_EnumValuecodeType);
			}
		}
		//The Function of Class    Call
		_EnumClassTesting->CodeFuncPoint(_codeType);
		return 0;
	}


枚举引用
-----------------------

枚举引用的逻辑和普通枚举是一样的，只是在枚举驱动部分返回的是一个枚举的指针。

**驱动代码：**

::

	wings_grammars_test::EnumBase& DriverenumLvalueEnumBase(string Enumvalue)
	{
		wings_grammars_test::EnumBase* _EnumBase_value = new wings_grammars_test::EnumBase();
		if (Enumvalue == "kNoCompression") {
			*_EnumBase_value = wings_grammars_test::EnumBase::kNoCompression;
		}
		else if (Enumvalue == "kSnappyCompression") {
			*_EnumBase_value = wings_grammars_test::EnumBase::kSnappyCompression;
		}  
		return *_EnumBase_value;
	}


**枚举引用赋值驱动代码：**

::

	int DriverEnumClassTesting::DriverEnumClassTestingEnumBaseFuncR1(int times)
	{
		EnumBaseFuncR1Times = times;
		/* Root is the json object of the value file.EnumBaseFuncR1_Root is function.EnumBaseFuncR1 is json object.  */
		const char* jsonFilePath = "drivervalue/EnumClassTesting/EnumBaseFuncR1.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _EnumBaseFuncR1_Root = Root["EnumBaseFuncR1" + std::to_string(times)];
		/*It is the 1 parameter: enumBaseType    EnumBaseFuncR1
		 *
		 * Parameters of the prototype:wings_grammars_test::EnumBase &enumBaseType     
		 */
		/* enumBaseType */
		string _EnumBase_enumBaseType = _EnumBaseFuncR1_Root["enumBaseType"].asString();
		wings_grammars_test::EnumBase& _enumBaseType = DriverenumLvalueEnumBase(_EnumBase_enumBaseType);
		//The Function of Class    Call
		_EnumClassTesting->EnumBaseFuncR(_enumBaseType);
		return 0;
	}


构造函数中对类中枚举类型的成员变量的赋值
-----------------------

因为构造函数中对枚举类型的成员变量的赋值调用的逻辑就是函数中对枚举类型的赋值，所以区别不大。

::

	DriverEnumClassTesting::DriverEnumClassTesting(Json::Value Root, int times)
	{
		Json::Value _EnumClassTesting_Root = Root["EnumClassTesting" + std::to_string(times)];
		/* enumBaseType */
		string _EnumBase_enumBaseType = _EnumClassTesting_Root["enumBaseType"].asString();
		wings_grammars_test::EnumBase _enumBaseType = DriverenumEnumBase(_EnumBase_enumBaseType); 
		/* enumBaseTypeR */
		string _EnumBase_enumBaseTypeR = _EnumClassTesting_Root["enumBaseTypeR"].asString();
		wings_grammars_test::EnumBase& _enumBaseTypeR = DriverenumLvalueEnumBase(_EnumBase_enumBaseTypeR);
		/* codeType */
		string _Code_codeType = _EnumClassTesting_Root["codeType"].asString();
		wings_grammars_test::Code _codeType = DriverenumCode(_Code_codeType); 
		/* codePointType */
		wings_grammars_test::Code* _codePointType;
		{
			int W_index = _EnumClassTesting_Root["codePointType"].size();
			_codePointType = new wings_grammars_test::Code[W_index];
			for (int i = 0; i < W_index; i++) {
				string _EnumValuecodePointType = _EnumClassTesting_Root["codePointType"][i].asString();
				_codePointType[i] = DriverenumCode(_EnumValuecodePointType);
			}
		}
		_EnumClassTesting = new wings_grammars_test::EnumClassTesting(_enumBaseType, _enumBaseTypeR, _codeType, _codePointType, false);
	}
	
	
