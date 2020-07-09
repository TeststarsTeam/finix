c++驱动代码 
=============================================
c++中主要是针对每个类进行测试，wings会自动生成每个类的驱动代码以及对应的gtest代码。


c++驱动代码的命名规则
-----------------------

每个类生成驱动对应的文件名为：driver+类名.cc 以及.h，驱动类的名字为：Driver+ClassName(类名)，c++中存在很多运算符重载的函数，为了保证函数的唯一性，针对类的函数从0开始进行编号处理，即Driver+函数名+编号。


c++ 驱动代码的详细介绍 
-----------------------

c++中会针对每个类生成对应的测试类代码，如下所示，Rectangle为被测试类，包含3个成员变量的类型分别为 Point、 int 、std::string,假设Rectangle作为函数参数，则此类的对象构造需要对3个成员变量进行赋值操作。

::

	#include "Rectangle.h"
	#include "driver.h"
	class DriverRectangle {
	public:
	  DriverRectangle(Json::Value Root, int times);
	  ~DriverRectangle();
	  int DriverRectangleGetVolume0(int times);
	  void ReturnDriver_GetVolume0(int returnType);
	  int GetVolume0Times;
	  int DriverRectanglePrintVolume1(int times);
	  int PrintVolume1Times;
	private:
	  Rectangle *_Rectangle;
	};

wings会自动针对每个类在源代码中插入一个构造函数如下所示，对成员变量进行赋值操作。

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
			//LOGI("Rectangle::Rectangle");
			this->m_point = m_point;
			this->m_z = m_z;
			this->name = name;
		}
	};
	

针对每个驱动类，对应的会生成一个构造函数，来初始化被测试类，如下所示：

::

	DriverRectangle::DriverRectangle(Json::Value Root, int times) {
	  Json::Value Rectangle_Root = Root["Rectangle" + to_string(times)];
	  int pointSize = 0;
	  Json::Value m_point_Root = Rectangle_Root["m_point"][pointSize];
	  /* m_x */
	  int _m_point_m_x = m_point_Root["m_x"].asInt();
	  /* m_y */
	  int _m_point_m_y = m_point_Root["m_y"].asInt();
	  Point *_m_point = new Point(_m_point_m_x, _m_point_m_y, false);
	  /* m_z */
	  int _m_z = Rectangle_Root["m_z"].asInt();
	  string _name = Rectangle_Root["name"].asString();
	  _Rectangle = new Rectangle(_m_point, _m_z, _name, false);
	}
	DriverRectangle::~DriverRectangle() {
	  if (_Rectangle != nullptr) {
		delete _Rectangle;
	  }
	}
	

驱动类中，针对每个函数生成一个驱动函数。如下所示：

::

	int DriverRectangle::DriverRectanglePrintVolume1(int times) {
	  PrintVolume1Times = times;
	  /* Root is the json object of the value file.PrintVolume1_Root is
	   * function.PrintVolume1 is json object.  */
	  const char *jsonFilePath = "drivervalue/Rectangle/PrintVolume1.json";
	  Json::Value Root;
	  Json::Reader _reader;
	  ifstream _ifs(jsonFilePath);
	  _reader.parse(_ifs, Root);
	  Json::Value PrintVolume1_Root = Root["PrintVolume1" + to_string(times)];
	  /*It is the 1 parameter: rect    PrintVolume1*/
	  int pointSize = 0;
	  Json::Value rect_Root = PrintVolume1_Root["rect"][pointSize];
	  int pointSize = 0;
	  Json::Value m_point_Root = rect_Root["m_point"][pointSize];
	  /* m_x */
	  int _rect_m_point_m_x = m_point_Root["m_x"].asInt();
	  /* m_y */
	  int _rect_m_point_m_y = m_point_Root["m_y"].asInt();
	  Point *_rect_m_point = new Point(_rect_m_point_m_x, _rect_m_point_m_y, false);
	  /* m_z */
	  int _rect_m_z = rect_Root["m_z"].asInt();
	  string _rect_name = rect_Root["name"].asString();
	  Rectangle *_rect = new Rectangle(_rect_m_point, _rect_m_z, _rect_name, false);
	  /*It is the 2 parameter: parm    PrintVolume1*/
	  int _parm = PrintVolume1_Root["parm"].asInt();
	  // The Function of Class    Call
	  _Rectangle->PrintVolume(_rect, _parm);
	  return 0;
	}


每个驱动类，对应一个gtest调用类，来进行期望的对比，如下所示

.. image:: /image/figure18.png

gtest函数的对比如下图所示：

.. image:: /image/figure18.png

