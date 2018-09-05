测试用例存储与展示
===================

finix测试数据描述
-----------------
finix通过编译技术提取函数参数信息与对应的全局变量信息，利用这些信息生成对应的测试用例，底层保存为json格式的形式
finix为了更好的展示测试数据，采用数据表格可以表达任意深度和多层次的数据关系，用户只需要对表格数据进行编辑，
自动生成的驱动程序会自动完成表格数据的读取和参数赋值的构造过程。

Testing example
----------------
code::

  typedef struct my_structone 
  {
	//基本类型
	int i_int;

	//数组类型
	int array_one[2];
	int array_two[3][4];

	//指针类型
	int *point_one;
	int **point_two;

	//空类型
	void *point;

	//位域类型
	unsigned int w : 1;

	//函数指针是指向函数的指针变量，即本质是一个指针变量
	int(*functionPtr)(int, int);
  }myy_structone;
 typedef struct my_struct 
 {
	float f_float;
	//结构体包含结构体
	myy_structone *structone;

	//结构体中包含系统头文件的类型
	FILE file;
	struct my_struct *next;
 }myy_struct;

 //结构体作为函数参数
 void StructTypeTest1(myy_struct m_struct);
 
json::
  {
   "StructTypeTest10" : {
      "m_struct" : {
         "f_float" : -7.571428775787354,
         "file" : "NULL",
         "next" : "NULL",
         "structone" : {
            "array_one" : [ 9806, 26173 ],
            "array_two" : [
               [ 26844, 25577, 19319, 9827 ],
               [ 19915, 10301, 31575, 7526 ],
               [ 23827, 1201, 4435, 31404 ]
            ],
            "functionPtr" : "NULL",
            "i_int" : 14820,
            "point_one" : [ 978, 32767, 20233 ],
            "point_two" : [
               [ 14315, 22841, 1955 ],
               [ 4821, 1433, 10546 ],
               [ 24557, 22426, 23460 ]
            ],
            "w" : 4677
         }
      }
   },
   
数据表格

