.. _StructArrayType

结构体数组类型
==============

一维数组
--------
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
  void StructType5(struct my_struct a[2]);

  //函数指针赋值函数
  int fun1(int b, int c);
PSD结构描述::

  <StructType5 parmType0="struct my_struct [2]" parmNum="1">
    <a baseType1="ArrayType" RowSize="2" type="StructureOrClassType" name="my_struct">
        <i_int baseType1="BuiltinType" type="ZOA_INT" />
        <array_one baseType1="ArrayType" RowSize="2" type="ZOA_INT" />
        <point_one baseType1="PointerType" type="ZOA_INT" />
        <point baseType1="PointerType" type="ZOA_VOID" />
        <w baseType1="BuiltinType" type="ZOA_UINT" bitfield="1" />
        <functionPtr baseType1="FunctionPointType" type="ZOA_FUNC" returnType="int" parmType0="int" parmType1="int" parmNum="2" />
    </a>
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </StructType5>
  
驱动代码::

  int Drive_StructType5(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,StructType5_Root是函数StructType5的json对象 */
    Json::Value StructType5_Root = Root[MyStrCat("StructType5",times)];

    /* 这是第1个参数:a    StructType5
     *
     * 参数原型: struct my_struct a[2]     
     */

    /* _a_Root 是a对应的json串 */
    Json::Value _a_Root = StructType5_Root["a"];

    /* a */
    struct my_struct _a[2];
    {
        for(int a_row = 0; a_row < 2; a_row++)
        {
            Json::Value _a_arr_Root = _a_Root[a_row];
            struct my_struct _a_tmp;

            /* i_int */
            int _i_int = _a_arr_Root["i_int"].asInt();
            _a_tmp.i_int = _i_int;

            /* array_one */
            int _array_one[2];
            for(int len = 0; len < 2; len++)
            {
                _array_one[len] = _a_arr_Root["array_one"][len].asInt();
            }
            memcpy(_a_tmp.array_one,_array_one,sizeof(_array_one));

            /* point_one */
            {
                int W_index = _a_arr_Root["point_one"].size();
                _a_tmp.point_one = new int[W_index];
                for(int i = 0; i < W_index; i++)
                {
                    _a_tmp.point_one[i] = _a_arr_Root["point_one"][i].asInt();
                }
            }

            /* point's type: void */
            _a_tmp.point = NULL;

            /* w */
            unsigned int _w = (unsigned int)_a_arr_Root["w"].asInt();
            _a_tmp.w = _w;

            /* functionPtr's type is function pointer */
            _a_tmp.functionPtr = fun1;
            _a[a_row] = _a_tmp;
        }
    }

    /* return & function Call */
    /* return void  */
     StructType5(_a);

    /* return print */
    cout<<"StructType5 :"<<endl; 
    return 0;
  }
二维数组
--------
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
  void StructType6(struct my_struct a[2][3]);

  //函数指针赋值函数
  int fun1(int b, int c);
PSD结构描述::

  <StructType6 parmType0="struct my_struct [2][3]" parmNum="1">
    <a baseType1="ArrayType" RowSize="2" baseType2="ArrayType" ColumnSize="3" type="StructureOrClassType" name="my_struct">
        <i_int baseType1="BuiltinType" type="ZOA_INT" />
        <array_one baseType1="ArrayType" RowSize="2" type="ZOA_INT" />
        <point_one baseType1="PointerType" type="ZOA_INT" />
        <point baseType1="PointerType" type="ZOA_VOID" />
        <w baseType1="BuiltinType" type="ZOA_UINT" bitfield="1" />
        <functionPtr baseType1="FunctionPointType" type="ZOA_FUNC" returnType="int" parmType0="int" parmType1="int" parmNum="2" />
    </a>
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </StructType6>
  
驱动代码::

  int Drive_StructType6(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,StructType6_Root是函数StructType6的json对象 */
    Json::Value StructType6_Root = Root[MyStrCat("StructType6",times)];

    /* 这是第1个参数:a    StructType6
     *
     * 参数原型: struct my_struct a[2][3]     
     */

    /* _a_Root 是a对应的json串 */
    Json::Value _a_Root = StructType6_Root["a"];

    /* a */
    struct my_struct _a[2][3];
    for(int a_Row = 0; a_Row < 2; a_Row++)
    {
        for(int a_Column = 0; a_Column < 3; a_Column++)
        {
            Json::Value _a_arr_Root = _a_Root[a_Row][a_Column];
            struct my_struct _a_tmp;

            /* i_int */
            int _i_int = _a_arr_Root["i_int"].asInt();
            _a_tmp.i_int = _i_int;

            /* array_one */
            int _array_one[2];
            for(int len = 0; len < 2; len++)
            {
                _array_one[len] = _a_arr_Root["array_one"][len].asInt();
            }
            memcpy(_a_tmp.array_one,_array_one,sizeof(_array_one));

            /* point_one */
            {
                int W_index = _a_arr_Root["point_one"].size();
                _a_tmp.point_one = new int[W_index];
                for(int i = 0; i < W_index; i++)
                {
                    _a_tmp.point_one[i] = _a_arr_Root["point_one"][i].asInt();
                }
            }

            /* point's type: void */
            _a_tmp.point = NULL;

            /* w */
            unsigned int _w = (unsigned int)_a_arr_Root["w"].asInt();
            _a_tmp.w = _w;

            /* functionPtr's type is function pointer */
            _a_tmp.functionPtr = fun1;
            _a[a_Row][a_Column] = _a_tmp;
        }
    }

    /* return & function Call */
    /* return void  */
     StructType6(_a);

    /* return print */
    cout<<"StructType6 :"<<endl; 
    return 0;
  }
三维数组
--------
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
 void StructType7(struct my_struct a[2][3][4]);

 //函数指针赋值函数
 int fun1(int b, int c);
 
PSD结构描述::
  
  <StructType7 parmType0="struct my_struct [2][3][4]" parmNum="1">
    <a baseType1="ArrayType" RowSize="2" baseType2="ArrayType" ColumnSize="3" baseType3="ArrayType" HighSize="4" type="StructureOrClassType" name="my_struct">
        <i_int baseType1="BuiltinType" type="ZOA_INT" />
        <array_one baseType1="ArrayType" RowSize="2" type="ZOA_INT" />
        <point_one baseType1="PointerType" type="ZOA_INT" />
        <point baseType1="PointerType" type="ZOA_VOID" />
        <w baseType1="BuiltinType" type="ZOA_UINT" bitfield="1" />
        <functionPtr baseType1="FunctionPointType" type="ZOA_FUNC" returnType="int" parmType0="int" parmType1="int" parmNum="2" />
    </a>
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </StructType7>
驱动代码::
  
  int Drive_StructType7(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,StructType7_Root是函数StructType7的json对象 */
    Json::Value StructType7_Root = Root[MyStrCat("StructType7",times)];
    /* 这是第1个参数:a    StructType7
     *
     * 参数原型: struct my_struct a[2][3][4]     
     */
    /* _a_Root 是a对应的json串 */
    Json::Value _a_Root = StructType7_Root["a"];

    /* a */
    struct my_struct _a[2][3][4];
    for(int a_Row = 0; a_Row < 2; a_Row++)
    {
        for(int a_Column = 0; a_Column < 3; a_Column++)
        {
            for(int a_High = 0; a_High < 4; a_High++)
            {
                Json::Value _a_arr_Root = _a_Root[a_Row][a_Column][a_High];
                struct my_struct _a_tmp;

                /* i_int */
                int _i_int = _a_arr_Root["i_int"].asInt();
                _a_tmp.i_int = _i_int;

                /* array_one */
                int _array_one[2];
                for(int len = 0; len < 2; len++)
                {
                    _array_one[len] = _a_arr_Root["array_one"][len].asInt();
                }
                memcpy(_a_tmp.array_one,_array_one,sizeof(_array_one));

                /* point_one */
                {
                    int W_index = _a_arr_Root["point_one"].size();
                    _a_tmp.point_one = new int[W_index];
                    for(int i = 0; i < W_index; i++)
                    {
                        _a_tmp.point_one[i] = _a_arr_Root["point_one"][i].asInt();
                    }
                }

                /* point's type: void */
                _a_tmp.point = NULL;

                /* w */
                unsigned int _w = (unsigned int)_a_arr_Root["w"].asInt();
                _a_tmp.w = _w;

                /* functionPtr's type is function pointer */
                _a_tmp.functionPtr = fun1;
                _a[a_Row][a_Column][a_High] = _a_tmp;
            }
        }
    }
    /* return & function Call */
    /* return void  */
     StructType7(_a);
    /* return print */
    cout<<"StructType7 :"<<endl; 
    return 0;
 }