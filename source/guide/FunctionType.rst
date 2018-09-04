.. _FunctionType

函数指针类型
============
函数指针的赋值方式，我们首先分析出函数指针的结构，包括返回值和各个参数类型，然后对本文件中的函数进行匹配，如果匹配到相同类型的函数，那就赋值给函数指针，如果相同类型的函数比较多，那就随机赋值，如果没有匹配到相同类型的函数，那就先赋值NULL，用户后期自行对其进行赋值。

原函数::

  typedef int(*FUNCPOINTER)(int, double);

  void FunctionPointerType(FUNCPOINTER fp);

  int fun2(int a, double d);
  
PSD结构描述::

   <FunctionPointerType parmType0="FUNCPOINTER" parmNum="1">
    <fp baseType1="FunctionPointType" type="ZOA_FUNC" returnType="int" parmType0="int" parmType1="double" parmNum="2" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
   </FunctionPointerType>
   
驱动代码::

  int Drive_FunctionPointerType(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,FunctionPointerType_Root是函数FunctionPointerType的json对象 */
    Json::Value FunctionPointerType_Root = Root[MyStrCat("FunctionPointerType",times)];

    /* 这是第1个参数:fp    FunctionPointerType
     *
     * 参数原型: int (*fp)(int, double);     
     */


    /* return & function Call */
    /* return void  */
     FunctionPointerType(&fun2);

    /* return print */
    cout<<"FunctionPointerType :"<<endl; 
    return 0;
 }