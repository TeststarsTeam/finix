void和void功能函数指针使用
=============

void
-----------------------

**1.打开工程会进行xml读取**

::

	WingsXmlInfoStorage::OpenSpecialFile()


函数会对void.xml和functionPointer.xml进行读取，

将读取的值存放在vector容器中。

**2.点击特殊赋值中的void*设置**

如果文件夹中不存在void.xml,则在此处生成void.xml

**3.根据1步骤中存放含有void.xml的容器**

::

	void WingsShowVoidInfoWidget::StartCreateVoidInfoTable(QStandardItemModel *model_value, QString Path)


在此函数执行后生成，生成树状表格

**4.在void*界面中会有改工程中的所需要填写的void*的真正类型**

.. image:: /image/figure31.png

**5.在reference栏中填写正在的类型，点击保存，即可生成void.xml文件。在文件中可以看到保存的真实类型。**

::

	void on_pushButton_save_clicked();


保存完后，会重新执行xmltostorage，读取xml，更新保存void的容器中的信息

**6.执行VoidPointerReplace()函数。**

遍历保存void的容器，和recordxml容器里面的信息，对比它们属于的的类名或者函数名

如果参数不是结构体真实值保存在voidPointerVar中，

如果参数是结构体或者类，存放在paramChildInfo中的是整个结构体或者类结构上所有的void*的信息

**7.在进行驱动生成等操作时候，检测参数类型是否为void*。**

如果参数为void*，将参数上的voidPointerVar替换原本的typeinfo

如果该参数为结构体其中含有void*，其中paramChildInfo中保存的是这个结构体或者类的全部void*的真实信息，需要进行处理，将结构体中的每个参数的paramChildInfo都存放上属于自己的真实类型


FuncChilderVecReplace


该函数用来处理这种情况

注:函数指针处理方式和void*相同



void*功能使用
-----------------------

1.打开工程后点击特殊赋值，选中特殊赋值栏中的（void*）设置

.. image:: /image/figure32.png

2.在界面中配置void*的类型

.. image:: /image/figure33.png

在途中Reference类型栏中填写void*的真正类型

在这里可以配置数组的维数长度，最多可使用三维数组

.. image:: /image/figure34.png

3.配置完成后点击保存按键，此时void.xml中以及保存了之前配置的基本信息

.. image:: /image/figure35.png

4.配置完成后，点击驱动生成。在以及配置过的类或者函数中，void*将被替换为配置好的值进行驱动生成或者值生成

例如：此处的 Test中的ptr_成员变量就会按照配置好的类型int进行值生成

.. image:: /image/figure36.png

注释：函数指针配置方式相同