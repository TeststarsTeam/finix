c语言驱动代码的说明 
=============================================
wings利用PSD的描述信息，完成单元测试驱动代码的生成。

驱动代码的命名规则
-----------------------
Wings生成的驱动代码，存储在drivercode文件夹中。

注：所有代码的命名规则采用google c++ 的命名规范，一些稍微特殊的除外。

（1）driver.cc与driver.h，主要针对程序中使用到的一些公共函数以及头文件

（2）同一个结构体或者联合体，可能会作为多个函数的参数使用，为了避免代码的重复，wings针对所有的结构体和联合体的不同类型，封装成不同的驱动函数或者参数捕获函数。driver_structorunion.cc 存储结构体驱动函数的代driver_structorunion.h 对应的头文件。

**举例说明如下：**

针对结构体信息，会对应生成下图中不同的驱动函数信息，如下图所示：
::

  struct coordinate_s DriverStructcoordinate_s(cJSON *coordinate_sRoot);
  struct coordinate_s *DriverStructcoordinate_sPoint(cJSON *coordinate_sRootPoint, int row);
  struct coordinate_s **DriverStructcoordinate_sPointPoint(cJSON *coordinate_sRootPointPoint, int row, int column);
  

（3）结构体实现函数的命名规则为：DriverStruct+结构体名字+类型，其中Point代表一级指针或者一维数组，PointPoint代表二级指针或者二维数组使用。
源文件驱动的实现，以每个c文件以一个单元，针对每个c文件中的每个函数，生成对应的驱动函数

源文件的命名规则为：driver_+源文件名+.cc
例如：driver_nginx.cc

驱动函数的命名规则：Driver_+函数名
例如：Driver_ngx_show_version_info(void);
注意：针对static函数的测试，需要去掉static的函数

驱动函数之前添加原函数的声明，例如：
extern void ngx_show_version_info(void); Driver_ngx_show_version_info(void);

（4）返回值的打印输出
返回值的打印输出函数命名规则：Driver+Return+Print_+函数名。
例如：DriverReturnPrint_ngx_show_version_info();

（5）用户源代码中的main函数需要手动注释掉，wings会在驱动代码中，重新生成一个main函数文件，来进行测试。Wings会生成驱动main的主函数文件为:gtest_auto_main.cc
注意：用户依据需要，自行选择使用。


驱动代码的简单说明
-----------------------
（1）Wings主要针对参数进行逐层展开，解析到最底层为基本类型进行处理。驱动的赋值部分，主要就是针对基本类型进行处理。（注：特殊类型，比如FILE等，后面会详细讲解如何赋值）
针对int类型，进行说明：

int p;  int *p;  int **p;  int ***p;

int p[1];  int p[2][3];  int p[1][2][3];

int(*p)[];  int(*p)[][3];  int *(*p)[];  int (**p)[];

int *a[];  int **a[];  int *a[][3];  int (*a[])[];

Wings会针对基本类型的以上15中类型，进行不同的赋值。

举例如下：

函数原型：如下图中的int coordinate_destory（location_s *loc,size_t length,char *ch）

.. image:: /image/figure6.png

其中返回值的类型为int

第一个参数的类型为location *

第二个参数的类型为size_t

第三个参数的类型为char *

针对以上三个参数进行赋值。

wings针对coordinate_destory函数的完整驱动代码在以下部分进行详细说明.

gtest_auto_main.cc 此文件为调用gtest的入口文件，内容如下图所示：

.. image:: /image/figure7.png

针对每个测试的.c文件，会生成对应的driver_文件名_gtest.cc文件，针对函数coordinate_destory的返回值进行期望对比操作。

如下图中，获取函数的返回值为return，用户填写的期望的返回值为expected。

.. image:: /image/figure8.png

**下图为被测函数的值文件。**

.. image:: /image/figure9.png

**下图为对应的驱动代码。**

::

  extern int coordinate_destroy(location *loc, size_t length, char *ch);
  int coordinate_destroy_intTimes = 0;
  int Drive_coordinate_destroy(int times)
  {
      coordinate_destroy_intTimes = times;
      /* Root is the json object of the value file.coordinate_destroy_int_Root is function.coordinate_destroy_int is json object.  */
      const char *jsonFile = "../drivervalue/wings_c_demo_coordinates/coordinate_destroy_int.json";
      char *json_data = get_json_data(jsonFile);
      cJSON *Root = cJSON_Parse(json_data);
      int coordinate_destroy_int_len = strlen("coordinate_destroy_int");
      char *coordinate_destroy_int_sp = (char *)malloc(sizeof(char) * (coordinate_destroy_int_len + 3));
      sprintf(coordinate_destroy_int_sp, "coordinate_destroy_int%d", times);
      cJSON *coordinate_destroy_int_Root = cJSON_GetObjectItem(Root, coordinate_destroy_int_sp);
      free(coordinate_destroy_int_sp);
      /*It is the 1 global variable: count    coordinate_destroy */
      int _count = cJSON_GetObjectItem(coordinate_destroy_int_Root, "count")->valueint;
      count = _count;
      /*It is the 1 parameter: loc    coordinate_destroy*/
      cJSON *loc_Arr_Root = cJSON_GetObjectItem(coordinate_destroy_int_Root, "loc");
      int loc_size = cJSON_GetArraySize(loc_Arr_Root);
      struct location_s *_loc = DriverStructlocation_sPoint(loc_Arr_Root, loc_size);
      /*It is the 2 parameter: length    coordinate_destroy*/
      unsigned int _length = (unsigned int)cJSON_GetObjectItem(coordinate_destroy_int_Root, "length")->valueint;
      /*It is the 3 parameter: ch    coordinate_destroy*/
      char *_ch;
      {
          char *_ch_str = cJSON_GetObjectItem(coordinate_destroy_int_Root, "ch")->valuestring;
          _ch = (char *)malloc(sizeof(char) * (strlen(_ch_str) + 1));
          memcpy(_ch, _ch_str, strlen(_ch_str));
          _ch[strlen(_ch_str)] = '\0';
      }
      //Function Call
      int returnType = coordinate_destroy(_loc, _length, _ch);
      return 0;
  }


::

  struct location_s *DriverStructlocation_sPoint(cJSON *location_sRootPoint, int row)
  {
      struct location_s *_location_s = (struct location_s *)malloc(sizeof(struct location_s) * row);
      for (int i = 0; i < row; i++)
      {
          cJSON *location_s_Root = cJSON_GetArrayItem(location_sRootPoint, i);
  
          int **_mPoi;
          cJSON *mPoi_Root = cJSON_GetObjectItem(location_s_Root, "mPoi");
          int mPoi_row = cJSON_GetArraySize(mPoi_Root);
          _mPoi = (int **)malloc(sizeof(int *) * mPoi_row);
          for (int i = 0; i < mPoi_row; i++)
          {
              cJSON *mPoi_Root_Row = cJSON_GetArrayItem(mPoi_Root, i);
              int mPoi_column = cJSON_GetArraySize(mPoi_Root_Row);
              _mPoi[i] = (int *)malloc(sizeof(int) * mPoi_column);
              for (int j = 0; j < mPoi_column; j++)
              {
                  _mPoi[i][j] = cJSON_GetArrayItem(mPoi_Root_Row, j)->valueint;
              }
          }

          _location_s[i].mPoi = _mPoi;
          cJSON *coor_Arr_Root = cJSON_GetObjectItem(location_s_Root, "coor");

          int coor_size = cJSON_GetArraySize(coor_Arr_Root);
          struct coordinate_s *_coor = DriverStructcoordinate_sPoint(coor_Arr_Root, coor_size);

          _location_s[i].coor = _coor;
          cJSON *pf_Arr_Root = cJSON_GetObjectItem(location_s_Root, "pf");
          cJSON *pf_Root = cJSON_GetArrayItem(pf_Arr_Root, 0);

          /* wingsParam1 */
          unsigned char *_wingsParam1;
          {
              char *_wingsParam1_str = cJSON_GetObjectItem(pf_Root, "wingsParam1")->valuestring;
              _wingsParam1 = (unsigned char *)malloc(sizeof(unsigned char) * (strlen(_wingsParam1_str) + 1));
              memcpy(_wingsParam1, (unsigned char *)_wingsParam1_str, strlen(_wingsParam1_str));
              _wingsParam1[strlen(_wingsParam1_str)] = '\0';
          }

          /* wingsParam2 */
          unsigned char *_wingsParam2;
          {
              char *_wingsParam2_str = cJSON_GetObjectItem(pf_Root, "wingsParam2")->valuestring;
              _wingsParam2 = (unsigned char *)malloc(sizeof(unsigned char) * (strlen(_wingsParam2_str) + 1));
              memcpy(_wingsParam2, (unsigned char *)_wingsParam2_str, strlen(_wingsParam2_str));
              _wingsParam2[strlen(_wingsParam2_str)] = '\0';
          }
          struct _iobuf *_pf = _iobufFunctionPointer(_wingsParam1, _wingsParam2);

          _location_s[i].pf = _pf;
          cJSON *next_Arr_Root = cJSON_GetObjectItem(location_s_Root, "next");

          int next_size = cJSON_GetArraySize(next_Arr_Root);
          struct location_s *_next = DriverStructlocation_sPoint(next_Arr_Root, next_size);

          _location_s[i].next = _next;
      }
      return _location_s;
  }

