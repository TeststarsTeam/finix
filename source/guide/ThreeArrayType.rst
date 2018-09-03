.. _ThreewoArrayType

三维数组
========

int类型
-------
原函数::

  void ArrayTypeIntThree(int array_three[2][3][4]);
  
PSD结构描述::

  <ArrayTypeIntThree parmType0="int [2][3][4]" parmNum="1">
    <array_three baseType1="ArrayType" RowSize="2" baseType2="ArrayType" ColumnSize="3" baseType3="ArrayType" HighSize="4" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
 </ArrayTypeIntThree>
 
驱动代码::

   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void ArrayTypeIntThree(int array_three[2][3][4])
    */
  int Drive_ArrayTypeIntThree(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayTypeIntThree_Root是函数ArrayTypeIntThree的json对象 */
    Json::Value ArrayTypeIntThree_Root = Root[MyStrCat("ArrayTypeIntThree",times)];

    /* 这是第1个参数:array_three    ArrayTypeIntThree
     *
     * 参数原型: int array_three[2][3][4]     
     */


    /* array_three */
    int _array_three[2][3][4];
    for(int Row = 0; Row < 2; Row++)
    {
        for(int Column = 0; Column < 3; Column++)
        {
            for(int High = 0; High < 4; High++)
            {
                _array_three[Row][Column][High] = ArrayTypeIntThree_Root["array_three"][Row][Column][High].asInt();
            }
        }
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeIntThree(_array_three);

    /* return print */
    cout<<"ArrayTypeIntThree :"<<endl; 
    return 0;
 }

char 类型
----------
原函数::

  void ArrayTypeCharThree(char array_three[2][3][16]);
  
PSD结构描述::

  <ArrayTypeCharThree parmType0="char [2][3][16]" parmNum="1">
    <array_three baseType1="ArrayType" RowSize="2" baseType2="ArrayType" ColumnSize="3" baseType3="ArrayType" HighSize="16" type="ZOA_CHAR_S" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </ArrayTypeCharThree>
  
驱动代码::

    /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
     * 函数原型:
     * void ArrayTypeCharThree(char array_three[2][3][16])
     */
  int Drive_ArrayTypeCharThree(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,ArrayTypeCharThree_Root是函数ArrayTypeCharThree的json对象 */
    Json::Value ArrayTypeCharThree_Root = Root[MyStrCat("ArrayTypeCharThree",times)];

    /* 这是第1个参数:array_three    ArrayTypeCharThree
     *
     * 参数原型: char array_three[2][3][16]     
     */


    /* array_three */
    char _array_three[2][3][16];
    for(int Row = 0; Row < 2; Row++)
    {
        for(int Column = 0; Column < 3; Column++)
        {
            string _array_three_str = ArrayTypeCharThree_Root["array_three"][Row][Column].asString();
            for(int High = 0; High < 16 - 1; High++)
            {
                _array_three[Row][Column][High] = _array_three_str[High];
            }
            _array_three[Row][Column][16 - 1] = '\0';
        }
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeCharThree(_array_three);

    /* return print */
    cout<<"ArrayTypeCharThree :"<<endl; 
    return 0;
  }
int类型不定数组
---------------
原函数::

  void ArrayTypeNoSizeIntThree(int array_three[][3][4]);
  
PSD结构描述::

  <ArrayTypeNoSizeIntThree parmType0="int [][3][4]" parmNum="1">
    <array_three baseType1="ArrayType" RowSize="1" Incomplete="NoSizeArray" baseType2="ArrayType" ColumnSize="3" baseType3="ArrayType" HighSize="4" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </ArrayTypeNoSizeIntThree>
  
驱动代码::

 /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
  * 函数原型:
  * void ArrayTypeNoSizeIntThree(int array_three[][3][4])
  */
 int Drive_ArrayTypeNoSizeIntThree(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayTypeNoSizeIntThree_Root是函数ArrayTypeNoSizeIntThree的json对象 */
    Json::Value ArrayTypeNoSizeIntThree_Root = Root[MyStrCat("ArrayTypeNoSizeIntThree",times)];

    /* 这是第1个参数:array_three    ArrayTypeNoSizeIntThree
     *
     * 参数原型: int array_three[][3][4]     
     */


    /* array_three */
    int array_three_index = ArrayTypeNoSizeIntThree_Root["array_three"].size();
    int (*_array_three)[3][4] = new int[array_three_index][3][4];
    for(int Row = 0; Row < array_three_index; Row++)
    {
        for(int Column = 0; Column < 3; Column++)
        {
            for(int High = 0; High < 4; High++)
            {
                _array_three[Row][Column][High] = ArrayTypeNoSizeIntThree_Root["array_three"][Row][Column][High].asInt();
            }
        }
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeNoSizeIntThree(_array_three);

    /* return print */
    cout<<"ArrayTypeNoSizeIntThree :"<<endl; 
    return 0;
 }

char类型不定数组
----------
原函数::

 void ArrayTypeNoSizeCharThree(char array_three[][3][16]);
 
PSD结构描述::

  <ArrayTypeNoSizeCharThree parmType0="char [][3][16]" parmNum="1">
    <array_three baseType1="ArrayType" RowSize="1" Incomplete="NoSizeArray" baseType2="ArrayType" ColumnSize="3" baseType3="ArrayType" HighSize="16" type="ZOA_CHAR_S" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </ArrayTypeNoSizeCharThree>
  
驱动代码::

   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void ArrayTypeNoSizeCharThree(char array_three[][3][16])
    */
  int Drive_ArrayTypeNoSizeCharThree(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,ArrayTypeNoSizeCharThree_Root是函数ArrayTypeNoSizeCharThree的json对象 */
    Json::Value ArrayTypeNoSizeCharThree_Root = Root[MyStrCat("ArrayTypeNoSizeCharThree",times)];

    /* 这是第1个参数:array_three    ArrayTypeNoSizeCharThree
     *
     * 参数原型: char array_three[][3][16]     
     */


    /* array_three */
    int array_three_index = ArrayTypeNoSizeCharThree_Root["array_three"].size();
    char (*_array_three)[3][16] = new char[array_three_index][3][16];
    for(int Row = 0; Row < array_three_index; Row++)
    {
        for(int Column = 0; Column < 3; Column++)
        {
            string _array_three_str = ArrayTypeNoSizeCharThree_Root["array_three"][Row][Column].asString();
            memcpy(_array_three, _array_three_str, _array_three_str.size());
        }
    }

    /* return & function Call */
    /* return void  */
     ArrayTypeNoSizeCharThree(_array_three);

    /* return print */
    cout<<"ArrayTypeNoSizeCharThree :"<<endl; 
    return 0;
 }