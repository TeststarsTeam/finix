.. _BuiltinType:

BuiltinType
===========

char类型
--------

原函数::

  void BuiltinTypeChar(char c_char);

PSD结构描述::

  <BuiltinTypeChar parmType0="char" ParNum="1">
    <c_char baseType1="BuiltinType" type="ZOA_CHAR_S" />
    <returnType returnType="void" />
  </BuiltinTypeChar>
  
驱动代码::
  
  /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
   * 函数原型:
   * void BuiltinTypeChar(char c_char)
  */
  int Drive_BuiltinTypeChar(Json::Value Root, int times)
  {
    /* Root是值文件的json对象,BuiltinTypeChar_Root是函数BuiltinTypeChar的json对象 */
    Json::Value BuiltinTypeChar_Root = Root[MyStrCat("BuiltinTypeChar",times)];

    /* 这是第1个参数: c_char    BuiltinTypeChar
     *
     * 参数原型: char c_char     
     */


    /* c_char */
    string _c_charStr = BuiltinTypeChar_Root["c_char"].asString();
    char _c_char = _c_charStr[0];

    BuiltinTypeChar(_c_char);
    return 0;
  }
  
int类型
-------
原函数::

  void BuiltinTypeInt(int i_int);
  
PSD结构描述::

  <BuiltinTypeInt parmType0="int" parmNum="1">
        <i_int baseType1="BuiltinType" type="ZOA_INT" />
        <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </BuiltinTypeInt>
  
驱动代码::

  /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
   * 函数原型:
   * void BuiltinTypeInt(int i_int)
   */
  int Drive_BuiltinTypeInt(Json::Value Root, int times)
  {
        /*Root是值文件的json对象,BuiltinTypeInt_Root是函数BuiltinTypeInt的json对象 */
        Json::Value BuiltinTypeInt_Root =Root[MyStrCat("BuiltinTypeInt",times)];

        /* 这是第1个参数:i_int    BuiltinTypeInt
         *
         * 参数原型: int i_int     
         */


        /* i_int */
        int _i_int = BuiltinTypeInt_Root["i_int"].asInt();

        /* return & function Call */
        /* return void  */
         BuiltinTypeInt(_i_int);

        /* return print */
        cout<<"BuiltinTypeInt :"<<endl; 
        return 0;
  }
  
long类型
--------
原函数::

 void BuiltinTypeLong(long l_long);
 
PSD结构描述::

 <BuiltinTypeLong parmType0="long" parmNum="1">
    <l_long baseType1="BuiltinType" type="ZOA_LONG" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </BuiltinTypeLong>
 
驱动代码::

  /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
   * 函数原型:
   * void BuiltinTypeLong(long l_long)
   */
  int Drive_BuiltinTypeLong(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,BuiltinTypeLong_Root是函数BuiltinTypeLong的json对象 */
    Json::Value BuiltinTypeLong_Root = Root[MyStrCat("BuiltinTypeLong",times)];

    /* 这是第1个参数:l_long    BuiltinTypeLong
     *
     * 参数原型: long l_long     
     */


    /* l_long */
    long _l_long = (long)BuiltinTypeLong_Root["l_long"].asInt();

    /* return & function Call */
    /* return void  */
     BuiltinTypeLong(_l_long);

    /* return print */
    cout<<"BuiltinTypeLong :"<<endl; 
    return 0;
  }
  
short类型
---------
原函数::

  void BuiltinTypeShort(short s_short);
  
PSD结构描述::

  <BuiltinTypeShort parmType0="short" parmNum="1">
    <s_short baseType1="BuiltinType" type="ZOA_SHORT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </BuiltinTypeShort>
  
驱动代码::

 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void BuiltinTypeShort(short s_short)
 */
 int Drive_BuiltinTypeShort(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,BuiltinTypeShort_Root是函数BuiltinTypeShort的json对象 */
    Json::Value BuiltinTypeShort_Root = Root[MyStrCat("BuiltinTypeShort",times)];

    /* 这是第1个参数:s_short    BuiltinTypeShort
     *
     * 参数原型: short s_short     
     */


    /* s_short */
    short _s_short = (short)BuiltinTypeShort_Root["s_short"].asInt();

    /* return & function Call */
    /* return void  */
     BuiltinTypeShort(_s_short);

    /* return print */
    cout<<"BuiltinTypeShort :"<<endl; 
    return 0;
 }
 
unsigned int类型
----------------
原函数::

 void BuiltinTypeUInt(unsigned int i_Uint);
 
PSD结构描述::

 <BuiltinTypeUInt parmType0="unsigned int" parmNum="1">
    <i_Uint baseType1="BuiltinType" type="ZOA_UINT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </BuiltinTypeUInt>
 
驱动代码::


 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void BuiltinTypeUInt(unsigned int i_Uint)
 */
 int Drive_BuiltinTypeUInt(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,BuiltinTypeUInt_Root是函数BuiltinTypeUInt的json对象 */
    Json::Value BuiltinTypeUInt_Root = Root[MyStrCat("BuiltinTypeUInt",times)];

    /* 这是第1个参数:i_Uint    BuiltinTypeUInt
     *
     * 参数原型: unsigned int i_Uint     
     */


    /* i_Uint */
    unsigned int _i_Uint = (unsigned int)BuiltinTypeUInt_Root["i_Uint"].asInt();

    /* return & function Call */
    /* return void  */
     BuiltinTypeUInt(_i_Uint);

    /* return print */
    cout<<"BuiltinTypeUInt :"<<endl; 
    return 0;
 }
unsigned char类型
----------------
原函数::

  void BuiltinTypeUChar(unsigned char c_Uchar);
  
PSD结构描述::

  <BuiltinTypeUChar parmType0="unsigned char" parmNum="1">
    <c_Uchar baseType1="BuiltinType" type="ZOA_UCHAR" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </BuiltinTypeUChar>
  
驱动代码::

 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void BuiltinTypeUChar(unsigned char c_Uchar)
 */
 int Drive_BuiltinTypeUChar(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,BuiltinTypeUChar_Root是函数BuiltinTypeUChar的json对象 */
    Json::Value BuiltinTypeUChar_Root = Root[MyStrCat("BuiltinTypeUChar",times)];

    /* 这是第1个参数:c_Uchar    BuiltinTypeUChar
     *
     * 参数原型: unsigned char c_Uchar     
     */


    /* c_Uchar */
    string _c_UcharStr = BuiltinTypeUChar_Root["c_Uchar"].asString();
    unsigned char _c_Uchar = (unsigned char)_c_UcharStr[0];

    /* return & function Call */
    /* return void  */
     BuiltinTypeUChar(_c_Uchar);

    /* return print */
    cout<<"BuiltinTypeUChar :"<<endl; 
    return 0;
 }
unsigned short类型
----------------
原函数::

 void BuiltinTypeUShort(unsigned short s_Ushort);

PSD结构描述::

 <BuiltinTypeUShort parmType0="unsigned short" parmNum="1">
    <s_Ushort baseType1="BuiltinType" type="ZOA_USHORT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </BuiltinTypeUShort>
 
驱动代码::

 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void BuiltinTypeUShort(unsigned short s_Ushort)
 */
 int Drive_BuiltinTypeUShort(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,BuiltinTypeUShort_Root是函数BuiltinTypeUShort的json对象 */
    Json::Value BuiltinTypeUShort_Root = Root[MyStrCat("BuiltinTypeUShort",times)];

    /* 这是第1个参数:s_Ushort    BuiltinTypeUShort
     *
     * 参数原型: unsigned short s_Ushort     
     */


    /* s_Ushort */
    unsigned short _s_Ushort = (unsigned short)BuiltinTypeUShort_Root["s_Ushort"].asInt();

    /* return & function Call */
    /* return void  */
     BuiltinTypeUShort(_s_Ushort);

    /* return print */
    cout<<"BuiltinTypeUShort :"<<endl; 
    return 0;
 }
 
unsigned long类型
----------------
原函数::

  void BuiltinTypeULong(unsigned long l_Ulong);
  
PSD结构描述::
  
  <BuiltinTypeULong parmType0="unsigned long" parmNum="1">
    <l_Ulong baseType1="BuiltinType" type="ZOA_ULONG" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </BuiltinTypeULong>
  
驱动代码::

 
 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void BuiltinTypeULong(unsigned long l_Ulong)
 */
 int Drive_BuiltinTypeULong(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,BuiltinTypeULong_Root是函数BuiltinTypeULong的json对象 */
    Json::Value BuiltinTypeULong_Root = Root[MyStrCat("BuiltinTypeULong",times)];

    /* 这是第1个参数:l_Ulong    BuiltinTypeULong
     *
     * 参数原型: unsigned long l_Ulong     
     */


    /* l_Ulong */
    unsigned long _l_Ulong = (unsigned long)BuiltinTypeULong_Root["l_Ulong"].asInt();

    /* return & function Call */
    /* return void  */
     BuiltinTypeULong(_l_Ulong);

    /* return print */
    cout<<"BuiltinTypeULong :"<<endl; 
    return 0;
 }
 
long long类型
----------------
原函数::

 void BuiltinTypeLongLong(long long ll_long);

PSD结构描述::

 <BuiltinTypeLongLong parmType0="long long" parmNum="1">
    <ll_long baseType1="BuiltinType" type="ZOA_LONGLONG" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </BuiltinTypeLongLong>
 
驱动代码::


 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void BuiltinTypeLongLong(long long ll_long)
 */
 int Drive_BuiltinTypeLongLong(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,BuiltinTypeLongLong_Root是函数BuiltinTypeLongLong的json对象 */
    Json::Value BuiltinTypeLongLong_Root = Root[MyStrCat("BuiltinTypeLongLong",times)];

    /* 这是第1个参数:ll_long    BuiltinTypeLongLong
     *
     * 参数原型: long long ll_long     
     */


    /* ll_long */
    long long _ll_long = (long long)BuiltinTypeLongLong_Root["ll_long"].asInt();

    /* return & function Call */
    /* return void  */
     BuiltinTypeLongLong(_ll_long);

    /* return print */
    cout<<"BuiltinTypeLongLong :"<<endl; 
    return 0;
 }
 
double 类型
----------------
原函数::

 void BuiltinTypeDouble(double d_double);
 
PSD结构描述::

 <BuiltinTypeDouble parmType0="double" parmNum="1">
    <d_double baseType1="BuiltinType" type="ZOA_DOUBLE" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </BuiltinTypeDouble>
 
驱动代码::


 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void BuiltinTypeDouble(double d_double)
 */
 int Drive_BuiltinTypeDouble(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,BuiltinTypeDouble_Root是函数BuiltinTypeDouble的json对象 */
    Json::Value BuiltinTypeDouble_Root = Root[MyStrCat("BuiltinTypeDouble",times)];

    /* 这是第1个参数:d_double    BuiltinTypeDouble
     *
     * 参数原型: double d_double     
     */


    /* d_double */
    double _d_double = BuiltinTypeDouble_Root["d_double"].asDouble();

    /* return & function Call */
    /* return void  */
     BuiltinTypeDouble(_d_double);

    /* return print */
    cout<<"BuiltinTypeDouble :"<<endl; 
    return 0;
 }
 
float 类型
----------------
原函数::

  void BuiltinTypeFloat(float f_float);
  
PSD结构描述::

 <BuiltinTypeFloat parmType0="float" parmNum="1">
    <f_float baseType1="BuiltinType" type="ZOA_FLOAT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </BuiltinTypeFloat>
 
驱动代码::


 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void BuiltinTypeFloat(float f_float)
 */
 int Drive_BuiltinTypeFloat(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,BuiltinTypeFloat_Root是函数BuiltinTypeFloat的json对象 */
    Json::Value BuiltinTypeFloat_Root = Root[MyStrCat("BuiltinTypeFloat",times)];

    /* 这是第1个参数:f_float    BuiltinTypeFloat
     *
     * 参数原型: float f_float     
     */


    /* f_float */
    float _f_float = BuiltinTypeFloat_Root["f_float"].asDouble();

    /* return & function Call */
    /* return void  */
     BuiltinTypeFloat(_f_float);

    /* return print */
    cout<<"BuiltinTypeFloat :"<<endl; 
    return 0;
 }


long double类型
----------------
原函数::

  void BuiltinTypeLongDouble(long double ld_double);
  
PSD结构描述::

  <BuiltinTypeLongDouble parmType0="long double" parmNum="1">
    <ld_double baseType1="BuiltinType" type="ZOA_LONGDOUBLE" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </BuiltinTypeLongDouble>
 
驱动代码::
  
 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void BuiltinTypeLongDouble(long double ld_double)
 */
 int Drive_BuiltinTypeLongDouble(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,BuiltinTypeLongDouble_Root是函数BuiltinTypeLongDouble的json对象 */
    Json::Value BuiltinTypeLongDouble_Root = Root[MyStrCat("BuiltinTypeLongDouble",times)];

    /* 这是第1个参数:ld_double    BuiltinTypeLongDouble
     *
     * 参数原型: long double ld_double     
     */


    /* ld_double */
    long double _ld_double = (long double)BuiltinTypeLongDouble_Root["ld_double"].asDouble();

    /* return & function Call */
    /* return void  */
     BuiltinTypeLongDouble(_ld_double);

    /* return print */
    cout<<"BuiltinTypeLongDouble :"<<endl; 
    return 0;
 }
 
bool 类型
----------------
原函数::

  void BuiltinTypeBool(bool b_bool);
  
PSD结构描述::
  
  <BuiltinTypeLongDouble parmType0="long double" parmNum="1">
    <ld_double baseType1="BuiltinType" type="ZOA_LONGDOUBLE" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </BuiltinTypeLongDouble>
  
驱动代码::

  
 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void BuiltinTypeLongDouble(long double ld_double)
 */
 int Drive_BuiltinTypeLongDouble(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,BuiltinTypeLongDouble_Root是函数BuiltinTypeLongDouble的json对象 */
    Json::Value BuiltinTypeLongDouble_Root = Root[MyStrCat("BuiltinTypeLongDouble",times)];

    /* 这是第1个参数:ld_double    BuiltinTypeLongDouble
     *
     * 参数原型: long double ld_double     
     */


    /* ld_double */
    long double _ld_double = (long double)BuiltinTypeLongDouble_Root["ld_double"].asDouble();

    /* return & function Call */
    /* return void  */
     BuiltinTypeLongDouble(_ld_double);

    /* return print */
    cout<<"BuiltinTypeLongDouble :"<<endl; 
    return 0;
 }
