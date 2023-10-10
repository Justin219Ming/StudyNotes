1. 以fdw结尾的文件 大部分出现的结构体变量是有重合部分
2. 用于访问日志数据的外来数据包装器 
3. 使得数据库可以通过SQL查询语言对外部数据进行查询、插入、更新和删除操作，将外部数据集成到数据库中，以便于数据分析、联接查询等
4. 主要都是自定义的结构体变量


- Datum
sizeof(Datum) == sizeof(void *) == 4 or 8
sizeof(char) == 1
sizeof(short) == 2

- FunctionCallInfo
常在PostgreSQL中用于提供有关当前函数调用上下文的信息
typedef struct FunctionCallInfoData* FunctionCallInfo;

- FmgrInfo
用于存储和传递关于函数的信息，包括函数的地址、参数、返回值、属性

- text

typedef struct varlena text;
varlena，用于表示可变长度的数据

- logFdwPlanState
该结构体包含了一系列用于处理日志外部表的相关信息和函数指针

- planstate
于存储与查询计划节点相关的状态信息和功能函数

- FdwLogType 枚举的三种类型
enum FdwLogType { FLT_PG_LOG = 0, FLT_GS_PROFILE, FLT_UNKNOWN };
/*
FLT_PG_LOG：表示PostgreSQL日志类型。
FLT_GS_PROFILE：表示Greenplum性能分析日志类型。
FLT_UNKNOWN：表示未知的日志类型。*/

- typedef unsigned int Oid

- opexpr 表示操作符表达式
包含了操作符的相关信息，如操作符的OID、底层函数的OID、结果值的OID、是否返回集合、排序规则的OID以及操作符的参数等。

- var
表示变量表达式。它包含了变量的相关信息，如变量所属关系的索引、属性号、类型的OID、类型修饰符、排序规则的OID

- Node  NodeTag
枚举类型

- Const  它包含了常量的相关信息 表示常量表达式

- List*  ListCell 实现链表数据结构

- log_file_info 表示日志文件的信息

- __MINGW_EXTENSION 是一个编译器扩展，用于在使用MinGW编译器时声明一些特定的类型或函数。

- `typedef __int64 __time64_t;` 
`__int64` 是一个64位整数类型，通常用于表示时间戳或日期时间值。
`__time64_t` 的类型别名，它是一个64位整数类型，被用于表示时间相关的值。

- TimestampTz

- typedef int64 pg_time_t;

- Value 通用的值，可以是整数或字符串

- typedef struct ld_scanner* logdir_scanner;
表示日志扫描器的相关信息

- stat 用于在不包含 `NO_OLDNAMES` 定义的情况下。 定义了文件的状态信息

- dirent 表示目录中的一个条目（文件或子目录）的信息

- ld_scanner 一个日志目录扫描器的实现，用于管理和操作日志目录及其文件的相关信息

- addrinfo 定义了一个结构体 `addrinfo`，用于表示网络地址信息。


- typedef struct RelationData* Relation;
表示和管理关系（表或索引）的元数据和相关信息。它存储了关系的物理标识符、引用计数、缓存文件句柄、元组描述符、访问方法实现、索引列表、触发器信息、重写规则、行级安全策略等

- cmp_func
typedef int (*cmp_func)(const void*, const void*);

## gc_fdw
 Query deparser for gc_fdw

List* get_str_targetlist(List* fdw_private)

函数从fdw_private参数中提取目标列表中的字符串值，并将这些字符串值封装到一个新的列表中返回。


- PlannerInfo* 该结构体的目的是在查询规划和优化过程中存储和跟踪各种信息，以便在生成最佳查询计划时使用。

- RelOptInfo 结构体是在 PostgreSQL 数据库管理系统中的定义。该结构体表示查询优化期间查询规划器使用的关系（表或连接）的信息。

- ForeignScan 结构体是在 PostgreSQL 中用于表示使用外部数据包装器扫描外部表的查询执行计划节点的定义。

- ForeignScanState  用于在扫描外部数据时存储相关的状态信息和选项

- TupleTableSlot 存储元组数据的槽结构体，包含了元组的各种属性和状态信息，以及用于访问和操作元组的方法和工作空间

- VectorBatch类似于一个用于处理向量批处理数据的类。它包含了管理批处理数据的各种成员变量和方法，例如序列化和反序列化数据、打包数据、重置数据等

- VecForeignScanStat 在外部数据源上执行向量化扫描操作。它包含了处理扫描数据的批次对象、输出批次对象以及与扫描相关的上下文和状态信息。

- RelationData 结构体是在 PostgreSQL 中定义的一个数据结构。它是 PostgreSQL 数据库系统中用于表示关系（表或索引）的基本数据结构。

- gc_fdw_single 用来执行FDW选项处理
- deparse_single 提取目标列表中的字符串值，然后封装到一个新的列表中返回
- option_single 将自定义的结构体类型转换成对应的数据类型

## oracle_fdw

这段代码是一个Makefile文件，用于构建和安装Oracle FDW（Foreign Data Wrapper）扩展。

all：默认目标为oracle_fdw_target，即构建Oracle FDW。
install：默认目标为install-data，即安装Oracle FDW。
top_builddir：指定了顶层构建目录。
include $(top_builddir)/src/Makefile.global：包含全局Makefile文件。
CODE_DIR：指定了Oracle FDW的代码目录。
oracle_fdw_target目标：依赖于prep_checked目标，执行在ORCFDW_HOME目录下的make命令来构建Oracle FDW。其中，NO_PGXS=1表示不使用PGXS构建系统，TOP_DIR=$(abs_top_srcdir)指定了顶层源代码目录。
prep_checked目标：检查ORCFDW_HOME目录是否存在，如果不存在则输出错误信息，并退出1。如果存在，则在当前目录下创建一个名为prep_checked的文件，表示已经检查过。
install-data目标：依赖于oracle_fdw_target目标，执行在ORCFDW_HOME目录下的make命令来安装Oracle FDW。
uninstall、distclean、clean目标：分别用于卸载、清除构建产生的目标文件和库文件，以及删除prep_checked文件。
在构建和安装过程中，使用了ORCFDW_HOME环境变量来指定Oracle FDW的目录。

## mysql_fdw

用于构建和安装MySQL FDW（Foreign Data Wrapper）扩展。

all：默认目标为mysql_fdw_target，即构建MySQL FDW。
install：默认目标为install-data，即安装MySQL FDW。
top_builddir：指定了顶层构建目录。
include $(top_builddir)/src/Makefile.global：包含全局Makefile文件。
CODE_DIR：指定了MySQL FDW的代码目录。
mysql_fdw_target目标：依赖于prep_checked目标，执行在MYFDW_HOME目录下的make命令来构建MySQL FDW。其中，TOP_DIR=$(abs_top_srcdir)指定了顶层源代码目录。
prep_checked目标：检查MYFDW_HOME目录是否存在，如果不存在则输出错误信息，并退出1。如果存在，则在当前目录下创建一个名为prep_checked的文件，表示已经检查过。
install-data目标：依赖于mysql_fdw_target目标，执行在MYFDW_HOME目录下的make命令来安装MySQL FDW。
uninstall、distclean、clean目标：分别用于卸载、清除构建产生的目标文件和库文件，以及删除prep_checked文件。
在构建和安装过程中，使用了MYFDW_HOME环境变量来指定MySQL FDW的目录。

## postgres_fdw
- conncacheentry 通过定义ConnCacheKey结构体，可以创建一个用于缓存连接信息的键（key）。该键可以用于识别和检索特定外部服务器和用户映射的连接信息，以便在连接缓存中进行快速查找和重用。使用这样的键可以提高连接的效率和性能。