模板容器类型
=============

测试源码
-----------------------

测试类型：vector的普通类型，指针类型，数组类型，模板参数为结构体指针，模板参数为pair类型（其他容器处理方法基本相同）

::

	#pragma once

	#include <iostream>
	#include <string>
	#include <vector>

	using namespace std;

	namespace MySpace
	{
		class FileMateData
		{
		private:
			int matedata_;
			string mma;
		};

		struct FileDate
		{

		};

		class STLTesting
		{
		public:

			std::vector<std::string> returnvector()
			{
				return std::vector<std::string>();
			}
		private:
			std::string str_;
			std::string *pstr_;
			std::vector<std::string> builit_vec;
			std::vector<std::string> *pvec;
			std::vector<int> vec[2];
			std::vector<std::pair<int, FileDate>> *pvecs;
			std::vector<FileMateData *> *input;
		};

	}


插装后的源码
^^^^^^^^

因为需要对类中的成员变量进行赋值，所有会在驱动生成的时候对类进行一些插装，通过插装的驱动函数对类成员变量完成初始化赋值。
插装构造函数额外添加一个bool变量防止出现构造函数冲突。

::

	#include"json/json.h"
	#pragma once

	#include <iostream>
	#include <string>
	#include <vector>

	using namespace std;

	namespace MySpace
	{
		class FileMateData
		{
		private:
			int matedata_;
			string mma;
		public:
		FileMateData(int matedata_, std::string mma, bool Wings):matedata_(matedata_), mma(mma)
		{}
	};

		struct FileDate
		{

		};

		class STLTesting
		{
		private:
			std::string str_;
			std::string *pstr_;
			std::vector<std::string> builit_vec;
			std::vector<std::string> *pvec;
			std::vector<int> vec[2];
			std::vector<std::pair<int, FileDate>> *pvecs;
			std::vector<FileMateData *> *input;
		public:
		STLTesting(std::string str_, std::string *pstr_, std::vector<std::string> builit_vec, std::vector<std::string> *pvec, std::vector<int> vec[2], std::vector<std::pair<int, FileDate> > *pvecs, std::vector<FileMateData *> *input, bool Wings):str_(str_), pstr_(pstr_), builit_vec(builit_vec), pvec(pvec), pvecs(pvecs), input(input)
		{/* vec */
					for(unsigned int size = 0; size < 2; size++)
					{
						this->vec[size] = vec[size];
					}
					}
		};

	}



驱动文件生成
----------------------

**驱动类的构造函数**
头文件

::

	#include "E:/wuqingwen/vscode/TestTemplate/TestTemplate/FileMateData.h"
	#include "driver.h"
	#include "driver_Enum.h"
	#include "driver_structorunion.h"
	class DriverSTLTesting {
	public:
		DriverSTLTesting(Json::Value Root, int times);
		~DriverSTLTesting();

	private:
		MySpace::STLTesting* _STLTesting;
	};


源文件

::

	/*The total header file of the tested file needed */
	/*The header file corresponding to the source file */
	#include "driverSTLTesting.h"
	DriverSTLTesting::DriverSTLTesting(Json::Value Root, int times)
	{
		Json::Value _STLTesting_Root = Root["STLTesting" + std::to_string(times)];
		string _str_ = _STLTesting_Root["str_"].asString();

		std::string* _pstr_ = new std::string(_STLTesting_Root["pstr_"].asString());

		Json::Value _builit_vec_RootArr = _STLTesting_Root["builit_vec"];
		vector<std::string> _builit_vec;
		int _builit_vec_size = _builit_vec_RootArr.size();
		for (int builit_vec_row = 0; builit_vec_row < _builit_vec_size; builit_vec_row++) {
			Json::Value _builit_vec_Root = _builit_vec_RootArr[builit_vec_row];

			string _builit_vec_0 = _builit_vec_Root["builit_vec_0"].asString();

			_builit_vec.push_back(_builit_vec_0);
		}
		Json::Value _pvec_RootArr = _STLTesting_Root["pvec"];
		vector<std::string>* _pvec = new vector<std::string>();
		int _pvec_size = _pvec_RootArr.size();
		for (int pvec_row = 0; pvec_row < _pvec_size; pvec_row++) {
			Json::Value _pvec_Root = _pvec_RootArr[pvec_row];

			string _pvec_0 = _pvec_Root["pvec_0"].asString();

			_pvec->push_back(_pvec_0);
		}
		/* vec */
		vector<int> _vec[2];
		for (int len = 0; len < 2; len++) {
			Json::Value _vec_RootArr = _STLTesting_Root["vec"][len];

			int _vec_size = _vec_RootArr.size();
			for (int vec_row = 0; vec_row < _vec_size; vec_row++) {
				Json::Value _vec_Root = _vec_RootArr[vec_row];

				/* vec_0 */
				int _vec_0 = _vec_Root["vec_0"].asInt();

				_vec[len].push_back(_vec_0);
			}
		}
		Json::Value _pvecs_RootArr = _STLTesting_Root["pvecs"];
		vector<pair<int, MySpace::FileDate>>* _pvecs = new vector<pair<int, MySpace::FileDate>>();
		int _pvecs_size = _pvecs_RootArr.size();
		for (int pvecs_row = 0; pvecs_row < _pvecs_size; pvecs_row++) {
			Json::Value _pvecs_Root = _pvecs_RootArr[pvecs_row];

			Json::Value _pvecs_0_RootArr = _pvecs_Root["pvecs_0"];
			Json::Value _pvecs_0_Root = _pvecs_0_RootArr[0];
			/* pvecs_0_0 */
			int _pvecs_0_0 = _pvecs_0_Root["pvecs_0_0"].asInt();
			/* pvecs_0_1 */
			Json::Value _pvecs_0_1_Root = _pvecs_0_Root["pvecs_0_1"];

			struct MySpace::FileDate _pvecs_0_1 = DriverstructFileDate(_pvecs_0_1_Root);

			std::pair<int, MySpace::FileDate> _pvecs_0;
			_pvecs_0 = std::pair<int, MySpace::FileDate>(_pvecs_0_0, _pvecs_0_1);

			_pvecs->push_back(_pvecs_0);
		}
		Json::Value _input_RootArr = _STLTesting_Root["input"];
		vector<MySpace::FileMateData*>* _input = new vector<MySpace::FileMateData*>();
		int _input_size = _input_RootArr.size();
		for (int input_row = 0; input_row < _input_size; input_row++) {
			Json::Value _input_Root = _input_RootArr[input_row];

			int _input_0pointSize = 0;
			Json::Value _input_0input_0_Root = _input_Root["input_0"][_input_0pointSize];
			/* matedata_ */
			int _input_0matedata_ = _input_0input_0_Root["matedata_"].asInt();

			string _input_0mma = _input_0input_0_Root["mma"].asString();

			MySpace::FileMateData* _input_0 = new MySpace::FileMateData(_input_0matedata_, _input_0mma, false);

			_input->push_back(_input_0);
		}
		_STLTesting = new MySpace::STLTesting(_str_, _pstr_, _builit_vec, _pvec, _vec, _pvecs, _input, false);
	}


string类型赋值部分
^^^^^^^^
直接从json中取出对应 的值进行赋值.

::

	std::string* _pstr_ = new std::string(_STLTesting_Root["pstr_"].asString());
	

vector基础类型赋值
^^^^^^^^
赋值过程：定义变量, json取值，构造容器元素对象，填充进容器，循环执行。

::

	Json::Value _builit_vec_RootArr = _STLTesting_Root["builit_vec"];
		vector<std::string> _builit_vec;
		int _builit_vec_size = _builit_vec_RootArr.size();
		for (int builit_vec_row = 0; builit_vec_row < _builit_vec_size; builit_vec_row++) {
			Json::Value _builit_vec_Root = _builit_vec_RootArr[builit_vec_row];

			string _builit_vec_0 = _builit_vec_Root["builit_vec_0"].asString();

			_builit_vec.push_back(_builit_vec_0);
		}


vector指针类型
^^^^^^^^

::

	Json::Value _pvec_RootArr = _STLTesting_Root["pvec"];
		vector<std::string>* _pvec = new vector<std::string>();
		int _pvec_size = _pvec_RootArr.size();
		for (int pvec_row = 0; pvec_row < _pvec_size; pvec_row++) {
			Json::Value _pvec_Root = _pvec_RootArr[pvec_row];

			string _pvec_0 = _pvec_Root["pvec_0"].asString();

			_pvec->push_back(_pvec_0);
		}


vector数组类型
^^^^^^^^

::

	vector<int> _vec[2];
		for (int len = 0; len < 2; len++) {
			Json::Value _vec_RootArr = _STLTesting_Root["vec"][len];

			int _vec_size = _vec_RootArr.size();
			for (int vec_row = 0; vec_row < _vec_size; vec_row++) {
				Json::Value _vec_Root = _vec_RootArr[vec_row];

				/* vec_0 */
				int _vec_0 = _vec_Root["vec_0"].asInt();

				_vec[len].push_back(_vec_0);
			}
		}


vector参数为模板类
^^^^^^^^

::

	Json::Value _input_RootArr = _STLTesting_Root["input"];
		vector<MySpace::FileMateData*>* _input = new vector<MySpace::FileMateData*>();
		int _input_size = _input_RootArr.size();
		for (int input_row = 0; input_row < _input_size; input_row++) {
			Json::Value _input_Root = _input_RootArr[input_row];

			int _input_0pointSize = 0;
			Json::Value _input_0input_0_Root = _input_Root["input_0"][_input_0pointSize];
			/* matedata_ */
			int _input_0matedata_ = _input_0input_0_Root["matedata_"].asInt();

			string _input_0mma = _input_0input_0_Root["mma"].asString();

			MySpace::FileMateData* _input_0 = new MySpace::FileMateData(_input_0matedata_, _input_0mma, false);

			_input->push_back(_input_0);
		}


构造被测类对象
^^^^^^^^

::

	_STLTesting = new MySpace::STLTesting(_str_, _pstr_, _builit_vec, _pvec, _vec, _pvecs, _input, false);
	

该类的测试用例生成
----------------------

此为被测类的值文件
容器默认为生成三组值生成元素的值，容器数组根据数组数生成元素的值.

::

	"STLTesting0" : {
      "builit_vec" : [
         {
            "builit_vec_0" : "G7B"
         },
         {
            "builit_vec_0" : "YKi"
         },
         {
            "builit_vec_0" : "pym"
         }
      ],
      "input" : [
         {
            "input_0" : [
               {
                  "matedata_" : 390,
                  "mma" : "Qth"
               }
            ]
         },
         {
            "input_0" : [
               {
                  "matedata_" : 2501,
                  "mma" : "Pke"
               }
            ]
         },
         {
            "input_0" : [
               {
                  "matedata_" : 6069,
                  "mma" : "xiD"
               }
            ]
         }
      ],
      "pstr_" : "Vdd",
      "pvec" : [
         {
            "pvec_0" : "jKJ"
         },
         {
            "pvec_0" : "zwW"
         },
         {
            "pvec_0" : "LGT"
         }
      ],
      "pvecs" : [
         {
            "pvecs_0" : [
               {
                  "pvecs_0_0" : 3141,
                  "pvecs_0_1" : null
               }
            ]
         },
         {
            "pvecs_0" : [
               {
                  "pvecs_0_0" : 9514,
                  "pvecs_0_1" : null
               }
            ]
         },
         {
            "pvecs_0" : [
               {
                  "pvecs_0_0" : 348,
                  "pvecs_0_1" : null
               }
            ]
         }
      ],
      "str_" : "5pD",
      "vec" : [
         [
            {
               "vec_0" : 3805
            },
            {
               "vec_0" : 8407
            },
            {
               "vec_0" : 167
            }
         ],
         [
            {
               "vec_0" : 1839
            },
            {
               "vec_0" : 9528
            },
            {
               "vec_0" : 2960
            }
         ]
      ]
   }


对参数有模板类的驱动测试
----------------------

对之前的类添加成员函数进行测试,在成员函数中对输入进去的数值进行打印。
因为fileDate中没有类型，这里我们只对了pair的int进行了打印。

::

	void TestVectorPair(std::vector<std::pair<int, FileDate>> *pvecs);
	
	
驱动代码
^^^^^^^^

::

	int DriverSTLTestingTestVectorPair6(int times);
	int TestVectorPair6Times;
	/* Parameterized function processing,Root is the json for this file,Times is the number of tests 
	 * The function prototype: 
	* void TestVectorPair(std::vector<std::pair<int, FileDate> > *pvecs)
				*/
	int DriverSTLTesting::DriverSTLTestingTestVectorPair6(int times)
	{
		TestVectorPair6Times = times;
		/* Root is the json object of the value file.TestVectorPair6_Root is function.TestVectorPair6 is json object.  */

		const char* jsonFilePath = "E:\\wuqingwen\\vscode\\TemplateValueTest\\drivervalue/STLTesting/TestVectorPair6.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _TestVectorPair6_Root = Root["TestVectorPair6" + std::to_string(times)];
		/*It is the 1 parameter: pvecs    TestVectorPair6
		 *
		 * Parameters of the prototype:std::vector<std::pair<int, FileDate> > *pvecs     
		 */

		Json::Value _pvecs_RootArr = _TestVectorPair6_Root["pvecs"];
		vector<std::pair<int, MySpace::FileDate>>* _pvecs = new vector<std::pair<int, MySpace::FileDate>>();
		int _pvecs_size = _pvecs_RootArr.size();
		for (int pvecs_row = 0; pvecs_row < _pvecs_size; pvecs_row++) {
			Json::Value _pvecs_Root = _pvecs_RootArr[pvecs_row];

			Json::Value _pvecs_0_RootArr = _pvecs_Root["pvecs_0"];
			Json::Value _pvecs_0_Root = _pvecs_0_RootArr[0];
			/* pvecs_0_0 */
			int _pvecs_0_0 = _pvecs_0_Root["pvecs_0_0"].asInt();
			/* pvecs_0_1 */
			Json::Value _pvecs_0_1_Root = _pvecs_0_Root["pvecs_0_1"];

			struct MySpace::FileDate _pvecs_0_1 = DriverstructFileDate(_pvecs_0_1_Root);

			std::pair<int, struct MySpace::FileDate> _pvecs_0;
			_pvecs_0 = std::pair<int, struct MySpace::FileDate>(_pvecs_0_0, _pvecs_0_1);

			_pvecs->push_back(_pvecs_0);
		}
		//The Function of Class    Call
		_STLTesting->TestVectorPair(_pvecs);

		return 0;
	}


此函数值文件
^^^^^^^^

::

	{
	   "TestVectorPair60" : {
		  "pvecs" : [
			 {
				"pvecs_0" : [
				   {
					  "pvecs_0_0" : 1976,
					  "pvecs_0_1" : null
				   }
				]
			 },
			 {
				"pvecs_0" : [
				   {
					  "pvecs_0_0" : 1829,
					  "pvecs_0_1" : null
				   }
				]
			 },
			 {
				"pvecs_0" : [
				   {
					  "pvecs_0_0" : 3988,
					  "pvecs_0_1" : null
				   }
				]
			 }
		  ]
	   }
	}


支持类型
----------------------

目前支持：vector，map，stack，set，list，deque，arrat
