数据表格
========================

Wings目前测试用例数据采用随机生成的方式，支持int、char、double、float、bool、char*类型。数据表格可以任意编辑以上类型的数值。

(1) wings数据表格将会针对参数进行展开，假如参数类型为结构类型，数据表格将分层展开结构的类型，到基本类型。

(2) 针对基本类型的指针类型，例如int *p;wings处理为不定长度的一维数组类型，int **p;处理为不定长度的二维数组类型，默认长度为3，数据表格界面可以点击进行添加和删除数据。

(3) 针对不定长度的数组作为函数参数，例如int p[];wings默认长度为1，用户依据需求，在数据表格界面进行任意添加和修改即可。

.. image:: /image/figure30.png