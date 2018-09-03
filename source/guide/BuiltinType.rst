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