.. _OneArrayType

一维数组
========

int类型
-------
原函数::

void ArrayTypeIntOne(int array_one[2]);

PSD结构描述::

 <ArrayTypeIntOne parmType0="int [2]" parmNum="1">
    <array_one baseType1="ArrayType" RowSize="2" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </ArrayTypeIntOne>
 
驱动代码::

 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void ArrayTypeIntOne(int array_one[2])
 */
 int Drive_ArrayTypeIntOne(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayTypeIntOne_Root是函数ArrayTypeIntOne的json对象 */
    Json::Value ArrayTypeIntOne_Root = Root[MyStrCat("ArrayTypeIntOne",times)];

    /* 这是第1个参数:array_one    ArrayTypeIntOne
     *
     * 参数原型: int array_one[2]     
     */


    /* array_one */
    int _array_one[2];
    for(int len = 0; len < 2; len++)
    {
        _array_one[len] = ArrayTypeIntOne_Root["array_one"][len].asInt();
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeIntOne(_array_one);

    /* return print */
    cout<<"ArrayTypeIntOne :"<<endl; 
    return 0;
 }
 
char 类型
----------
原函数::

  void ArrayTypeCharOne(char array_one[16]);
  
PSD结构描述::
  
  <ArrayTypeCharOne parmType0="char [16]" parmNum="1">
    <array_one baseType1="ArrayType" RowSize="16" type="ZOA_CHAR_S" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </ArrayTypeCharOne>
  
驱动代码::
  
   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void ArrayTypeCharOne(char array_one[16])
 */
 int Drive_ArrayTypeCharOne(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayTypeCharOne_Root是函数ArrayTypeCharOne的json对象 */
    Json::Value ArrayTypeCharOne_Root = Root[MyStrCat("ArrayTypeCharOne",times)];

    /* 这是第1个参数:array_one    ArrayTypeCharOne
     *
     * 参数原型: char array_one[16]     
     */


    /* array_one */
    char _array_one[16];
    string _array_one_str = ArrayTypeCharOne_Root["array_one"].asString();
    for(int len = 0; len < 16 - 1; len++)
    {
        _array_one[len] = _array_one_str[len];
    }
    _array_one[16 - 1] = '\0';

    /* return & function Call */
    /* return void  */
     ArrayTypeCharOne(_array_one);

    /* return print */
    cout<<"ArrayTypeCharOne :"<<endl; 
    return 0;
 }
 
int类型不定数组
----------
原函数::

 void ArrayTypeNoSizeIntOne(int array_one[]);
 
PSD结构描述::

 <ArrayTypeNoSizeIntOne parmType0="int []" parmNum="1">
    <array_one baseType1="ArrayType" RowSize="1" Incomplete="NoSizeArray" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </ArrayTypeNoSizeIntOne>

 
驱动代码::


 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void ArrayTypeNoSizeIntOne(int array_one[])
 */
 int Drive_ArrayTypeNoSizeIntOne(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayTypeNoSizeIntOne_Root是函数ArrayTypeNoSizeIntOne的json对象 */
    Json::Value ArrayTypeNoSizeIntOne_Root = Root[MyStrCat("ArrayTypeNoSizeIntOne",times)];

    /* 这是第1个参数:array_one    ArrayTypeNoSizeIntOne
     *
     * 参数原型: int array_one[]     
     */


    /* array_one */
    int array_one_index = ArrayTypeNoSizeIntOne_Root["array_one"].size();
    int *_array_one = new int[array_one_index];
    for(int len = 0; len < array_one_index; len++)
    {
        _array_one[len] = ArrayTypeNoSizeIntOne_Root["array_one"][len].asInt();
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeNoSizeIntOne(_array_one);

    /* return print */
    cout<<"ArrayTypeNoSizeIntOne :"<<endl; 
    return 0;
 }

char类型不定数组
----------
原函数::

void ArrayTypeNoSizeCharOne(char array_one[]);

PSD结构描述::

 <ArrayTypeNoSizeCharOne parmType0="char []" parmNum="1">
    <array_one baseType1="ArrayType" RowSize="1" Incomplete="NoSizeArray" type="ZOA_CHAR_S" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </ArrayTypeNoSizeCharOne>
  
驱动代码::


 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
 * 函数原型:
 * void ArrayTypeNoSizeCharOne(char array_one[1])
 */
 int Drive_ArrayTypeNoSizeCharOne(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayTypeNoSizeCharOne_Root是函数ArrayTypeNoSizeCharOne的json对象 */
    Json::Value ArrayTypeNoSizeCharOne_Root = Root[MyStrCat("ArrayTypeNoSizeCharOne",times)];

    /* 这是第1个参数:array_one    ArrayTypeNoSizeCharOne
     *
     * 参数原型: char array_one[1]     
     */


    /* array_one */
    char *_array_one;
    {
        string _array_one_str = ArrayTypeNoSizeCharOne_Root["array_one"].asString();
        _array_one = new char[_array_one_str.size()];
        memcpy(_array_one,_array_one_str.c_str(),_array_one_str.size());
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeNoSizeCharOne(_array_one);

    /* return print */
    cout<<"ArrayTypeNoSizeCharOne :"<<endl; 
    return 0;
 }
