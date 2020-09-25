对于STL标准库的Gtest部分的处理
=============

测试例子及Gtest的EXPECT_EQ思想
-----------------------

以下例子用于测试STL标准库的内容，测试包含了STL标准库中的容器作为类的成员变量、结构体的成员变量、函数的返回值、函数的参数以及存在其一级指针和二级指针的情况，此外在考虑了容器之间嵌套和容器内置类型为类的情况。
 
::

	#pragma once
	#include <vector>
	#include <set>
	#include <map>
	#include <list>
	#include <iostream>
	using namespace std;
	class Testshare {
	public:
		int a;
		char ch;
		string str;
		Testshare(int a) {
			cout << a << endl;
		};
		~Testshare() {};
	};
	struct TestStruct
	{
		vector<int> TestV;
		vector<int> *TestVec;
		map<int, string> TestM;
		map<int, string> *TestMaC;
		set<string> *TestSetC;
		set<string> TestStrStr;
		string *str;
		string strt;
		vector<vector<string>> TestVecV;
		vector<map<int, string>> TestVVV;
		vector<map<int, string>> *TestVP;
		vector<vector<Testshare>> TestClass;
		vector<vector<Testshare>> *TestClP;
		map<int,vector<double>> TestMMM;
		map<int,vector<string, string>> *TestMP;
		unique_ptr <int> un1;
		pair<int, string> pa1;
		shared_ptr <string> p1;
		vector<Testshare> TestVecC;
		map<int,Testshare> TestMaC;
		set<Testshare> TestSetC;
	};
	class ClassTest
	{
	private:
		vector<int> TestV;
		vector<int> *TestVec;
		map<int, string> TestM;
		map<int, string> *TestMaC;
		set<string> *TestSetC;
		set<string> TestStrStr;
		string *str;
		string strt;
		vector<vector<string>> TestVecV;
		vector<map<int, string>> TestVVV;
		vector<map<int, string>> *TestVP;
		vector<vector<Testshare>> TestClass;
		vector<vector<Testshare>> *TestClP;
		map<int, vector<double>> TestMMM;
		map<int, vector<string, string>> *TestMP;
		unique_ptr <int> un1;
		pair<int, string> pa1;
		shared_ptr <string> p1;
		vector<Testshare> TestVecC;
		map<int,Testshare> TestMaC;
		set<Testshare> TestSetC;
	public:
		void TestvecReturn(vector<vector<string>> TestVec);
		void TestvecReturnP(vector<vector<string>> *TestVecP);
		void TestmapReturn(map<int, string> *Testmap);
		void Testsetreturn(set<map<int, string>> Testset);
		void TestsetreturnP(set<map<int,string>> *TestsetP);
		void TestCLReturn(set<Testshare> *Testsh);
		void TestStr(string *str);
		void testPairReturn(pair<int, string> *a);
		vector<Testshare> TestReturn();
		void TestStr(string str);
		vector<string> TestCaseVed();
		map<int, string> TestCaseMap();
		set<string> TestCaseSet();
		string TestStrReturn();
	};
	
	
**Gtest的EXPECT_EQ思想**

鉴于STL容器中存在内置类型，我们要确保两个容器中的内容完全一致（即相等），我们使用了将所有的容器进行展开进行比较，即比较两个容器内部进行展开比较。这种方法很好的解决了容器之间嵌套的问题，以及特殊类型作为容器的内置类型的情况（如类类型、string等作为容器的内置类型）。


STL标准库的容器作为类的成员变量
----------------------

STL标准库的容器作为类的成员变量，需要在我们的GTest文件中对类中的每个成员变量进行比较，对于STL容器需要对内部类型进行展开进行比较，分别从_return_actual_Root、_expectreturn_expected_Root中取出实际值和期望值，进行一层层取值比较。

::

	void GtestExpectClassTest(Json::Value _return_actual_Root, Json::Value _expectreturn_expected_Root)
	{
		Json::Value _TestV_actual_Root = _return_actual_Root["TestV"];
		Json::Value _TestV_expected_Root = _expectreturn_expected_Root["TestV"];
		/* TestV */
		int size_TestV = _TestV_actual_Root.size();
		for (auto z = 0; z < size_TestV; z++) {
			/* TestV_0 */
			int _TestV_0_actual = _TestV_actual_Root["TestV_0"].asInt();
			/* TestV_0 */
			int _TestV_0_expected = _TestV_expected_Root["TestV_0"].asInt();
			/* TestV_0 */
			EXPECT_EQ(_TestV_0_actual, _TestV_0_expected);
		}
		Json::Value _TestVec_actual_Root = _return_actual_Root["TestVec"];
		Json::Value _TestVec_expected_Root = _expectreturn_expected_Root["TestVec"];
		/* TestVec */
		int size_TestVec = _TestVec_actual_Root.size();
		for (auto z = 0; z < size_TestVec; z++) {
			int size_TestVec_s = _TestVec_actual_Root[z].size();
			for (auto t = 0; t < size_TestVec_s; t++) {
				/* TestVec_0 */
				int _TestVec_0_actual = _TestVec_actual_Root["TestVec_0"].asInt();
				/* TestVec_0 */
				int _TestVec_0_expected = _TestVec_expected_Root["TestVec_0"].asInt();
				/* TestVec_0 */
				EXPECT_EQ(_TestVec_0_actual, _TestVec_0_expected);
			}
		}
		Json::Value _TestM_actual_Root = _return_actual_Root["TestM"];
		Json::Value _TestM_expected_Root = _expectreturn_expected_Root["TestM"];
		/* TestM */
		int size_TestM = _TestM_actual_Root.size();
		for (auto z = 0; z < size_TestM; z++) {
			/* TestM_0 */
			int _TestM_0_actual = _TestM_actual_Root["TestM_0"].asInt();
			/* TestM_0 */
			int _TestM_0_expected = _TestM_expected_Root["TestM_0"].asInt();
			/* TestM_0 */
			EXPECT_EQ(_TestM_0_actual, _TestM_0_expected);
			Json::Value _TestM_1_actual_Root = _TestM_actual_Root["TestM_1"];
			Json::Value _TestM_1_expected_Root = _TestM_expected_Root["TestM_1"];
			string _TestM_1_actual = _TestM_1_actual_Root["TestM_1"].asString();
			string _TestM_1_expected = _TestM_1_expected_Root["TestM_1"].asString();
			EXPECT_EQ(_TestM_1_actual, _TestM_1_expected);
		}
		Json::Value _TestMaC_actual_Root = _return_actual_Root["TestMaC"];
		Json::Value _TestMaC_expected_Root = _expectreturn_expected_Root["TestMaC"];
		/* TestMaC */
		int size_TestMaC = _TestMaC_actual_Root.size();
		for (auto z = 0; z < size_TestMaC; z++) {
			int size_TestMaC_s = _TestMaC_actual_Root[z].size();
			for (auto t = 0; t < size_TestMaC_s; t++) {
				/* TestMaC_0 */
				int _TestMaC_0_actual = _TestMaC_actual_Root["TestMaC_0"].asInt();
				/* TestMaC_0 */
				int _TestMaC_0_expected = _TestMaC_expected_Root["TestMaC_0"].asInt();
				/* TestMaC_0 */
				EXPECT_EQ(_TestMaC_0_actual, _TestMaC_0_expected);
				Json::Value _TestMaC_1_actual_Root = _TestMaC_actual_Root["TestMaC_1"];
				Json::Value _TestMaC_1_expected_Root = _TestMaC_expected_Root["TestMaC_1"];
				string _TestMaC_1_actual = _TestMaC_1_actual_Root["TestMaC_1"].asString();
				string _TestMaC_1_expected = _TestMaC_1_expected_Root["TestMaC_1"].asString();
				EXPECT_EQ(_TestMaC_1_actual, _TestMaC_1_expected);
			}
		}
	}


StL标准库的容器作为结构体的成员
----------------------

结构体中包含容器的成员变量逻辑和类的成员变量是一致的，再次不过多进行赘述。

::

	void GtestExpectTestStruct(Json::Value _return_actual_Root, Json::Value _expectreturn_expected_Root){
		Json::Value _TestSetC_actual_Root = _return_actual_Root["TestSetC"];
		Json::Value _TestSetC_expected_Root = _expectreturn_expected_Root["TestSetC"];
		/* TestSetC */
		int size_TestSetC = _TestSetC_actual_Root.size();
		for (auto z = 0; z < size_TestSetC; z++) {
			int size_TestSetC_s = _TestSetC_actual_Root[z].size();
			for (auto t = 0; t < size_TestSetC_s; t++) {
				string _TestSetC_0_actual = _return_actual_Root["TestSetC_0"].asString();
				string _TestSetC_0_expected = _expectreturn_expected_Root["TestSetC_0"].asString();
				EXPECT_EQ(_TestSetC_0_actual, _TestSetC_0_expected);
			}
		}
		Json::Value _TestStrStr_actual_Root = _return_actual_Root["TestStrStr"];
		Json::Value _TestStrStr_expected_Root = _expectreturn_expected_Root["TestStrStr"];
		/* TestStrStr */
		int size_TestStrStr = _TestStrStr_actual_Root.size();
		for (auto z = 0; z < size_TestStrStr; z++) {
			string _TestStrStr_0_actual = _return_actual_Root["TestStrStr_0"].asString();
			string _TestStrStr_0_expected = _expectreturn_expected_Root["TestStrStr_0"].asString();
			EXPECT_EQ(_TestStrStr_0_actual, _TestStrStr_0_expected);
		}
		string _str_actual = _return_actual_Root["str"].asString();
		string _str_expected = _expectreturn_expected_Root["str"].asString();
		EXPECT_EQ(_str_actual, _str_expected);
		string _strt_actual = _return_actual_Root["strt"].asString();
		string _strt_expected = _expectreturn_expected_Root["strt"].asString();
		EXPECT_EQ(_strt_actual, _strt_expected);
		Json::Value _TestVecV_actual_Root = _return_actual_Root["TestVecV"];
		Json::Value _TestVecV_expected_Root = _expectreturn_expected_Root["TestVecV"];
		/* TestVecV */
		int size_TestVecV = _TestVecV_actual_Root.size();
		for (auto z = 0; z < size_TestVecV; z++) {
			/* TestVecV_0 */
			int size_TestVecV_0 = _TestVecV_actual_Root.size();
			for (auto z = 0; z < size_TestVecV_0; z++) {
				string _TestVecV_0_0_actual = _return_actual_Root["TestVecV_0_0"].asString();
				string _TestVecV_0_0_expected = _expectreturn_expected_Root["TestVecV_0_0"].asString();
				EXPECT_EQ(_TestVecV_0_0_actual, _TestVecV_0_0_expected);
			}
		}
		Json::Value _TestVVV_actual_Root = _return_actual_Root["TestVVV"];
		Json::Value _TestVVV_expected_Root = _expectreturn_expected_Root["TestVVV"];
		/* TestVVV */
		int size_TestVVV = _TestVVV_actual_Root.size();
		for (auto z = 0; z < size_TestVVV; z++) {
			/* TestVVV_0 */
			int size_TestVVV_0 = _TestVVV_actual_Root.size();
			for (auto z = 0; z < size_TestVVV_0; z++) {
				/* TestVVV_0_0 */
				int _TestVVV_0_0_actual = _TestVVV_0_actual_Root["TestVVV_0_0"].asInt();
				/* TestVVV_0_0 */
				int _TestVVV_0_0_expected = _TestVVV_0_expected_Root["TestVVV_0_0"].asInt();
				/* TestVVV_0_0 */
				EXPECT_EQ(_TestVVV_0_0_actual, _TestVVV_0_0_expected);
				string _TestVVV_0_1_actual = _return_actual_Root["TestVVV_0_1"].asString();
				string _TestVVV_0_1_expected = _expectreturn_expected_Root["TestVVV_0_1"].asString();
				EXPECT_EQ(_TestVVV_0_1_actual, _TestVVV_0_1_expected);
			}
		}
	}


STL作为函数返回值
----------------------

当存在容器作为函数的返回值时，我们需要对函数的返回值进行GTest的比较，我们是从相应的Json的文件中取出对应的实际值和期望值，然后进行比较。

::

	TEST_F(GtestClassTest, DriverClassTestTestcharReturnda7)
	{
		const char* jsonFilePath = "drivervalue/ClassTest/TestcharReturnda7.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		for (int i = 0; i < CLASSTEST_TESTCHARRETURNDA7_TIMES; i++) {
			driverClassTest->DriverClassTestTestcharReturnda7(i);
			Json::Value _TestcharReturnda7_Root = Root["TestcharReturnda7" + std::to_string(i)];
			/* return */
			std::string _return_actualStr = _TestcharReturnda7_actual_Root["return"].asString();
			char _return_actual = _return_actualStr[0];
			/* return */
			std::string _return_expectedStr = _TestcharReturnda7_expected_Root["expectreturn"].asString();
			char _return_expected = _return_expectedStr[0];
			/* return_expected */
			EXPECT_EQ(_return_expected, _return_actual);
		}
	}
	TEST_F(GtestClassTest, DriverClassTestTestEnumCase8)
	{
		const char* jsonFilePath = "drivervalue/ClassTest/TestEnumCase8.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		for (int i = 0; i < CLASSTEST_TESTENUMCASE8_TIMES; i++) {
			driverClassTest->DriverClassTestTestEnumCase8(i);
			Json::Value _TestEnumCase8_Root = Root["TestEnumCase8" + std::to_string(i)];
			/* return */
			int _return_actual = _TestEnumCase8_actual_Root["return"].asInt();
			/* return */
			int _return_expected = _TestEnumCase8_expected_Root["expectreturn"].asInt();
			/* return */
			EXPECT_EQ(_return_actual, _return_expected);
		}
	}

	TEST_F(GtestClassTest, DriverClassTestTestIntretrun9)
	{
		const char* jsonFilePath = "drivervalue/ClassTest/TestIntretrun9.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		for (int i = 0; i < CLASSTEST_TESTINTRETRUN9_TIMES; i++) {
			driverClassTest->DriverClassTestTestIntretrun9(i);
			Json::Value _TestIntretrun9_Root = Root["TestIntretrun9" + std::to_string(i)];
			/* return */
			int _return_actual = _TestIntretrun9_actual_Root["return"].asInt();
			/* return */
			int _return_expected = _TestIntretrun9_expected_Root["expectreturn"].asInt();
			/* return */
			EXPECT_EQ(_return_actual, _return_expected);
		}
	}


STL作为函数参数
----------------------

::

	int DriverClassTest::DriverClassTestTestCLReturn5(int times)
	{
		TestCLReturn5Times = times;
		/* Root is the json object of the value file.TestCLReturn5_Root is function.TestCLReturn5 is json object.  */

		const char* jsonFilePath = "drivervalue/ClassTest/TestCLReturn5.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _TestCLReturn5_Root = Root["TestCLReturn5" + std::to_string(times)];
		/*It is the 1 parameter: Testsh    TestCLReturn5
		 *
		 * Parameters of the prototype:set<Testshare> *Testsh     
		 */
		
		Json::Value _Testsh_RootArr = _TestCLReturn5_Root["Testsh"];
		set<Testshare>* _Testsh = new set<Testshare>();
		int _Testsh_size = _Testsh_RootArr.size();
		for (int Testsh_row = 0; Testsh_row < _Testsh_size; Testsh_row++) {
			Json::Value _Testsh_Root = _Testsh_RootArr[Testsh_row];
		
			Json::Value _Testsh_0Testsh_0_Root = _Testsh_Root["Testsh_0"];
			/* a */
			int _Testsh_0a = _Testsh_0Testsh_0_Root["a"].asInt();
		
			/* ch */
			std::string _Testsh_0chStr = _Testsh_0Testsh_0_Root["ch"].asString();
			char _Testsh_0ch = _Testsh_0chStr[0];
		
			string _Testsh_0str = _Testsh_0Testsh_0_Root["str"].asString();
		
			Testshare _Testsh_0(_Testsh_0a, _Testsh_0ch, _Testsh_0str, false);
		}
		//The Function of Class    Call
		_ClassTest->TestCLReturn(_Testsh);    
		return 0;
	}
	/* Parameterized function processing,Root is the json for this file,Times is the number of tests 

	 * The function prototype: 

	* void TestStr(std::string *str)
		  */
	  int DriverClassTest::DriverClassTestTestStr6(int times)
	  {
	  TestStr6Times = times;
	  /* Root is the json object of the value file.TestStr6_Root is function.TestStr6 is json object.  */

	  const char* jsonFilePath = "drivervalue/ClassTest/TestStr6.json";
	  Json::Value Root;
	  Json::Reader _reader;
	  std::ifstream _ifs(jsonFilePath);
	  _reader.parse(_ifs, Root);
	  Json::Value _TestStr6_Root = Root["TestStr6" + std::to_string(times)];
	  /*It is the 1 parameter: str    TestStr6
	   *

	   * Parameters of the prototype:std::string *str     
		 */

	  std::string* _str = new std::string(_TestStr6_Root["str"].asString());

	  //The Function of Class    Call
	  _ClassTest->TestStr(_str);

	  return 0;
	  }


处理STL的一级指针、二级指针
----------------------

对于STL容器中的一级指针我们视其为一维数组，进行数组的展开比较，在每一层的数组中进行容器的展开比较，即将容器的内置类型进行展开，与上面的逻辑是相同的。

::

	void GtestExpectClassTestPoint(Json::Value ClassTestreturn_actual_Root, Json::Value ClassTestexpectreturn_expected_Root)
	{
		int W_index_return = ClassTestreturn_actual_Root.size();
		int W_index_expectreturn = ClassTestexpectreturn_expected_Root.size();
		if (W_index_return == W_index_expectreturn && W_index_expectreturn != 0) {
			for (int i = 0; i < W_index_expectreturn; i++) {
				Json::Value _return_actual_Root = ClassTestreturn_actual_Root[i];
				Json::Value _expectreturn_expected_Root = ClassTestexpectreturn_expected_Root[i];
				Json::Value _TestV_actual_Root = _return_actual_Root["TestV"];
				Json::Value _TestV_expected_Root = _expectreturn_expected_Root["TestV"];
				/* TestV */
				int size_TestV = _TestV_actual_Root.size();
				for (auto z = 0; z < size_TestV; z++) {
		
					/* TestV_0 */
					int _TestV_0_actual = _TestV_actual_Root["TestV_0"].asInt();
					/* TestV_0 */
					int _TestV_0_expected = _TestV_expected_Root["TestV_0"].asInt();
					/* TestV_0 */
					EXPECT_EQ(_TestV_0_actual, _TestV_0_expected);
				}
		
				Json::Value _TestVec_actual_Root = _return_actual_Root["TestVec"];
				Json::Value _TestVec_expected_Root = _expectreturn_expected_Root["TestVec"];
				/* TestVec */
				int size_TestVec = _TestVec_actual_Root.size();
				for (auto z = 0; z < size_TestVec; z++) {
					int size_TestVec_s = _TestVec_actual_Root[z].size();
					for (auto t = 0; t < size_TestVec_s; t++) {
		
						/* TestVec_0 */
						int _TestVec_0_actual = _TestVec_actual_Root["TestVec_0"].asInt();
						/* TestVec_0 */
						int _TestVec_0_expected = _TestVec_expected_Root["TestVec_0"].asInt();
						/* TestVec_0 */
						EXPECT_EQ(_TestVec_0_actual, _TestVec_0_expected);
					}
				}
		
				Json::Value _TestM_actual_Root = _return_actual_Root["TestM"];
				Json::Value _TestM_expected_Root = _expectreturn_expected_Root["TestM"];
				/* TestM */
				int size_TestM = _TestM_actual_Root.size();
				for (auto z = 0; z < size_TestM; z++) {
		
					/* TestM_0 */
					int _TestM_0_actual = _TestM_actual_Root["TestM_0"].asInt();
					/* TestM_0 */
					int _TestM_0_expected = _TestM_expected_Root["TestM_0"].asInt();
					/* TestM_0 */
					EXPECT_EQ(_TestM_0_actual, _TestM_0_expected);
		
					Json::Value _TestM_1_actual_Root = _TestM_actual_Root["TestM_1"];
					Json::Value _TestM_1_expected_Root = _TestM_expected_Root["TestM_1"];
		
					string _TestM_1_actual = _TestM_1_actual_Root["TestM_1"].asString();
					string _TestM_1_expected = _TestM_1_expected_Root["TestM_1"].asString();
					EXPECT_EQ(_TestM_1_actual, _TestM_1_expected);
				}
	}


特殊情况
----------------------

当一些特殊类型作为容器的内置类型时，我们有不同的处理方式（暂时存在string、类类型后续会支持更多）。

**string**

string类型本身是容器，但它的比较是不需要进行展开的，我们是直接进行转化为string类型，即直接进行字符串比较。

**class（类类型）**

两个类之间是无法直接比较的，我们的处理方法是对类进行展开，比较类中的成员变量的值是否相同，对于类我们生成相应的成员变量比较的函数，当容器中出现类类型时，调用相应的函数比较函数即可。
