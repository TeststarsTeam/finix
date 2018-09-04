.. _PointArrayType

指针数组
========

int (*a)[]
-------
原函数::
 
  void ArrayPointerType(int (*a)[3]);
  
PSD结构描述::
 
  <ArrayPointerType parmType0="int (*)[3]" parmNum="1">
    <a baseType1="PointerType" baseType2="ArrayType" ColumnSize="3" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </ArrayPointerType>
  
驱动代码::
 
   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void ArrayPointerType(int (*a)[3])
    */
 int Drive_ArrayPointerType(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayPointerType_Root是函数ArrayPointerType的json对象 */
    Json::Value ArrayPointerType_Root = Root[MyStrCat("ArrayPointerType",times)];

    /* 这是第1个参数:a    ArrayPointerType
     *
     * 参数原型: int (*a)[3]     
     */

    /* a */
    int (*_a)[3];
    int _a_tmp[3];
    for(int len = 0; len < 3; len++)
    {
        _a_tmp[len] = ArrayPointerType_Root["a"][len].asInt();
    }
    _a = &_a_tmp;

    /* return & function Call */
    /* return void  */
     ArrayPointerType(_a);

    /* return print */
    cout<<"ArrayPointerType :"<<endl; 
    return 0;
 }
int (*a)[][] 
-------

原函数::
 
  void SecondArrayPointerType(int (*a)[2][3]);
  
PSD结构描述::
 
  <SecondArrayPointerType parmType0="int (*)[2][3]" parmNum="1">
    <a baseType1="PointerType" baseType2="ArrayType" ColumnSize="2" baseType3="ArrayType" HighSize="3" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </SecondArrayPointerType>
  
驱动代码::
  
   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void SecondArrayPointerType(int (*a)[2][3])
    */
  int Drive_SecondArrayPointerType(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,SecondArrayPointerType_Root是函数SecondArrayPointerType的json对象 */
    Json::Value SecondArrayPointerType_Root = Root[MyStrCat("SecondArrayPointerType",times)];

    /* 这是第1个参数:a    SecondArrayPointerType
     *
     * 参数原型: int (*a)[2][3]     
     */

    /* a */
    int (*_a)[2][3];
    int _a_tmp[2][3];
    for(int Column = 0; Column < 2; Column++)
    {
        for(int High = 0; High < 3; High++)
        {
            _a_tmp[Column][High] = SecondArrayPointerType_Root["a"][Column][High].asInt();
        }
    }
    _a = &_a_tmp;

    /* return & function Call */
    /* return void  */
     SecondArrayPointerType(_a);

    /* return print */
    cout<<"SecondArrayPointerType :"<<endl; 
    return 0;
 }
int *(*a)[] 
-------
原函数::
 
  void PointerArrayPointerType(int *(*a)[3]);
  
PSD结构描述::
 
  <PointerArrayPointerType parmType0="int *(*)[3]" parmNum="1">
    <a baseType1="PointerType" baseType2="ArrayType" ColumnSize="3" baseType3="PointerType" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </PointerArrayPointerType>
  
驱动代码::

   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void PointerArrayPointerType(int *(*a)[3])
    */
 int Drive_PointerArrayPointerType(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,PointerArrayPointerType_Root是函数PointerArrayPointerType的json对象 */
    Json::Value PointerArrayPointerType_Root = Root[MyStrCat("PointerArrayPointerType",times)];

    /* 这是第1个参数:a    PointerArrayPointerType
     *
     * 参数原型: int *(*a)[3]     
     */

    /* a */
    int _a_tmp[3];
    int *_a_ptr[3];
    int *(*_a)[3];
    for(int Size = 0; Size < 3; Size++)
    {
        _a_tmp[Size] = PointerArrayPointerType_Root["a"][Size].asInt();
        _a_ptr[Size] = &_a_tmp[Size];
    }
    _a = &_a_ptr;

    /* return & function Call */
    /* return void  */
     PointerArrayPointerType(_a);

    /* return print */
    cout<<"PointerArrayPointerType :"<<endl; 
    return 0;
 }
int (**a)[]
-------
原函数::
 
  void ArraySecondPointerType(int (**a)[3]);
  
PSD结构描述::

  <ArraySecondPointerType parmType0="int (**)[3]" parmNum="1">
    <a baseType1="PointerType" baseType2="PointerType" baseType3="ArrayType" HighSize="3" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </ArraySecondPointerType>

驱动代码::

    /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
     * 函数原型:
     * void ArraySecondPointerType(int (*a)[3])
     */
 int Drive_ArraySecondPointerType(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArraySecondPointerType_Root是函数ArraySecondPointerType的json对象 */
    Json::Value ArraySecondPointerType_Root = Root[MyStrCat("ArraySecondPointerType",times)];

    /* 这是第1个参数:a    ArraySecondPointerType
     *
     * 参数原型: int (*a)[3]     
     */

    /* a */
    int _a_tmp[3];
    int (*_a_ptr)[3];
    int (**_a)[3];
    for(int High = 0; High < 3; High++)
    {
        _a_tmp[High] = ArraySecondPointerType_Root["a"][High].asInt();
    }
    _a_ptr = &_a_tmp;
    _a = &_a_ptr;

    /* return & function Call */
    /* return void  */
     ArraySecondPointerType(_a);

    /* return print */
    cout<<"ArraySecondPointerType :"<<endl; 
    return 0;
 }