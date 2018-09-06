finix overall description
========================
C type description
------------------
.. image:: /image/type.png

The overall process of finix
-----------------------------
.. image:: /image/principle.png

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
 
PSD file::
  
  <StructTypeTest1 parmType0="myy_struct" parmNum="1">
    <m_struct baseType1="StructureOrClassType" type="StructureOrClassType" name="my_struct">
        <f_float baseType1="BuiltinType" type="ZOA_FLOAT" />
        <structone baseType1="PointerType" type="StructureOrClassType" name="my_structone">
            <i_int baseType1="BuiltinType" type="ZOA_INT" />
            <array_one baseType1="ArrayType" RowSize="2" type="ZOA_INT" />
            <array_two baseType1="ArrayType" RowSize="3" baseType2="ArrayType" ColumnSize="4" type="ZOA_INT" />
            <point_one baseType1="PointerType" type="ZOA_INT" />
            <point_two baseType1="PointerType" baseType2="PointerType" type="ZOA_INT" />
            <point baseType1="PointerType" type="ZOA_VOID" />
            <w baseType1="BuiltinType" type="ZOA_UINT" bitfield="1" />
            <functionPtr baseType1="FunctionPointType" type="ZOA_FUNC" returnType="int" parmType0="int" parmType1="int" parmNum="2" />
        </structone>
        <file baseType1="StructureOrClassType" type="StructureOrClassType" name="_iobuf" SystemVar="_iobuf" />
        <next NodeType="LinkNode" baseType1="PointerType" type="StructureOrClassType" name="my_struct" />
    </m_struct>
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </StructTypeTest1

PSD file attribute description
------------------------------

+------------------------+------------------------+
| 基本类型               | PSD文件对应类型        |               
+========================+========================+
| char /unsigned char    |ZOA_CHAR_S/ZOA_UCHAR    | 
+------------------------+------------------------+
| int /unsigned int      |ZOA_INT/ZOA_UINT        | 
+------------------------+------------------------+
| long/unsigned long     |ZOA_LONG/ZOA_ULONG      | 
+------------------------+------------------------+
| float/unsigned float   |ZOA_FLOAT/ZOA_UFLOAT    | 
+------------------------+------------------------+
| short/unsigned short   |ZOA_SHOTR/ZOA_USHORT    | 
+------------------------+------------------------+
| double/unsigned double |ZOA_DOUBLE/ZOA_UDOUBLE  | 
+------------------------+------------------------+


+------------------------+------------------------+
| BuiltinType            |基本类型                |               
+------------------------+------------------------+
| ArrayType              |数组类型                | 
+------------------------+------------------------+
| PointerType            |指针类型                | 
+------------------------+------------------------+
| StructureOrClassType   |结构体类型              | 
+------------------------+------------------------+
| UnionType              |联合体类型              | 
+------------------------+------------------------+
| EnumType               |枚举类型                | 
+------------------------+------------------------+
| FunctionPointType      |函数指针类型            | 
+------------------------+------------------------+


+-------------------------------------------------+------------------------+
|ZOA_CHAR_S/ZOA_UCHAR/ZOA_INT/ZOA_UINT/ZOA_LONG   |                        |
|ZOA_ULONG/ZOA_FLOAT/ZOA_UFLOAT/ZOA_SHOTR         |基本类型                |
|ZOA_USHORT/ZOA_DOUBLE/ZOA_UDOUBLE                |                        |      
+-------------------------------------------------+------------------------+
| StructureOrClassType                            |结构体类型              | 
+------------------------+------------------------+------------------------+
| ZOA_UNION                                       |联合体类型              | 
+------------------------+------------------------+------------------------+
| ZOA_ENUM                                        |枚举类型                | 
+------------------------+------------------------+------------------------+
| ZOA_FUNC                                        |函数指针类型            | 
+------------------------+------------------------+------------------------+


+------------------------+------------------------+
|Name                    |代表结构体名称          |               
+------------------------+------------------------+
|NodeType                |代表链表类型            | 
+------------------------+------------------------+
|parmType                |代表函数参数类型        | 
+------------------------+------------------------+
|parNum                  |代表函数参数个数        | 
+------------------------+------------------------+
|SystemVar               |代表系统头文件类型      | 
+------------------------+------------------------+
|value                   |代表枚举类型的值        | 
+------------------------+------------------------+
|bitfield                |代表位域类型所占字节    | 
+------------------------+------------------------+
|returnType              |代表返回值类型          | 
+------------------------+------------------------+

Finix test data description
-----------------
finix通过编译技术提取函数参数信息与对应的全局变量信息，利用这些信息生成对应的测试用例，底层保存为json格式的形式
finix为了更好的展示测试数据，采用数据表格可以表达任意深度和多层次的数据关系，用户只需要对表格数据进行编辑，
自动生成的驱动程序会自动完成表格数据的读取和参数赋值的构造过程。

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
   
Data table

.. image:: /image/data.png