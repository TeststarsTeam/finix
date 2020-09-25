类A包含类B指针，类B包含类A变量
=============

源码（插装过后）
-----------------------

::

	#include"json/json.h"
	#pragma once

	#include <iostream>
	#include <string>

	class MyPointer;

	class MyObject
	{
	public:
		void show() const
		{
			std::cout << "this is MyObject" << std::endl;
			std::cout << "my data: " << data_ << std::endl;
			if (m_pointer_ != nullptr) std::cout << "m_object show(): " << std::endl;
		}
	private:
		const std::string data_;
		MyPointer* m_pointer_;
	public:
	friend class DriverMyObject;
	MyObject(std::string data_, MyPointer *m_pointer_, bool Wings):data_(data_), m_pointer_(m_pointer_)
	{}
	};

	class MyPointer
	{
	public:
		void show() const
		{
			std::cout << "this is MyPointer" << std::endl;
			std::cout << "my data: " << data_ << std::endl;
			std::cout << "m_object show(): " << std::endl;
			m_object_.show();
		}
	private:
		std::string data_;
		MyObject m_object_;
	public:
	friend class DriverMyPointer;
	MyPointer(std::string data_, MyObject m_object_, bool Wings):data_(data_), m_object_(m_object_)
	{}
	MyPointer()
	{}
	};


初版解决方法
----------------------

**驱动生成**

1.成员变量是类，将其类型存放在vector中（vector中初始存放驱动生成所生成的类的类型）

2.对该类进行驱动生成，处理其成员变量

3.遇见成员变量为类，与vector中对比

4.如果存在相同数据的停止，此成员变量指针赋值为空，普通的类对象不再处理

5.不同将类型放入vector中，继续执行2


出现问题
^^^^^^^^

**以MyObject类为例子**

vector中存放顺序：

MyObject， MyPointer， 然后就遇到 MyPointer中的成员变量MyObject m_object_，与MyObject相同，所有停止赋值，不对它进行处理。

如果MyObject不存在默认构造函数，此时m_object_就无法生成，出现错误


解决方法
^^^^^^^^

在3中对比判断之前，先判断成员变量是否为指针，指针则对比，非指针，就存入vector中继续处理.

**以MyObject类为例子**

vector中存放顺序：

1.MyObject

2.MyObject， MyPointer

3.MyObject， MyPointer， MyObject

4.MyPointer* m_pointer_ ;为指针，且有相同数据。驱动生成 MyPointer* m_pointer_ = nullptr；


驱动代码
----------------------

主要是两个类的构造代码

::

	//类MyObject
	DriverMyObject::DriverMyObject(Json::Value Root, int times)
	{
		Json::Value _MyObject_Root = Root["MyObject" + std::to_string(times)];
		std::string _data_ = _MyObject_Root["data_"].asString();

		int _m_pointer_pointSize = 0;
		Json::Value _m_pointer_m_pointer__Root = _MyObject_Root["m_pointer_"][_m_pointer_pointSize];
		std::string _m_pointer_data_ = _m_pointer_m_pointer__Root["data_"].asString();

		Json::Value _m_pointer_m_object_m_object__Root = _m_pointer_m_pointer__Root["m_object_"];
		std::string _m_pointer_m_object_data_ = _m_pointer_m_object_m_object__Root["data_"].asString();

		MyPointer* _m_pointer_m_object_m_pointer_ = nullptr;

		MyObject _m_pointer_m_object_(_m_pointer_m_object_data_, _m_pointer_m_object_m_pointer_, false);

		MyPointer* _m_pointer_ = new MyPointer(_m_pointer_data_, _m_pointer_m_object_, false);

		_MyObject = new MyObject(_data_, _m_pointer_, false);
	}
	//类MyPointer
	DriverMyPointer::DriverMyPointer(Json::Value Root, int times)
	{
		Json::Value _MyPointer_Root = Root["MyPointer" + std::to_string(times)];
		std::string _data_ = _MyPointer_Root["data_"].asString();

		Json::Value _m_object_m_object__Root = _MyPointer_Root["m_object_"];
		std::string _m_object_data_ = _m_object_m_object__Root["data_"].asString();

		MyPointer* _m_object_m_pointer_ = nullptr;

		MyObject _m_object_(_m_object_data_, _m_object_m_pointer_, false);

		_MyPointer = new MyPointer(_data_, _m_object_, false);
	}

