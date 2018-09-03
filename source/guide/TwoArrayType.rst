.. _TwoArrayType

二维数组
========

int类型
-------
原函数::

 void ArrayTypeIntTwo(int array_two[3][4]);

PSD结构描述::
  
  <ArrayTypeIntTwo parmType0="int [3][4]" parmNum="1">
    <array_two baseType1="ArrayType" RowSize="3" baseType2="ArrayType" ColumnSize="4" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </ArrayTypeIntTwo>
 
驱动代码::

  /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
   * 函数原型:
   * void ArrayTypeIntTwo(int array_two[3][4])
   */
 int Drive_ArrayTypeIntTwo(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayTypeIntTwo_Root是函数ArrayTypeIntTwo的json对象 */
    Json::Value ArrayTypeIntTwo_Root = Root[MyStrCat("ArrayTypeIntTwo",times)];

    /* 这是第1个参数:array_two    ArrayTypeIntTwo
     *
     * 参数原型: int array_two[3][4]     
     */


    /* array_two */
    int _array_two[3][4];
    for(int Row = 0; Row < 3; Row++)
    {
        for(int Column = 0; Column < 4; Column++)
        {
            _array_two[Row][Column] = ArrayTypeIntTwo_Root["array_two"][Row][Column].asInt();
        }
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeIntTwo(_array_two);

    /* return print */
    cout<<"ArrayTypeIntTwo :"<<endl; 
    return 0;
 }

char 类型
----------
原函数::

  void ArrayTypeCharTwo(char array_two[3][16]);
  
  
PSD结构描述::

  <ArrayTypeCharTwo parmType0="char [3][16]" parmNum="1">
    <array_two baseType1="ArrayType" RowSize="3" baseType2="ArrayType" ColumnSize="16" type="ZOA_CHAR_S" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </ArrayTypeCharTwo>
  
驱动代码::

   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void ArrayTypeCharTwo(char array_two[3][16])
    */
  int Drive_ArrayTypeCharTwo(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,ArrayTypeCharTwo_Root是函数ArrayTypeCharTwo的json对象 */
    Json::Value ArrayTypeCharTwo_Root = Root[MyStrCat("ArrayTypeCharTwo",times)];

    /* 这是第1个参数:array_two    ArrayTypeCharTwo
     *
     * 参数原型: char array_two[3][16]     
     */


    /* array_two */
    char _array_two[3][16];
    for(int Row = 0; Row < 3; Row++)
    {
        string _array_two_Str = ArrayTypeCharTwo_Root["array_two"][Row].asString();
        for(int Column = 0; Column < 16 - 1; Column++)
        {
            _array_two[Row][Column] = _array_two_Str[Column];
        }
        _array_two[Row][16 - 1] = '\0';
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeCharTwo(_array_two);

    /* return print */
    cout<<"ArrayTypeCharTwo :"<<endl; 
    return 0;
 }
 
int类型不定数组
---------------
原函数::

  void ArrayTypeNoSizeIntTwo(int array_two[][4]);
  
PSD结构描述::

  <ArrayTypeNoSizeIntTwo parmType0="int [][4]" parmNum="1">
    <array_two baseType1="ArrayType" RowSize="1" Incomplete="NoSizeArray" baseType2="ArrayType" ColumnSize="4" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </ArrayTypeNoSizeIntTwo>
  
驱动代码::

   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void ArrayTypeNoSizeIntTwo(int array_two[][4])
    */
  int Drive_ArrayTypeNoSizeIntTwo(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayTypeNoSizeIntTwo_Root是函数ArrayTypeNoSizeIntTwo的json对象 */
    Json::Value ArrayTypeNoSizeIntTwo_Root = Root[MyStrCat("ArrayTypeNoSizeIntTwo",times)];

    /* 这是第1个参数:array_two    ArrayTypeNoSizeIntTwo
     *
     * 参数原型: int array_two[][4]     
     */


    /* array_two */
    int array_two_index = ArrayTypeNoSizeIntTwo_Root["array_two"].size();
    int (*_array_two)[4] = new int[array_two_index][4];
    for(int Row = 0; Row < 1; Row++)
    {
        for(int Column = 0; Column < 4; Column++)
        {
            _array_two[Row][Column] = ArrayTypeNoSizeIntTwo_Root["array_two"][Row][Column].asInt();
        }
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeNoSizeIntTwo(_array_two);

    /* return print */
    cout<<"ArrayTypeNoSizeIntTwo :"<<endl; 
    return 0;
 }

char类型不定数组
----------
原函数::

  void ArrayTypeNoSizeCharTwo(char array_two[][16]);
  
PSD结构描述::

  <ArrayTypeNoSizeCharTwo parmType0="char [][16]" parmNum="1">
    <array_two baseType1="ArrayType" RowSize="1" Incomplete="NoSizeArray" baseType2="ArrayType" ColumnSize="16" type="ZOA_CHAR_S" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </ArrayTypeNoSizeCharTwo>
  
驱动代码::

   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void ArrayTypeNoSizeCharTwo(char array_two[][16])
    */
  int Drive_ArrayTypeNoSizeCharTwo(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,ArrayTypeNoSizeCharTwo_Root是函数ArrayTypeNoSizeCharTwo的json对象 */
    Json::Value ArrayTypeNoSizeCharTwo_Root = Root[MyStrCat("ArrayTypeNoSizeCharTwo",times)];

    /* 这是第1个参数:array_two    ArrayTypeNoSizeCharTwo
     *
     * 参数原型: char array_two[][16]     
     */


    /* array_two */
    int array_two_index = ArrayTypeNoSizeCharTwo_Root["array_two"].size();
    char (*_array_two)[16] = new char[array_two_index][16];
    for(int Row = 0; Row < array_two_index; Row++)
    {
        string _array_two_Str = ArrayTypeNoSizeCharTwo_Root["array_two"][Row].asString();
        memcpy(_array_two[Row], _array_two_Str.c_str(),_array_two_Str.size());
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeNoSizeCharTwo(_array_two);

    /* return print */
    cout<<"ArrayTypeNoSizeCharTwo :"<<endl; 
    return 0;
  }