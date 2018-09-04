.. _NestStructType

嵌套结构体类型
==============
结构体嵌套结构体类型，内部的结构体赋值方式跟外部方法一样，内部全部赋完值之后在赋值给上一层结构体，再往内部方法也是一样的。

原函数::

  struct my_struct_in {
	int a;
	double b;
	long c;
  };

 struct my_struct_out {
	//结构体类型
	struct my_struct_in d;
 };

 void struct_struct_type(struct my_struct_out ss); 
 
PSD结构描述::

  <struct_struct_type parmType0="struct my_struct_out" parmNum="1">
    <ss baseType1="StructureOrClassType" type="StructureOrClassType" name="my_struct_out">
        <d baseType1="StructureOrClassType" type="StructureOrClassType" name="my_struct_in">
            <a baseType1="BuiltinType" type="ZOA_INT" />
            <b baseType1="BuiltinType" type="ZOA_DOUBLE" />
            <c baseType1="BuiltinType" type="ZOA_LONG" />
        </d>
    </ss>
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </struct_struct_type>
  
驱动代码::

  int Drive_struct_struct_type(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,struct_struct_type_Root是函数struct_struct_type的json对象 */
    Json::Value struct_struct_type_Root = Root[MyStrCat("struct_struct_type",times)];

    /* 这是第1个参数:ss    struct_struct_type
     *
     * 参数原型: struct my_struct_out ss     
     */

    /* _ss_Root 是ss对应的json串 */
    Json::Value _ss_Root = struct_struct_type_Root["ss"];

    /* ss */
    struct my_struct_out _ss;
    {
        /* 结构体 d */
        /* _d_Root 是d对应的json串 */
        Json::Value _d_Root = _ss_Root["d"];

        /* d */
        struct my_struct_in _d;
        {

            /* a */
            int _a = _d_Root["a"].asInt();
            _d.a = _a;

            /* b */
            double _b = _d_Root["b"].asDouble();
            _d.b = _b;

            /* c */
            long _c = (long)_d_Root["c"].asInt();
            _d.c = _c;
        }
        _ss.d = _d;
    }

    /* return & function Call */
    /* return void  */
     struct_struct_type(_ss);

    /* return print */
    cout<<"struct_struct_type :"<<endl; 
    return 0;
 }