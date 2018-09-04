.. _GlobalType

全局变量
========
全局变量赋值方式跟作为函数参数的赋值方式是一样的，作为全局变量的时候，会先在驱动文件开头声明一下变量，然后在对其进行赋值。

原函数::

  extern int g_int ;
  extern double g_double ;

  void globalType()
 {
	g_int = 12;
	g_double = 15;
 }
 
PSD函数结构描述::

  <globalType parmNum="0">
    <g_double globalType="globalVar" />
    <g_int globalType="globalVar" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </globalType>
  
PSD全局变量结构描述::
 
  <GlobalVar>
    <g_int baseType1="BuiltinType" type="ZOA_INT" />
  </GlobalVar>
  <GlobalVar>
    <g_double baseType1="BuiltinType" type="ZOA_DOUBLE" />
  </GlobalVar>

驱动代码::

  //全局变量声明
  extern "C" {
    extern int g_int;
    extern double g_double;
  }
  
 /* 函数原型: 
  *
  * void globalType();
  */
 int Drive_globalType(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,globalType_Root是函数globalType的json对象 */
    Json::Value globalType_Root = Root[MyStrCat("globalType",times)];

    /* 这是第1个全局变量: g_double    globalType
     *
     * 全局变量原型: double g_double     
     */


    /* g_double */
    double _g_double = globalType_Root["g_double"].asDouble();
    g_double = _g_double;

    /* 这是第2个全局变量: g_int    globalType
     *
     * 全局变量原型: int g_int     
     */


    /* g_int */
    int _g_int = globalType_Root["g_int"].asInt();
    g_int = _g_int;

    /* return & function Call */
    /* return void  */
     globalType();

    /* return print */
    cout<<"globalType :"<<endl; 
    return 0;
 }