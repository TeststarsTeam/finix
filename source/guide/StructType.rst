.. _StructType:

StructType
==========

.. toctree::
   :maxdepth: 3
   
普通结构体类型
--------------
原函数::

  struct my_struct
  {
	//基本类型
	int i_int;

	//数组类型
	int array_one[2];

	//指针类型
	int *point_one;

	//空类型
	void *point;

	//位域类型
	unsigned int w : 1;

	//函数指针是指向函数的指针变量，即本质是一个指针变量
	int(*functionPtr)(int, int);

 };

  //被测函数
  void StructType1(struct my_struct a);

  //函数指针赋值函数
  int fun1(int b, int c);
  
  
PSD结构描述::

  <StructType1 parmType0="struct my_struct" parmNum="1">
    <a baseType1="StructureOrClassType" type="StructureOrClassType" name="my_struct">
        <i_int baseType1="BuiltinType" type="ZOA_INT" />
        <array_one baseType1="ArrayType" RowSize="2" type="ZOA_INT" />
        <point_one baseType1="PointerType" type="ZOA_INT" />
        <point baseType1="PointerType" type="ZOA_VOID" />
        <w baseType1="BuiltinType" type="ZOA_UINT" bitfield="1" />
        <functionPtr baseType1="FunctionPointType" type="ZOA_FUNC" returnType="int" parmType0="int" parmType1="int" parmNum="2" />
    </a>
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </StructType1>
  
驱动代码::

  int Drive_StructType1(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,StructType1_Root是函数StructType1的json对象 */
    Json::Value StructType1_Root = Root[MyStrCat("StructType1",times)];

    /* 这是第1个参数:a    StructType1
     *
     * 参数原型: struct my_struct a     
     */

    /* _a_Root 是a对应的json串 */
    Json::Value _a_Root = StructType1_Root["a"];

    /* a */
    struct my_struct _a;
    {

        /* i_int */
        int _i_int = _a_Root["i_int"].asInt();
        _a.i_int = _i_int;

        /* array_one */
        int _array_one[2];
        for(int len = 0; len < 2; len++)
        {
            _array_one[len] = _a_Root["array_one"][len].asInt();
        }
        memcpy(_a.array_one,_array_one,sizeof(_array_one));

        /* point_one */
        {
            int W_index = _a_Root["point_one"].size();
            _a.point_one = new int[W_index];
            for(int i = 0; i < W_index; i++)
            {
                _a.point_one[i] = _a_Root["point_one"][i].asInt();
            }
        }

        /* point's type: void */
        _a.point = NULL;

        /* w */
        unsigned int _w = (unsigned int)_a_Root["w"].asInt();
        _a.w = _w;

        /* functionPtr's type is function pointer */
        _a.functionPtr = fun1;
    }

    /* return & function Call */
    /* return void  */
     StructType1(_a);

    /* return print */
    cout<<"StructType1 :"<<endl; 
    return 0;
 }
 
 
 guide/StructPointType.rst