.. _UnionType

联合体类型
==========
Union
-----
联合体类型我们默认的是全部赋值，可以根据实际情况，将用户自己需要的赋值内容保留
 
原函数::

  union my_union {
	int a;
	double b;
	long c;
  };
  struct my_struct_union {

	//联合体类型
	union my_union d;
  };
  void struct_union_type(struct my_struct_union su);
  
PSD结构描述::

  <struct_union_type parmType0="struct my_struct_union" parmNum="1">
    <su baseType1="StructureOrClassType" type="StructureOrClassType" name="my_struct_union">
        <d baseType1="UnionType" type="ZOA_UNION" name="my_union">
            <a baseType1="BuiltinType" type="ZOA_INT" />
            <b baseType1="BuiltinType" type="ZOA_DOUBLE" />
            <c baseType1="BuiltinType" type="ZOA_LONG" />
        </d>
    </su>
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </struct_union_type>
  
驱动代码::

  int Drive_struct_union_type(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,struct_union_type_Root是函数struct_union_type的json对象 */
    Json::Value struct_union_type_Root = Root[MyStrCat("struct_union_type",times)];

    /* 这是第1个参数:su    struct_union_type
     *
     * 参数原型: struct my_struct_union su     
     */

    /* _su_Root 是su对应的json串 */
    Json::Value _su_Root = struct_union_type_Root["su"];

    /* su */
    struct my_struct_union _su;
    {
        /* 结构体 d */
        /* _d_Root 是d对应的json串 */
        Json::Value _d_Root = _su_Root["d"];

        /* d */
        union my_union _d;
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
        _su.d = _d;
    }

    /* return & function Call */
    /* return void  */
     struct_union_type(_su);

    /* return print */
    cout<<"struct_union_type :"<<endl; 
    return 0;
 }