类的运算符重载
=============

**逻辑思想：**

运算符重载函数分为成员函数重载和非成员函数重载（即friend），两者的驱动生成会有所不同，主要表现在调用原函数上；首先在函数中将运算符重载函数分离出来；再将运算符重载函数区分为非成员函数（friend）和成员函数；然后分别进行具体的运算符进行驱动生成，不同的运算符会有不同处理。

**问题：** 为什么要将运算符重载函数与普通函数分离？

**答：** 在生成驱动时，运算符重载成员函数与类的普通成员函数区别在于类名以及参数，前者会有一个运算符的后缀，以及在成员函数的运算符重载中，有一个this指针隐含的传过去。

**问题：** 为什么要将运算符重载函数区分为非成员函数（friend）和成员函数？

**答：** 因为friend和普通函数的处理会有不同，主要表现在非成员重载函数正常会比成员重载函数多一个参数，成员函数的运算符重载中，有一个this指针隐含的传过去，以及在调用部分也会不同，成员重载函数可以直接使用类指针进行调用，而friend不一样。

**问题：** 为什么要对不同的操作符进行不同的处理？

**答：** 因为操作符直接的差异很大，有的并不需要参数，而需要参数的一部分是人为赋值的参数，一部分是系统参数，需要分开处理。


测试的源码
-----------------------

::

	#pragma warning(disable:4996)
	#include"json/json.h"
	#include <iostream>
	#include <fstream>
	using namespace std;
	#pragma once
	class ClassOpertor
	{
	public:
		ClassOpertor();
		~ClassOpertor();
		ClassOpertor operator- ();
		bool operator<(const ClassOpertor& d);
		ClassOpertor operator+(ClassOpertor&add);
		ClassOpertor operator+(int b);
		void displayDistance();
		friend ClassOpertor operator+(const int b, ClassOpertor obj);
		friend ClassOpertor operator+(const ClassOpertor& a, const ClassOpertor& b);
		ostream &operator<<(ostream &output);
		friend ostream &operator<<(ostream &output, const ClassOpertor &Dis);
		ClassOpertor operator++();//前缀形式
		ClassOpertor operator++(int);//后缀形式
		int fun();
		bool operator!();
		void * operator new(size_t size);//一个参数，为size_t类型
		void operator delete(void *ptr);// void 类型的指针作为参数
		void operator=(const ClassOpertor &D);
		void operator+=(const ClassOpertor &D);
		int& operator[](int i);//下标
		ClassOpertor& operator*();//指针
		ClassOpertor* operator&();//取地址符
		int operator()(int val); 
		ClassOpertor* operator->();
		void operator delete[](void*, size_t size);
	private:
		int length;
		int weight;
		int arr[10];
		ClassOpertor *persion;
	public:
	ClassOpertor(int length, int weight, int arr[10], bool Wings):length(length), weight(weight)
	{
		/* arr */
	   for(unsigned int size = 0; size < 10; size++)
	   {
		   this->arr[size] = arr[size];
	   }
	}
	};



成员函数的处理（篇幅原因，每种只列标志性的运算符重载的驱动代码）
-----------------------

**1.算数运算符**

+、-、*、/、%、++、--

加法（+）测试源码：

::

	Operatortor Operatortor::operator+=(int len)
	{
		length += len;
		return Operatortor();
	}

加法（+）驱动：

::

	int DriverClassOpertor::DriverClassOpertoroperator2(int times)
	{
		operator2Times = times;
		const char* jsonFilePath = "drivervalue/ClassOpertor/operator2.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator2_Root = Root["operator2" + std::to_string(times)];
		Json::Value _addadd_Root = _operator2_Root["add"];
		/* length */
		int _addlength = _addadd_Root["length"].asInt();
		/* weight */
		int _addweight = _addadd_Root["weight"].asInt();
		/* arr */
		int _addarr[10];
		for (int len = 0; len < 10; len++) {
			_addarr[len] = _addadd_Root["arr"][len].asInt();
		}
		ClassOpertor _addadd(_addlength, _addweight, _addarr, false);
		_ClassOpertor->operator+(_addadd);
		return 0;
	}


前置自增（++length）测试源码：

::

	Operatortor Operatortor::operator++()
	{
		++length;
		if (length >= 60)
		{
			++weight;
			length -= 60;
		}
		return ClassOpertor();
	}

前置自增（++length）驱动：

::

	int DriverOperatortor::DriverOperatortoroperator8(int times)
	{
		_ClassOpertor->operator++();
		return 0;
	}


**2.关系运算符**

<、>、==、>=、<=、!=

小于号（<）源码：

::

	bool Operatortor::operator<(const Operatortor & d)
	{
		if (length < d.length)
		{
			return true;
		}
		if (length == d.length && weight < d.weight)
		{
			return true;
		}
		return false;
	}
	
	
小于号（<）驱动：

::

	int DriverOperatortor::DriverOperatortoroperator1(int times)
	{
		operator1Times = times;
		const char* jsonFilePath = "drivervalue/Operatortor/operator1.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator1_Root = Root["operator1" + std::to_string(times)];
		Json::Value _d_Root = _operator1_Root["d"];
		/* length */
		int _dlength = _d_Root["length"].asInt();
		/* weight */
		int _dweight = _d_Root["weight"].asInt();  
		Operatortor _dd(_dlength, _dweight, false);
	   // The Function of Class    Call
		_ClassOpertor->operator<(_dd);
		return 0;
	}


小于等于号（<=）驱动：

::

	int DriverOperatortor::DriverOperatortoroperator1(int times)
	{
		operator1Times = times;
		const char* jsonFilePath = "drivervalue/Operatortor/operator2.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator2_Root = Root["operator2" + std::to_string(times)];
		Json::Value _d_Root = _operator2_Root["d"];
		/* length */
		int _dlength = _d_Root["length"].asInt();
		/* weight */
		int _dweight = _d_Root["weight"].asInt();  
		Operatortor _dd(_dlength, _dweight, false);
	   // The Function of Class    Call
		_ClassOpertor->operator<=(_dd);
		return 0;
	}


**3.赋值运算符**

-=、+=、/=、*=、%=、&=、 ^=、 |=、 <<=、 >>=、=

=赋值运算符

::

	int DriverClassOpertor::DriverClassOpertoroperator12(int times)
	{
		operator12Times = times;
		const char* jsonFilePath = "drivervalue/ClassOpertor/operator12.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator12_Root = Root["operator12" + std::to_string(times)];
		Json::Value _DD_Root = _operator12_Root["D"];
		/* length */
		int _Dlength = _DD_Root["length"].asInt();
		/* weight */
		int _Dweight = _DD_Root["weight"].asInt();
		/* arr */
		int _Darr[10];
		for (int len = 0; len < 10; len++) {
			_Darr[len] = _DD_Root["arr"][len].asInt();
		}
		ClassOpertor _DD(_Dlength, _Dweight, _Darr, false);
		//The Function of Class    Call
		_ClassOpertor->operator=(_DD);
		return 0;
	}


+=赋值运算符

::

	int DriverClassOpertor::DriverClassOpertoroperator13(int times)
	{
		operator13Times = times;
		const char* jsonFilePath = "drivervalue/ClassOpertor/operator13.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator13_Root = Root["operator13" + std::to_string(times)];
		Json::Value _DD_Root = _operator13_Root["D"];
		/* length */
		int _Dlength = _DD_Root["length"].asInt(); 
		/* weight */
		int _Dweight = _DD_Root["weight"].asInt();
		/* arr */
		int _Darr[10];
		for (int len = 0; len < 10; len++) {
			_Darr[len] = _DD_Root["arr"][len].asInt();
		}
		ClassOpertor _DD(_Dlength, _Dweight, _Darr, false);
		//The Function of Class    Call
		_ClassOpertor->operator+=(_DD);
		return 0;
	}


**4.单目运算符**

+（正）、-（负）、*（指针）、&（取地址）

&(取地址)

::

	int DriverClassOpertor::DriverClassOpertoroperator16(int times)
	{
		operator16Times = times;
		const char* jsonFilePath = "drivervalue/ClassOpertor/operator16.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator16_Root = Root["operator16" + std::to_string(times)];
		_ClassOpertor->operator&();
		return 0;
	}
	
	
-（负）运算符

::

	int DriverClassOpertor::DriverClassOpertoroperator0(int times)
	{
		_ClassOpertor->operator-();
		return 0;
	}


**5.逻辑运算符和位运算符**

&、|、^、&&、||、！、<<(左移)、>>(右移)、~（按位取反）

&&和||不建议重载运算符。

**问题：** 为什么不建议重载&&和||？

**答：** 逻辑&&和逻辑||运算符是可以重载的，但是重载不能实现逻辑&&和逻辑||运算符的短路功能。

::

	!=（逻辑非运算符）
	int DriverClassOpertor::DriverClassOpertoroperator9(int times)
	{
		operator9Times = times;
		const char* jsonFilePath = "drivervalue/ClassOpertor/operator9.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator9_Root = Root["operator9" + std::to_string(times)];
		_ClassOpertor->operator!();
		return 0;
	}



**6.流运算符**

<<、>>

对于C++的输入输出流的重载函数来说，参数output我们在驱动赋值时并不需要去定义和赋值，可以直接使用cout，即标准c++的输出函数。

<<输出流的源码：

::

	ostream & ClassOpertor::operator<<(ostream & output)
	{
		return output;
	}
	<<输出流运算符
	int DriverClassOpertor::DriverClassOpertoroperator5(int times)
	{	
		_ClassOpertor->operator<<(cout);
		return 0;
	}



**7.空间申请与释放**

new、delete、new[]、delete[]

new运算符

::

	int DriverClassOpertor::DriverClassOpertoroperator10(int times)
	{
		operator10Times = times;
		const char* jsonFilePath = "drivervalue/ClassOpertor/operator10.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator10_Root = Root["operator10" + std::to_string(times)];
		/* size */
		unsigned int _size = _operator10_Root["size"].asUInt();
		_ClassOpertor->operator new(_size);
		return 0;
	}


delete运算符

::

	int DriverClassOpertor::DriverClassOpertoroperator11(int times)
	{
		operator11Times = times;
		const char* jsonFilePath = "drivervalue/ClassOpertor/operator11.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator11_Root = Root["operator11" + std::to_string(times)];
		/* ptr */
		void* _ptr = nullptr;
		//The Function of Class    Call
		_ClassOpertor->operator delete(_ptr);
		return 0;
	}


**8.特殊运算符**

->（类成员访问运算符）、()（函数运算符）、[]（下标运算符）、co_await、,(逗号)）

[]下标运算符源码

::

	int & ClassOpertor::operator[](int i)
	{
		int SIZE = 10;
		if (i > SIZE)
		{
			cout << "索引超过最大值" << endl;
			return arr[0];
		}
		return arr[i];
	}
	

[]下标运算符驱动

::

	int DriverClassOpertor::DriverClassOpertoroperator14(int times)
	{
		operator14Times = times;
		const char* jsonFilePath = "drivervalue/ClassOpertor/operator14.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator14_Root = Root["operator14" + std::to_string(times)];
		/* i */
		int _i = _operator14_Root["i"].asInt();
		_ClassOpertor->operator[](_i);
		return 0;
	}


()函数运算符

::

	int DriverClassOpertor::DriverClassOpertoroperator17(int times)
	{
		operator17Times = times;
		const char* jsonFilePath = "drivervalue/ClassOpertor/operator17.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator17_Root = Root["operator17" + std::to_string(times)];
		/* val */
		int _val = _operator17_Root["val"].asInt();
		_ClassOpertor->operator()(_val);
		return 0;
	}


->(类成员访问符)

::

	int DriverClassOpertor::DriverClassOpertoroperator9(int times)
	{
		_ClassOpertor->operator->();
		return 0;
	}


，（逗号运算符）

::

	int DriverClassOpertor::DriverClassOpertoroperator2(int times)
	{
		operator2Times = times;
		const char* jsonFilePath = "drivervalue/ClassOpertor/operator18.json";
		Json::Value Root;
		Json::Reader _reader;
		std::ifstream _ifs(jsonFilePath);
		_reader.parse(_ifs, Root);
		Json::Value _operator2_Root = Root["operator2" + std::to_string(times)];  
		Json::Value _rdd_Root = _operator2_Root["add"];
		/* length */
		int _rddlength = _rdd_Root["length"].asInt();
		/* weight */
		int _rddweight = _rdd_Root["weight"].asInt(); 
		/* arr */
		int _addarr[10];
		for (int len = 0; len < 10; len++) {
			_rddarr[len] = _rdd_Root["arr"][len].asInt();
		}  
		ClassOpertor _rdd(_rddlength, _rddweight, _rddarr, false);
		_ClassOpertor->operator->(_rdd); 
		return 0;
	}



非成员函数（friend）的处理
-----------------------

非成员的重载函数在驱动生成上和成员函数区别并不是很大，主要表现在以下两点：

1）函数参数上，作为成员函数重载时，，有一个this指针隐含的传过去，不需要驱动中给它额外赋值；非成员的重载函数则没有，相同符号的重载，后者会比前者多一个参数，这里需要驱动给它赋值。

2）调用原函数上，作为成员函数重载时，可以使用类指针进行调用重载函数，非成员的重载函数则不需要，可以直接使用。