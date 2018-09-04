.. _EnumType

枚举类型
========
Enum
-----
原函数::

  enum my_enum {
	enum1,
	enum2 = 22,
	enum3
 };
 struct my_struct_enum {

	//枚举类型
	enum my_enum a;
 };
 void Struct_enum_type(struct my_struct_enum se);
 
PSD结构描述::

 <Struct_enum_type parmType0="struct my_struct_enum" parmNum="1">
    <se baseType1="StructureOrClassType" type="StructureOrClassType" name="my_struct_enum">
        <a baseType1="EnumType" type="ZOA_ENUM" name="my_enum">
            <enum1 type="ZOA_INT" value="0" />
            <enum2 type="ZOA_INT" value="22" />
            <enum3 type="ZOA_INT" value="23" />
        </a>
    </se>
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </Struct_enum_type>
 
驱动代码::

  int Drive_Struct_enum_type(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,Struct_enum_type_Root是函数Struct_enum_type的json对象 */
    Json::Value Struct_enum_type_Root = Root[MyStrCat("Struct_enum_type",times)];

    /* 这是第1个参数:se    Struct_enum_type
     *
     * 参数原型: struct my_struct_enum se     
     */

    /* _se_Root 是se对应的json串 */
    Json::Value _se_Root = Struct_enum_type_Root["se"];

    /* se */
    struct my_struct_enum _se;
    {

        /* a ENUM :enum1 enum2 enum3 */
        _se.a = enum1;
    }

    /* return & function Call */
    /* return void  */
     Struct_enum_type(_se);

    /* return print */
    cout<<"Struct_enum_type :"<<endl; 
    return 0;
 }