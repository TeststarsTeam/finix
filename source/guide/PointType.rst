.. _PointType:

指针类型
===========

一级指针
--------
原函数::

  void  PointTypeOne(int *point_one);
  
PSD结构描述::

  <PointTypeOne parmType0="int *" parmNum="1">
    <point_one baseType1="PointerType" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </PointTypeOne>
  
驱动代码::
  
   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void PointTypeOne(int *point_one)
    */
  int Drive_PointTypeOne(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,PointTypeOne_Root是函数PointTypeOne的json对象 */
    Json::Value PointTypeOne_Root = Root[MyStrCat("PointTypeOne",times)];

    /* 这是第1个参数:point_one    PointTypeOne
     *
     * 参数原型: int *point_one     
     */


    /* point_one */
    int *_point_one;
    {
        int W_index = PointTypeOne_Root["point_one"].size();
        _point_one = new int[W_index];
        for(int i = 0; i < W_index; i++)
        {
            *(_point_one+i) = PointTypeOne_Root["point_one"][i].asInt();
        }
    }

    /* return & function Call */
    /* return void  */
     PointTypeOne(_point_one);

    /* return print */
    cout<<"PointTypeOne :"<<endl; 
    return 0;
  }

二级指针
--------
原函数::

  void  PointTypeTwo(int **point_two);
  
PSD结构描述::

  <PointTypeTwo parmType0="int **" parmNum="1">
    <point_two baseType1="PointerType" baseType2="PointerType" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </PointTypeTwo>
  
驱动代码::
  
   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void PointTypeTwo(int **point_two)
    */
  int Drive_PointTypeTwo(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,PointTypeTwo_Root是函数PointTypeTwo的json对象 */
    Json::Value PointTypeTwo_Root = Root[MyStrCat("PointTypeTwo",times)];

    /* 这是第1个参数:point_two    PointTypeTwo
     *
     * 参数原型: int **point_two     
     */


    /* point_two */
    int **_point_two;
    {
        int W_x = PointTypeTwo_Root["point_two"].size();
        _point_two = new int*[W_x];
        for(int i = 0; i < W_x; i++)
        {
            int W_y = PointTypeTwo_Root["point_two"][i].size();
            _point_two[i] = new int[W_y];
            for(int j = 0; j < W_y; j++)
            {
                _point_two[i][j] = PointTypeTwo_Root["point_two"][i][j].asInt();
            }
        }
    }

    /* return & function Call */
    /* return void  */
     PointTypeTwo(_point_two);

    /* return print */
    cout<<"PointTypeTwo :"<<endl; 
    return 0;
  }
  
三级指针
--------
原函数::

  void  PointTypeThree(int ***point_three);
  
PSD结构描述::
  
  <PointTypeThree parmType0="int ***" parmNum="1">
    <point_three baseType1="PointerType" baseType2="PointerType" baseType3="PointerType" type="ZOA_INT" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </PointTypeThree>
  
驱动代码::

   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void PointTypeThree(int ***point_three)
    */
  int Drive_PointTypeThree(Json::Value Root, int times)
  {
   /* Root是值文件的json对象,PointTypeThree_Root是函数PointTypeThree的json对象 */
    Json::Value PointTypeThree_Root = Root[MyStrCat("PointTypeThree",times)];

    /* 这是第1个参数:point_three    PointTypeThree
     *
     * 参数原型: int ***point_three     
     */


    /* point_three */
    int _point_three_tmp = PointTypeThree_Root["point_three"].asInt();
    int *_point_three_ptr = &_point_three_tmp;
    int **_point_three_pptr = &_point_three_ptr;
    int ***_point_three = &_point_three_pptr;

    /* return & function Call */
    /* return void  */
     PointTypeThree(_point_three);

    /* return print */
    cout<<"PointTypeThree :"<<endl; 
    return 0;
  }