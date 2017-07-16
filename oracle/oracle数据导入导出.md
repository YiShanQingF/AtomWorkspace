# oracle数据导入导出

## 导出
```
C:\Users\qingshan>exp help=y

Export: Release 11.2.0.1.0 - Production on 星期二 9月 27 21:33:15 2016

Copyright (c) 1982, 2009, Oracle and/or its affiliates.  All rights reserved.



通过输入 EXP 命令和您的用户名/口令, 导出
操作将提示您输入参数:

     例如: EXP SCOTT/TIGER

或者, 您也可以通过输入跟有各种参数的 EXP 命令来控制导出
的运行方式。要指定参数, 您可以使用关键字:

     格式:  EXP KEYWORD=value 或 KEYWORD=(value1,value2,...,valueN)
     例如: EXP SCOTT/TIGER GRANTS=Y TABLES=(EMP,DEPT,MGR)
               或 TABLES=(T1:P1,T1:P2), 如果 T1 是分区表

USERID 必须是命令行中的第一个参数。

关键字   说明 (默认值)         关键字      说明 (默认值)
--------------------------------------------------------------------------
USERID   用户名/口令           FULL        导出整个文件 (N)
BUFFER   数据缓冲区大小        OWNER        所有者用户名列表
FILE     输出文件 (EXPDAT.DMP)  TABLES     表名列表
COMPRESS  导入到一个区 (Y)   RECORDLENGTH   IO 记录的长度
GRANTS    导出权限 (Y)          INCTYPE     增量导出类型
INDEXES   导出索引 (Y)         RECORD       跟踪增量导出 (Y)
DIRECT    直接路径 (N)         TRIGGERS     导出触发器 (Y)
LOG      屏幕输出的日志文件    STATISTICS    分析对象 (ESTIMATE)
ROWS      导出数据行 (Y)        PARFILE      参数文件名
CONSISTENT 交叉表的一致性 (N)   CONSTRAINTS  导出的约束条件 (Y)

OBJECT_CONSISTENT    只在对象导出期间设置为只读的事务处理 (N)
FEEDBACK             每 x 行显示进度 (0)
FILESIZE             每个转储文件的最大大小
FLASHBACK_SCN        用于将会话快照设置回以前状态的 SCN
FLASHBACK_TIME       用于获取最接近指定时间的 SCN 的时间
QUERY                用于导出表的子集的 select 子句
RESUMABLE            遇到与空格相关的错误时挂起 (N)
RESUMABLE_NAME       用于标识可恢复语句的文本字符串
RESUMABLE_TIMEOUT    RESUMABLE 的等待时间
TTS_FULL_CHECK       对 TTS 执行完整或部分相关性检查
TABLESPACES          要导出的表空间列表
TRANSPORT_TABLESPACE 导出可传输的表空间元数据 (N)
TEMPLATE             调用 iAS 模式导出的模板名
```

**工作中常用**

导出指定用户的所有数据
```
exp user/password@ORCL file=D:\user_data.dmp log=D:\exp_userdatalog.log owner=user
```
 导出指定的表
 ```
exp user/password@ORCL file=d:\table1-3_date.dmp  log=D:\exp_table1-3_date.log tables=(table1,table2，table3)
```


## 导入
```
C:\Users\qingshan>imp help=y

Import: Release 11.2.0.1.0 - Production on 星期二 9月 27 21:25:27 2016

Copyright (c) 1982, 2009, Oracle and/or its affiliates.  All rights reserved.



通过输入 IMP 命令和您的用户名/口令, 导入
操作将提示您输入参数:

     例如: IMP SCOTT/TIGER

或者, 可以通过输入 IMP 命令和各种参数来控制导入
的运行方式。要指定参数, 您可以使用关键字:

     格式:  IMP KEYWORD=value 或 KEYWORD=(value1,value2,...,valueN)
     例如: IMP SCOTT/TIGER IGNORE=Y TABLES=(EMP,DEPT) FULL=N
               或 TABLES=(T1:P1,T1:P2), 如果 T1 是分区表

USERID 必须是命令行中的第一个参数。

关键字   说明 (默认值)        关键字      说明 (默认值)
--------------------------------------------------------------------------
USERID   用户名/口令           FULL       导入整个文件 (N)
BUFFER   数据缓冲区大小        FROMUSER    所有者用户名列表
FILE     输入文件 (EXPDAT.DMP)  TOUSER     用户名列表
SHOW     只列出文件内容 (N)     TABLES      表名列表
IGNORE   忽略创建错误 (N)    RECORDLENGTH  IO 记录的长度
GRANTS   导入权限 (Y)          INCTYPE     增量导入类型
INDEXES   导入索引 (Y)         COMMIT       提交数组插入 (N)
ROWS     导入数据行 (Y)        PARFILE      参数文件名
LOG     屏幕输出的日志文件    CONSTRAINTS    导入限制 (Y)
DESTROY                覆盖表空间数据文件 (N)
INDEXFILE              将表/索引信息写入指定的文件
SKIP_UNUSABLE_INDEXES  跳过不可用索引的维护 (N)
FEEDBACK               每 x 行显示进度 (0)
TOID_NOVALIDATE        跳过指定类型 ID 的验证
FILESIZE               每个转储文件的最大大小
STATISTICS             始终导入预计算的统计信息
RESUMABLE              在遇到有关空间的错误时挂起 (N)
RESUMABLE_NAME         用来标识可恢复语句的文本字符串
RESUMABLE_TIMEOUT      RESUMABLE 的等待时间
COMPILE                编译过程, 程序包和函数 (Y)
STREAMS_CONFIGURATION  导入流的一般元数据 (Y)
STREAMS_INSTANTIATION  导入流实例化元数据 (N)
DATA_ONLY              仅导入数据 (N)

下列关键字仅用于可传输的表空间
TRANSPORT_TABLESPACE 导入可传输的表空间元数据 (N)
TABLESPACES 将要传输到数据库的表空间
DATAFILES 将要传输到数据库的数据文件
TTS_OWNERS 拥有可传输表空间集中数据的用户
```

**工作中常用**


```
imp user1/password@orcl file=D:\user_data.dmp fromuser=user touser=user1 log=D:\imp_user1date.log
```

## Oracle 11g用exp无法导出空表的处理发布方法

> 11G中有个新特性，当表无数据时，不分配 segment ，以节省空间

**解决方法**

> * insert 一行，再 rollback 就产生segment了(不推荐)
> * 设置 deferred_segment_creation 参数，该参数值默认是 TRUE，当改为 FALSE 时，
     将 deferred_segment_creation 参数调为 FALSE ，这样调整后建的表都会立即分配空间，但是调整前的表都不会改变
```
alter system set deferred_segment_creation=false;
```
> * 调整前的表分配 segment 方法
```
--用以下这句sql查找空表,再把查询结果导出，执行导出的语句。
select 'alter table '||table_name||' allocate extent;' from user_tables where num_rows=0
```
