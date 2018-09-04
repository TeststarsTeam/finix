.. _ArrayPointType

数组指针
========

int *a[]
原函数::
 
  void PointerArrayType(int *a[3]);
PSD结构描述::

  <PointerArrayType parmType0="int *[3]" parmNum="1">
    <a baseType1="ArrayType" RowSize="3" baseType2="PointerType" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </PointerArrayType>

驱动代码::

   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void PointerArrayType(int *a[3])
    */
 int Drive_PointerArrayType(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,PointerArrayType_Root是函数PointerArrayType的json对象 */
    Json::Value PointerArrayType_Root = Root[MyStrCat("PointerArrayType",times)];

    /* 这是第1个参数:a    PointerArrayType
     *
     * 参数原型: int *a[3]     
     */

    /* a */
    int *_a[3];
    int _a_tmp[3];
    for(int len = 0; len < 3; len++)
    {
        _a_tmp[len] = PointerArrayType_Root["a"][len].asInt();
        _a[len] = &_a_tmp[len];
    }

    /* return & function Call */
    /* return void  */
     PointerArrayType(_a);

    /* return print */
    cout<<"PointerArrayType :"<<endl; 
    return 0;
 } 
int **a[] 
--------
原函数::
  
 void SecondPointerArrayType(int **a[3]); 
PSD结构描述::
  <SecondPointerArrayType parmType0="int **[3]" parmNum="1">
    <a baseType1="ArrayType" RowSize="3" baseType2="PointerType" baseType3="PointerType" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </SecondPointerArrayType>

驱动代码:: 
   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
   * 函数原型:
   * void SecondPointerArrayType(int **a[3])
   */
 int Drive_SecondPointerArrayType(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,SecondPointerArrayType_Root是函数SecondPointerArrayType的json对象 */
    Json::Value SecondPointerArrayType_Root = Root[MyStrCat("SecondPointerArrayType",times)];

    /* 这是第1个参数:a    SecondPointerArrayType
     *
     * 参数原型: int **a[3]     
     */

    /* a */
    int _a_tmp[3];
    int *_a_ptr[3];
    int **_a[3];
    for(int Size = 0; Size < 3; Size++)
    {
        _a_tmp[Size] = SecondPointerArrayType_Root["a"][Size].asInt();
        _a_ptr[Size] = &_a_tmp[Size];
        _a[Size] = &_a_ptr[Size];
    }

    /* return & function Call */
    /* return void  */
     SecondPointerArrayType(_a);

    /* return print */
    cout<<"SecondPointerArrayType :"<<endl; 
    return 0;
 }
int *a[][]
--------
原函数::

  void PointerSecondArrayType(int *a[2][3]);
PSD结构描述::

  <PointerSecondArrayType parmType0="int *[2][3]" parmNum="1">
    <a baseType1="ArrayType" RowSize="2" baseType2="ArrayType" ColumnSize="3" baseType3="PointerType" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </PointerSecondArrayType>

驱动代码:: 

   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void PointerSecondArrayType(int *a[2][3])
    */
 int Drive_PointerSecondArrayType(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,PointerSecondArrayType_Root是函数PointerSecondArrayType的json对象 */
    Json::Value PointerSecondArrayType_Root = Root[MyStrCat("PointerSecondArrayType",times)];

    /* 这是第1个参数:a    PointerSecondArrayType
     *
     * 参数原型: int *a[2][3]     
     */


    /* a */
    int _a_tmp[2][3];
    int *_a[2][3];
    for(int Row = 0; Row < 2; Row++)
    {
        for(int Column = 0; Column < 3; Column++)
        {
            _a_tmp[Row][Column] = PointerSecondArrayType_Root["a"][Row][Column].asInt();
            _a[Row][Column] = &_a_tmp[Row][Column];
        }
    }

    /* return & function Call */
    /* return void  */
     PointerSecondArrayType(_a);

    /* return print */
    cout<<"PointerSecondArrayType :"<<endl; 
    return 0;
 } 
int (*a[])[]
--------
原函数::

  void ArrayPointerArrayType(int (*a[2])[3]);
PSD结构描述::

 <ArrayPointerArrayType parmType0="int (*[2])[3]" parmNum="1">
    <a baseType1="ArrayType" RowSize="2" baseType2="PointerType" baseType3="ArrayType" HighSize="3" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </ArrayPointerArrayType>
驱动代码:: 

 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
  * 函数原型:
  * void ArrayPointerArrayType(int (*a[2])[3])
  */
 int Drive_ArrayPointerArrayType(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayPointerArrayType_Root是函数ArrayPointerArrayType的json对象 */
    Json::Value ArrayPointerArrayType_Root = Root[MyStrCat("ArrayPointerArrayType",times)];

    /* 这是第1个参数:a    ArrayPointerArrayType
     *
     * 参数原型: int (*a[2])[3]     
     */


    /* a */
    int _a_tmp[2][3];
    int (*_a[2])[3];
    for(int Row = 0; Row < 2; Row++)
    {
        for(int High = 0; High < 3; High++)
        {
            _a_tmp[Row][High] = ArrayPointerArrayType_Root["a"][Row][High].asInt();
        }
        _a[Row] = &_a_tmp[Row];
    }

    /* return & function Call */
    /* return void  */
     ArrayPointerArrayType(_a);

    /* return print */
    cout<<"ArrayPointerArrayType :"<<endl; 
    return 0;
 } 