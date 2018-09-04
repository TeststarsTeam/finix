.. _LinkNodeType

链表类型
========

finix中针对链表类型，采用比较灵活的赋值方式，考虑到实际应用中的一些因素，我们针对链表类型，默认赋值两层结构，在实际测试过程中，用户可依据需要自动添加节点。

原函数::

  struct s {
	int a;
	struct s * b;
  };

  void fun_1(struct s * p);
  
PSD结构描述::
  
  <fun_1 parmType0="struct s *" parmNum="1">
    <p baseType1="PointerType" type="StructureOrClassType" name="s">
        <a baseType1="BuiltinType" type="ZOA_INT" />
        <b NodeType="LinkNode" baseType1="PointerType" type="StructureOrClassType" name="s" />
    </p>
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </fun_1>
  
驱动代码::

  /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
   * 函数原型:
   * void fun_1(struct s *p)
   */
 int Drive_fun_1(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,fun_1_Root是函数fun_1的json对象 */
    Json::Value fun_1_Root = Root[MyStrCat("fun_1",times)];

    /* 这是第1个参数:p    fun_1
     *
     * 参数原型: struct s *p     
     */

    /* _p_Root 是p对应的json串 */
    Json::Value _p_Root = fun_1_Root["p"];

    struct s *_p;
    {
        struct s _p_tmp;

        /* a */
        int _a = _p_Root["a"].asInt();
        _p_tmp.a = _a;

        /* b LinkNode */
        Json::Value _b_Root = _p_Root["b"];
        _p_tmp.b = s_linknode_assign(_b_Root);
        _p = &_p_tmp;
    }

    /* return & function Call */
    /* return void  */
     fun_1(_p);

    /* return print */
    cout<<"fun_1 :"<<endl; 
    return 0;
 }

 struct s * s_linknode_assign(Json::Value Link_Root)
 {
    if(Link_Root.empty())
    {
        return NULL;
    }
    else
    {
        struct s* _p = new struct s;
        struct s _p_tmp;

        /* a */
        int _a = Link_Root["a"].asInt();
        _p_tmp.a = _a;

        /* b LinkNode */
        Json::Value _b_Root = Link_Root["b"];
        _p_tmp.b = s_linknode_assign(_b_Root);
        _p = &_p_tmp;
        return _p;
    }
 }