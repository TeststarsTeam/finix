.. _SystemType

系统头文件类型
==============
一些系统头文件的变量，例如File、pthread_mutex_t等，通过添加模板，由用户自定义添加赋值方式。 
在解析参数的过程中，通过判断当头文件不在当前工程中，则标记为系统的一些头文件，针对这些变量，finix不在继续往下进行分析。

原函数::

 void SystemVar_type(FILE *file);
 
PSD结构描述::

  <SystemVar_type parmType0="FILE *" parmNum="1">
    <file baseType1="PointerType" type="StructureOrClassType" name="_iobuf" SystemVar="_iobuf" />
    <returnType returnType="void" baseType1="BuiltinType" type="ZOA_VOID" />
  </SystemVar_type>
  
模板添加

.. image:: /image/system.png

驱动代码::

   /* 有参数的函数处理,Root,该文件对应的json，times是测试次数 
    * 函数原型:
    * void SystemVar_type(struct _iobuf *file)
    */
 int Drive_SystemVar_type(Json::Value Root, int times)
 {
   /* Root是值文件的json对象,SystemVar_type_Root是函数SystemVar_type的json对象 */
    Json::Value SystemVar_type_Root = Root[MyStrCat("SystemVar_type",times)];

    /* 这是第1个参数:file    SystemVar_type
     *
     * 参数原型: struct _iobuf *file     
     */

    /* 此处是系统内置类型，需手动赋值或者模板赋值 */

    /* file 模板里面定义该函数的赋值方法 */
    _iobuf *_file;
    {
        FILE *_file_tmp = fopen("E:/test.txt");
        _file = _file_tmp;
    }

    /* return & function Call */
    /* return void  */
     SystemVar_type(_file);

    /* return print */
    cout<<"SystemVar_type :"<<endl; 
    return 0;
 }

  
