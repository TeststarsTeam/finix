参数捕获测试Demo
=============

前言
-----------------------

参数捕获是通过在用户源码中插装wings生成的参数捕获代码进行获取函数的参数数据，对于基本类型在获取到数据就可以直接存放入json文件中，用于后续驱动代码的读取；对于类、结构体等特殊数据类型，我们选择的是将类和结构体的内部数据进行统一处理，生成对应类的W_MemberVarCaputer()以及结构体的驱动函数，对应生成两个结构体和类的文件WingsClassCapture和paramcapture_structorunion。
PS：当遇到自定义模板类时，上述类的方法不可行，所以自定义模板类是通过直接插装在源代码的头文件中，然后在参数捕获代码中进行调用，当自定义模板类中存在类类型、结构体等复杂数据类型时暂不支持，因为会造成头文件循环包含问题。


自定义模板类型的参数捕获
----------------------

测试demo
^^^^^^^^

::

	#include"json/json.h"
	#pragma once
	#pragma warning(disable:4996)
	#include <iostream>
	#include <fstream>
	using namespace std;
	namespace Test
	{
		struct student
		{
			int a;
			char b;
			string str;
	};
		class Teststr
		{
		private:
			int a;
		public:
	};
		template<typename T1, typename T2>
		class TestTemplate
		{
		private:
			T1 *t1;
			T2 t2;
			int a;
			char b;
			string s;
			student stu;
	public:
	//此函数为插装进入的类参数捕获函数
		Json::Value W_MemberVarCaputer()
		{
			Json::Value TestTemplate_Root;
			/*t1*/
			TestTemplate_Root["t1"] = Json::Value(t1);
			/*t2*/
			TestTemplate_Root["t2"] = Json::Value(t2);
			/*a*/
			TestTemplate_Root["a"] = Json::Value(a);
			/*b*/
			char _b[2];
			_b[0] = b;
			_b[1] = '\0';
			TestTemplate_Root["b"] = Json::Value(_b);
			TestTemplate_Root["s"] = Json::Value(s);
			/* stu */
			/*Json::Value stu_Root;
			stu_Root = Paramstruct_Test_student(stu_Root, stu);//结构体和类目前无法调用该函数，会造成头文件循环包含问题
			TestTemplate_Root["stu"] = stu_Root;*/
			return TestTemplate_Root;
	}
	}
		class ClassTest
		{
		public:
		ClassTest(){};
		~ClassTest() {};
		void TestTemplateParam(TestTemplate<int, int> TePl);
		void TestTempate(TestTemplate<string, int>  TePo);
		void TestClassNamespace(Teststr te);
		};
	}


参数捕获代码
^^^^^^^^

::

	被测试函数：void TestTemplateParam(TestTemplate<int, int> TePl);
	int ParamCaptureTest_ClassTestTestTemplateParam1Times = -1;
	void ParamCaptureTest_ClassTest::ParamCapture_TestTemplateParam1(Test::TestTemplate<int, int> TePl)
	{
		ParamCaptureTest_ClassTestTestTemplateParam1Times++;
		Json::Value Root;
		Json::Value TestTemplateParam1_Root;
		const char* JsonFilePath = "paramcapturevalue/Test_ClassTest/TestTemplateParam1.json";
		if (ParamCaptureTest_ClassTestTestTemplateParam1Times != 0) {
		} else {
			Json::Reader _reader;
			std::ifstream _ifs(JsonFilePath);
			_reader.parse(_ifs, Root);
			TestTemplateParam1_Root = Root["TestTemplateParam1" + std::to_string(ParamCaptureTest_ClassTestTestTemplateParam1Times)];
		}

		/*It is the 1 parameter: TePl    TestTemplateParam1
		 *
		 * Parameters of the prototype:TestTemplate<int, int> TePl     
		 */

		/* TePl */
		TestTemplateParam1_Root["TePl"] = TePl.W_MemberVarCaputer();

		Root["TestTemplateParam1" + std::to_string(ParamCaptureTest_ClassTestTestTemplateParam1Times)] = TestTemplateParam1_Root;
		std::ofstream JsonFile;
		JsonFile.open(JsonFilePath);
		Json::StyledWriter sw;
		JsonFile << sw.write(Root);
		JsonFile.close();
	}
	void ParamCaptureTest_ClassTest::GlobalCapture_TestTemplateParam1()
	{
		const char* JsonFilePath = "paramcapturevalue/Test_ClassTest/TestTemplateParam1.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(JsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value TestTemplateParam1_Root = Root["TestTemplateParam1" + std::to_string(ParamCaptureTest_ClassTestTestTemplateParam1Times)];

		Root["TestTemplateParam1" + std::to_string(ParamCaptureTest_ClassTestTestTemplateParam1Times)] = TestTemplateParam1_Root;
		std::ofstream JsonFile;
		Json::StyledWriter sw;
		JsonFile.open(JsonFilePath);
		JsonFile << sw.write(Root);
		JsonFile.close();
	}
	void ParamCaptureTest_ClassTest::ReturnCapture_TestTemplateParam1()
	{
		const char* JsonFilePath = "paramcapturevalue/Test_ClassTest/TestTemplateParam1.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(JsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value TestTemplateParam1_Root = Root["TestTemplateParam1" + std::to_string(ParamCaptureTest_ClassTestTestTemplateParam1Times)];

		Root["TestTemplateParam1" + std::to_string(ParamCaptureTest_ClassTestTestTemplateParam1Times)] = TestTemplateParam1_Root;
		std::ofstream JsonFile;
		Json::StyledWriter sw;
		JsonFile.open(JsonFilePath);
		JsonFile << sw.write(Root);
		JsonFile.close();
	}


枚举类型的参数捕获
----------------------

测试demo
^^^^^^^^

::

	#include"json/json.h"
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
			EnumBase &enumBaseTypeR = enumBaseType;
			Code codeType;
			Code *codePointType;
		public:
			void EnumBaseFunc(wings_grammars_test::EnumBase enumBaseType);
			void EnumBaseFuncR(wings_grammars_test::EnumBase &enumBaseType);
			void CodeFunc(wings_grammars_test::Code codeType);
		public:
		Json::Value W_MemberVarCaputer();
		};
	}


参数捕获代码
^^^^^^^^

::

	对应的测试函数：void CodeFunc(wings_grammars_test::Code codeType);

	int ParamCapturewings_grammars_test_EnumClassTestingCodeFunc2Times = -1;
	void ParamCapturewings_grammars_test_EnumClassTesting::ParamCapture_CodeFunc2(wings_grammars_test::Code codeType)
	{
		ParamCapturewings_grammars_test_EnumClassTestingCodeFunc2Times++;
		Json::Value Root;
		Json::Value CodeFunc2_Root;
		const char* JsonFilePath = "paramcapturevalue/wings_grammars_test_EnumClassTesting/CodeFunc2.json";
		if (ParamCapturewings_grammars_test_EnumClassTestingCodeFunc2Times != 0) {
		}
		else {
			Json::Reader _reader;
			std::ifstream _ifs(JsonFilePath);
			_reader.parse(_ifs, Root);
			CodeFunc2_Root = Root["CodeFunc2" + std::to_string(ParamCapturewings_grammars_test_EnumClassTestingCodeFunc2Times)];
		}
		/*It is the 1 parameter: codeType    CodeFunc2
		 *
		 * Parameters of the prototype:wings_grammars_test::Code codeType
		*/
		 /*codeType*/
		string codeType_enum;
		if (codeType == wings_grammars_test::Code::kOk) {
			codeType_enum = "kOk";
		}
		if (codeType == wings_grammars_test::Code::kNotFound) {
			codeType_enum = "kNotFound";
		}
		if (codeType == wings_grammars_test::Code::kCorruption) {
			codeType_enum = "kCorruption";
		}
		if (codeType == wings_grammars_test::Code::kNotSupported) {
			codeType_enum = "kNotSupported";
		}
		if (codeType == wings_grammars_test::Code::kInvalidArgument) {
			codeType_enum = "kInvalidArgument";
		}
		if (codeType == wings_grammars_test::Code::kIOError) {
			codeType_enum = "kIOError";
		}
		CodeFunc2_Root["codeType"] = Json::Value(codeType_enum);
		Root["CodeFunc2" + std::to_string(ParamCapturewings_grammars_test_EnumClassTestingCodeFunc2Times)] = CodeFunc2_Root;
		std::ofstream JsonFile;
		JsonFile.open(JsonFilePath);
		Json::StyledWriter sw;
		JsonFile << sw.write(Root);
		JsonFile.close();
	}


参数捕获代码解析
^^^^^^^^

枚举一般分为普通枚举和强枚举类型（c++11之后），对于两种枚举的详细区别可自行学习，在此简述，强枚举类型定义的变量只能使用该枚举类型去赋值，而普通枚举值可以简单的理解为int型，是可以给其赋int型值的；针对这种情况，我们是通过对枚举类型赋值字符串的形式，然后在对应参数不好中给其添加对应枚举类型的前缀（即达到给对应枚举类型赋值的目的）。


STL标准库容器参数捕获
----------------------

测试demo
^^^^^^^^

::

	#include"json/json.h"
	#pragma warning(disable:4996)
	#pragma once
	#include <iostream>
	#include <vector>
	#include<map>
	#include <set>
	using namespace std;
	namespace TestTest
	{
	class TestOne
	{
	private:
		int On;
	public:
		TestOne() {};
		~TestOne() {};
	};
	class ClassTest
	{
	private:
		int a;
		char b;
		const char* data_;
		class Tes;
	public:
		//测试STL模板类
		void TestString(string str);
		void TestStringPoint(string* strP);
		void TestVector(vector<string> vec);
		void TestMap(map<string,string> Ma);
		void TestSet(set<int> Se);
		void TestStringArray(string strss[3]);
		ClassTest(int a) :a(a) {};
		~ClassTest() {};
	};
	}


参数捕获代码
^^^^^^^^

::

	被测试函数：void TestVector(vector<string> vec);
	int TestVector6Times = -1;
	void ParamCaptureClassTest::ParamCapture_TestVector6(vector<int, std::string> vec)
	{
		TestVector6Times++;
		Json::Value Root;
		Json::Value TestVector6_Root;
		const char* JsonFilePath = "TestVector6.json";
		if (TestVector6Times != 0) {
		} else {
			Json::Reader _reader;
			std::ifstream _ifs(JsonFilePath);
			_reader.parse(_ifs, Root);
			TestVector6_Root = Root["TestVector6" + std::to_string(TestVector6Times)];
		}

		/*It is the 1 parameter: vec    TestVector6
		 *
		 * Parameters of the prototype:vector<int, std::string> vec     
		 */
		
		/*vec*/
		Json::Value vec_Root;
		int size_vec = vec.size();
		for (auto t = 0; t < size_vec; t++) {
			vec_Root.append(vec.at(t));
		}
		
		TestVector6_Root.append(vec_Root);
		
		Root["TestVector6" + std::to_string(TestVector6Times)] = TestVector6_Root;
		std::ofstream JsonFile;
		JsonFile.open(JsonFilePath);
		Json::StyledWriter sw;
		JsonFile << sw.write(Root);
		JsonFile.close();

	}
	void ParamCaptureClassTest::GlobalCapture_TestVector6()
	{
		const char* JsonFilePath = "paramcapturevalue/ClassTest/TestVector6.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(JsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value TestVector6_Root = Root["TestVector6" + std::to_string(TestVector6Times)];

		Root["TestVector6" + std::to_string(TestVector6Times)] = TestVector6_Root;
		std::ofstream JsonFile;
		Json::StyledWriter sw;
		JsonFile.open(JsonFilePath);
		JsonFile << sw.write(Root);
		JsonFile.close();

	}
	void ParamCaptureClassTest::ReturnCapture_TestVector6()
	{
		const char* JsonFilePath = "paramcapturevalue/ClassTest/TestVector6.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(JsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value TestVector6_Root = Root["TestVector6" + std::to_string(TestVector6Times)];

		Root["TestVector6" + std::to_string(TestVector6Times)] = TestVector6_Root;
		std::ofstream JsonFile;
		Json::StyledWriter sw;
		JsonFile.open(JsonFilePath);
		JsonFile << sw.write(Root);
		JsonFile.close();

	}


	被测试函数：void TestMap(map<string,string> Ma);
	int TestMap7Times = -1;
	void ParamCaptureClassTest::ParamCapture_TestMap7(map<std::string, std::string> Ma)
	{
		TestMap7Times++;
		Json::Value Root;
		Json::Value TestMap7_Root;
		const char* JsonFilePath = "TestMap7.json";
		if (TestMap7Times != 0) {
		} else {
			Json::Reader _reader;
			std::ifstream _ifs(JsonFilePath);
			_reader.parse(_ifs, Root);
			TestMap7_Root = Root["TestMap7" + std::to_string(TestMap7Times)];
		}
		/*It is the 1 parameter: Ma    TestMap7
		 *
		 * Parameters of the prototype:map<std::string, std::string> Ma     
		 */
		/*Ma*/
		Json::Value Ma_Root;
		int size_Ma = Ma.size();
		for (auto i : Ma)
		{
			Ma_Root["Ma_0"] = i.first;
			Ma_Root["Ma_1"] = i.second;
		}
		TestMap7_Root.append(Ma_Root);
		Root["TestMap7" + std::to_string(TestMap7Times)] = TestMap7_Root;
		std::ofstream JsonFile;
		JsonFile.open(JsonFilePath);
		Json::StyledWriter sw;
		JsonFile << sw.write(Root);
		JsonFile.close();
	}
	void ParamCaptureClassTest::GlobalCapture_TestMap7()
	{
		const char* JsonFilePath = "paramcapturevalue/ClassTest/TestMap7.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(JsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value TestMap7_Root = Root["TestMap7" + std::to_string(TestMap7Times)];
		Root["TestMap7" + std::to_string(TestMap7Times)] = TestMap7_Root;
		std::ofstream JsonFile;
		Json::StyledWriter sw;
		JsonFile.open(JsonFilePath);
		JsonFile << sw.write(Root);
		JsonFile.close();
	}
	void ParamCaptureClassTest::ReturnCapture_TestMap7()
	{
		const char* JsonFilePath = "paramcapturevalue/ClassTest/TestMap7.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(JsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value TestMap7_Root = Root["TestMap7" + std::to_string(TestMap7Times)];
		Root["TestMap7" + std::to_string(TestMap7Times)] = TestMap7_Root;
		std::ofstream JsonFile;
		Json::StyledWriter sw;
		JsonFile.open(JsonFilePath);
		JsonFile << sw.write(Root);
		JsonFile.close();
	}


参数捕获代码解析
^^^^^^^^

对于每种STL容器的取值方式不同，所以大部分都需要特殊处理，如map、pair、string等（以下简介几种）；
string类型的处理：普通的直接作为字符串处理，string数组则作为字符串数组；
map类型的处理：由于map最常用的就是一组key对应一组value，所以采用只赋值这两组，对于它还存在的排列组合方式则不考虑；
pair类型的处理：pair就是一组map，所以相同的方式存放一组值就可以；
vector类型的处理：vector通过遍历的方式，将所有值都取出来进行存放。


基本类型的参数捕获
----------------------

测试demo
^^^^^^^^

::

	#include"json/json.h"
	#pragma warning(disable:4996)
	#pragma once
	#include <iostream>
	#include <vector>
	#include<map>
	#include <set>
	using namespace std;
	namespace TestTest
	{
		struct Test
		{
			int in;
			char che;
			char * chP;
		};
		enum Te
		{
			one,
			two
		};
		class TestOne
		{
		private:
			int On;
		public:
			TestOne() {};
			~TestOne() {};
			//测试枚举类型
		};
		class ClassTest
		{
		private:
			int a;
			char b;
			const char* data_;
			class Tes;
		public:
			//测试基本类型
			int TestIntReturn(int Param);
			char TestCharReturn(char ch);
			Test TeststructReturn(Test te);
			const char* TestCharPoint();
			TestOne TestClassReturn();
			void TestClassFun(TestOne tss);
			void TestFun1(double de, long lo);
			void TestFun2(short sh, float fl);
			void TestFun(uint16_t uin, unsigned char ch);
			void TestFun4(unsigned long lo);
			//测试类类型
			void TestClassbuitin(TestOne te);
			void TestClassPoint(TestOne* Poin);
			//测试基本类型的指针
			void TestIntPoint(int* a,char * ch);
			void TestdoublePonit(long* lo,double* d);
			ClassTest(int a) :a(a) {};
			~ClassTest() {};
		};
	}


参数捕获代码
^^^^^^^^

::

	被测试函数：void TestIntPoint(int* a,char * ch);
	int TestIntPoint2Times = -1;
	void ParamCaptureClassTest::ParamCapture_TestIntPoint2(int* a, char* ch)
	{
		TestIntPoint2Times++;
		Json::Value Root;
		Json::Value TestIntPoint2_Root;
		const char* JsonFilePath = "paramcapturevalue/ClassTest/TestIntPoint2.json";
		if (TestIntPoint2Times != 0) {
		} else {
			Json::Reader _reader;
			std::ifstream _ifs(JsonFilePath);
			_reader.parse(_ifs, Root);
			TestIntPoint2_Root = Root["TestIntPoint2" + std::to_string(TestIntPoint2Times)];
		}

		/*It is the 1 parameter: a    TestIntPoint2
		 *
		 * Parameters of the prototype:int *a     
		 */
		
		/*a*/
		Json::Value Arr_a;
		for (int row = 0; row < 1; row++) {
			Arr_a.append(Json::Value(a[row]));
		}
		TestIntPoint2_Root["a"] = Arr_a;
		
		/*It is the 2 parameter: ch    TestIntPoint2
		 *
		 * Parameters of the prototype:char *ch     
		 */
		
		/*ch*/
		TestIntPoint2_Root["ch"] = Json::Value(ch);
		
		Root["TestIntPoint2" + std::to_string(TestIntPoint2Times)] = TestIntPoint2_Root;
		std::ofstream JsonFile;
		JsonFile.open(JsonFilePath);
		Json::StyledWriter sw;
		JsonFile << sw.write(Root);
		JsonFile.close();

	}
	被测试函数：void TestdoublePonit(long* lo,double* d);
	int TestdoublePonit3Times = -1;
	void ParamCaptureClassTest::ParamCapture_TestdoublePonit3(long* lo, double* d)
	{
		TestdoublePonit3Times++;
		Json::Value Root;
		Json::Value TestdoublePonit3_Root;
		const char* JsonFilePath = "paramcapturevalue/ClassTest/TestdoublePonit3.json";
		if (TestdoublePonit3Times != 0) {
		} else {
			Json::Reader _reader;
			std::ifstream _ifs(JsonFilePath);
			_reader.parse(_ifs, Root);
			TestdoublePonit3_Root = Root["TestdoublePonit3" + std::to_string(TestdoublePonit3Times)];
		}

		/*It is the 1 parameter: lo    TestdoublePonit3
		 *
		 * Parameters of the prototype:long *lo     
		 */

		/*lo*/
		Json::Value Arr_lo;
		for (int row = 0; row < 1; row++) {
			Arr_lo.append(Json::Value(lo[row]));
		}
		TestdoublePonit3_Root["lo"] = Arr_lo;

		/*It is the 2 parameter: d    TestdoublePonit3
		 *
		 * Parameters of the prototype:double *d     
		 */

		/*d*/
		Json::Value Arr_d;
		for (int row = 0; row < 1; row++) {
			Arr_d.append(Json::Value(d[row]));
		}
		TestdoublePonit3_Root["d"] = Arr_d;

		Root["TestdoublePonit3" + std::to_string(TestdoublePonit3Times)] = TestdoublePonit3_Root;
		std::ofstream JsonFile;
		JsonFile.open(JsonFilePath);
		Json::StyledWriter sw;
		JsonFile << sw.write(Root);
		JsonFile.close();
	}


类类型和结构体类型的参数捕获
----------------------

测试demo
^^^^^^^^

::

	#include"json/json.h"
	#pragma warning(disable:4996)
	#pragma once
	#include <iostream>
	#include <string>
	class WingsClassCapture;
	namespace mySpace
	{
		struct student
		{
			int age_;
			std::string name_;
			bool sex;
			FILE *fptr;
		};
		class ClassSecond;
		class ClassFirst
		{
		public:
			friend WingsClassCapture;
			ClassFirst() {}
			ClassFirst(int i ,student s) :  num_(i), stu_(s)
			{
			}
		private:
			int num_;
			void *ptr_;
			int(*mma)(int, int);
			FILE *fptr;
			student stu_;
			ClassSecond *chd_;
		};
		class ClassSecond
		{
		public:
			friend WingsClassCapture;
			ClassSecond() {}
			int num_;
			void *ptr_;
			int(*mma)(int, int);
			FILE *fptr;
			ClassFirst chd_;
		};
		class ClassTest
		{
		public:
			friend WingsClassCapture;
			//类中包含结构体
			ClassFirst showtestobj(ClassFirst obj);
			ClassFirst* showtestptr(ClassFirst *ptr);
			student showstructtestobj(student obj);
			student* showstructtestptr(student *ptr);
		};
	}


参数捕获代码
^^^^^^^^

::

	被测试函数：ClassFirst showtestobj(ClassFirst obj);
	int ParamCapturemySpace_ClassTestshowtestobj0Times = -1;
	void ParamCapturemySpace_ClassTest::ParamCapture_showtestobj0(mySpace::ClassFirst obj)
	{
		ParamCapturemySpace_ClassTestshowtestobj0Times++;
		Json::Value Root;
		Json::Value showtestobj0_Root;
		const char* JsonFilePath = "D:/showtestobj0.json";
		if (ParamCapturemySpace_ClassTestshowtestobj0Times != 0) {
		} else {
			Json::Reader _reader;
			std::ifstream _ifs(JsonFilePath);
			_reader.parse(_ifs, Root);
			showtestobj0_Root = Root["showtestobj0" + std::to_string(ParamCapturemySpace_ClassTestshowtestobj0Times)];
		}

		/*It is the 1 parameter: obj    showtestobj0
		 *
		 * Parameters of the prototype:mySpace::ClassFirst obj     
		 */

		/* obj */
		WingsClassCapture wings_capture_class;
		showtestobj0_Root["obj"] = wings_capture_class.mySpace_ClassFirst_W_MemberVarCaputer(obj);

		Root["showtestobj0" + std::to_string(ParamCapturemySpace_ClassTestshowtestobj0Times)] = showtestobj0_Root;
		std::ofstream JsonFile;
		JsonFile.open(JsonFilePath);
		Json::StyledWriter sw;
		JsonFile << sw.write(Root);
		JsonFile.close();
	}
	被测试函数：ClassFirst* showtestptr(ClassFirst *ptr);
	int ParamCapturemySpace_ClassTestshowtestptr1Times = -1;
	void ParamCapturemySpace_ClassTest::ParamCapture_showtestptr1(mySpace::ClassFirst* ptr)
	{
		ParamCapturemySpace_ClassTestshowtestptr1Times++;
		Json::Value Root;
		Json::Value showtestptr1_Root;
		const char* JsonFilePath = "D:/showtestptr1.json";
		if (ParamCapturemySpace_ClassTestshowtestptr1Times != 0) {
		} else {
			Json::Reader _reader;
			std::ifstream _ifs(JsonFilePath);
			_reader.parse(_ifs, Root);
			showtestptr1_Root = Root["showtestptr1" + std::to_string(ParamCapturemySpace_ClassTestshowtestptr1Times)];
		}
		/*It is the 1 parameter: ptr    showtestptr1
		 *
		 * Parameters of the prototype:mySpace::ClassFirst *ptr     
		 */

		/* ptr */
		if (ptr != nullptr) {
			WingsClassCapture wings_capture_class;
			showtestptr1_Root["ptr"] = wings_capture_class.mySpace_ClassFirst_W_MemberVarCaputer(*ptr);
		}
		Root["showtestptr1" + std::to_string(ParamCapturemySpace_ClassTestshowtestptr1Times)] = showtestptr1_Root;
		std::ofstream JsonFile;
		JsonFile.open(JsonFilePath);
		Json::StyledWriter sw;
		JsonFile << sw.write(Root);
		JsonFile.close();
	}
	被测函数：student showstructtestobj(student obj);
	int ParamCapturemySpace_ClassTestshowstructtestobj2Times = -1;
	void ParamCapturemySpace_ClassTest::ParamCapture_showstructtestobj2(mySpace::student obj)
	{
		ParamCapturemySpace_ClassTestshowstructtestobj2Times++;
		Json::Value Root;
		Json::Value showstructtestobj2_Root;
		const char* JsonFilePath = "paramcapturevalue/mySpace_ClassTest/showstructtestobj2.json";
		if (ParamCapturemySpace_ClassTestshowstructtestobj2Times != 0) {
		} else {
			Json::Reader _reader;
			std::ifstream _ifs(JsonFilePath);
			_reader.parse(_ifs, Root);
			showstructtestobj2_Root = Root["showstructtestobj2" + std::to_string(ParamCapturemySpace_ClassTestshowstructtestobj2Times)];
		}

		/*It is the 1 parameter: obj    showstructtestobj2
		 *
		 * Parameters of the prototype:mySpace::student obj     
		 */

		/* obj */
		Json::Value obj_Root;
		obj_Root = Paramstruct_mySpace_student(obj_Root, obj);
		showstructtestobj2_Root["obj"] = obj_Root;

		Root["showstructtestobj2" + std::to_string(ParamCapturemySpace_ClassTestshowstructtestobj2Times)] = showstructtestobj2_Root;
		std::ofstream JsonFile;
		JsonFile.open(JsonFilePath);
		Json::StyledWriter sw;
		JsonFile << sw.write(Root);
		JsonFile.close();
	}
	void ParamCapturemySpace_ClassTest::GlobalCapture_showstructtestobj2()
	{
		const char* JsonFilePath = "paramcapturevalue/mySpace_ClassTest/showstructtestobj2.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(JsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value showstructtestobj2_Root = Root["showstructtestobj2" + std::to_string(ParamCapturemySpace_ClassTestshowstructtestobj2Times)];

		Root["showstructtestobj2" + std::to_string(ParamCapturemySpace_ClassTestshowstructtestobj2Times)] = showstructtestobj2_Root;
		std::ofstream JsonFile;
		Json::StyledWriter sw;
		JsonFile.open(JsonFilePath);
		JsonFile << sw.write(Root);
		JsonFile.close();
	}
	void ParamCapturemySpace_ClassTest::ReturnCapture_showstructtestobj2(mySpace::student returnType)
	{
		const char* JsonFilePath = "paramcapturevalue/mySpace_ClassTest/showstructtestobj2.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(JsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value showstructtestobj2_Root = Root["showstructtestobj2" + std::to_string(ParamCapturemySpace_ClassTestshowstructtestobj2Times)];

		/* returnType */
		Json::Value returnType_Root;
		returnType_Root = Paramstruct_mySpace_student(returnType_Root, returnType);
		showstructtestobj2_Root["returnType"] = returnType_Root;

		Root["showstructtestobj2" + std::to_string(ParamCapturemySpace_ClassTestshowstructtestobj2Times)] = showstructtestobj2_Root;
		std::ofstream JsonFile;
		Json::StyledWriter sw;
		JsonFile.open(JsonFilePath);
		JsonFile << sw.write(Root);
		JsonFile.close();
	}
	生成的类的参数捕获调用函数
	#include "WingsClassCapture.h"
	#include "paramcapture_structorunion.h"

	Json::Value WingsClassCapture::mySpace_ClassFirst_W_MemberVarCaputer(mySpace::ClassFirst temp_wings)
	{
		Json::Value mySpace_ClassFirst_Root;
		/*num_*/
		mySpace_ClassFirst_Root["num_"] = Json::Value(temp_wings.num_);
		/*ptr_*/
		mySpace_ClassFirst_Root["ptr_"] = Json::Value();
		/* stu_ */
		Json::Value stu__Root;
		stu__Root = Paramstruct_mySpace_student(stu__Root, temp_wings.stu_);
		mySpace_ClassFirst_Root["stu_"] = stu__Root;

		/* chd_ */
		if (temp_wings.chd_ != nullptr) {
			WingsClassCapture wings_capture_class;
			mySpace_ClassFirst_Root["chd_"] = wings_capture_class.mySpace_ClassSecond_W_MemberVarCaputer(*temp_wings.chd_);
		}
		return mySpace_ClassFirst_Root;
	}

	Json::Value WingsClassCapture::mySpace_ClassSecond_W_MemberVarCaputer(mySpace::ClassSecond temp_wings)
	{
		Json::Value mySpace_ClassSecond_Root;
		/*num_*/
		mySpace_ClassSecond_Root["num_"] = Json::Value(temp_wings.num_);
		/*ptr_*/
		mySpace_ClassSecond_Root["ptr_"] = Json::Value();
		/* chd_ */
		WingsClassCapture wings_capture_class;
		mySpace_ClassSecond_Root["chd_"] = wings_capture_class.mySpace_ClassFirst_W_MemberVarCaputer(temp_wings.chd_);
		return mySpace_ClassSecond_Root;
	}

	Json::Value WingsClassCapture::mySpace_ClassTest_W_MemberVarCaputer(mySpace::ClassTest temp_wings)
	{
		Json::Value mySpace_ClassTest_Root;
		return mySpace_ClassTest_Root;
	}
	生成的结构体的参数捕获调用函数
	#include "paramcapture_structorunion.h"
	Json::Value Paramstruct_TestTest_Test(Json::Value Test_Root, struct TestTest::Test a)
	{
		/*in*/
		Test_Root["in"] = Json::Value(a.in);
		/*che*/
		char _che[2];
		_che[0] = a.che;
		_che[1] = '\0';
		Test_Root["che"] = Json::Value(_che);
		/*chP*/
		Test_Root["chP"] = Json::Value(a.chP);  
		return Test_Root;

	}

	Json::Value Paramstruct_TestTest_Test_Point(Json::Value ArrayRoot, struct TestTest::Test* a, int row)
	{
		if (a == nullptr) {
			return ArrayRoot;
		}
		for (int i = 0; i < row; i++) {
			Json::Value Test_Root;
			/*in*/
			Test_Root["in"] = Json::Value(a[i].in);
			/*che*/
			char _che[2];
			_che[0] = a[i].che;
			_che[1] = '\0';
			Test_Root["che"] = Json::Value(_che);
			/*chP*/
			Test_Root["chP"] = Json::Value(a[i].chP);
		
			ArrayRoot.append(Test_Root);
		}
		return ArrayRoot;
	}

	Json::Value Paramstruct_TestTest_Test_PointPoint(Json::Value SECArrayRoot, struct TestTest::Test** a, int row, int column)
	{
		if (a == nullptr) {
			return SECArrayRoot;
		}
		for (int i = 0; i < row; i++) {
			Json::Value ArrayRoot;
			for (int j = 0; j < column; j++) {
				Json::Value Test_Root;
				/*in*/
				Test_Root["in"] = Json::Value(a[i][j].in);
		
				/*che*/
				char _che[2];
				_che[0] = a[i][j].che;
				_che[1] = '\0';
				Test_Root["che"] = Json::Value(_che);
		
				/*chP*/
				Test_Root["chP"] = Json::Value(a[i][j].chP);
		
				ArrayRoot.append(Test_Root);
			}
			SECArrayRoot.append(ArrayRoot);
		}
		return SECArrayRoot;
	}



参数捕获代码解析
^^^^^^^^

由上面代码可以看到，类、结构体都是通过生成对应的函数，然后调用该函数进行赋值操作，当在结构体、类的成员变量中含有类或结构体时，会重复这个过程，即调用相对应的类或结构体的参数捕获函数。