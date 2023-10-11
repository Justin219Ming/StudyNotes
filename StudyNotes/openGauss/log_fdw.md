
### log_fdw.cpp

-  ���ڷ�����־���ݵ��������ݰ�װ��
-  openGauss ���� Mulan PSL v2 �����ɡ�

FDW��Foreign Data Wrapper���ⲿ���ݰ�װ��������д������һ�����ݿ�ϵͳ����չģ�飬����ʵ�����ݿ����ⲿ����Դ֮��Ľ�����FDW�������ݿ�ϵͳͨ�������ʹ���ʵ��Ľӿڣ�����ʱ��ر�һ�������ⲿ����Դ�����ݡ�

ʹ��FDW�����ݿ���Խ��ⲿ����Դ�����������ݿ⡢�ļ�ϵͳ��Web����ȣ��е�����ӳ�䵽���ݿ��е�������Ӷ�ʵ�ֶ��ⲿ����Դ��͸�����ʡ���ʹ�����ݿ����ͨ��SQL��ѯ���Զ��ⲿ���ݽ��в�ѯ�����롢���º�ɾ�����������ⲿ���ݼ��ɵ����ݿ��У��Ա������ݷ��������Ӳ�ѯ�ȡ�

FDWΪ���ݿ�ϵͳ�ṩ�����¹��ܺͺô���

1. ���ݼ��ɣ����Խ��ⲿ����Դ�����������ݿ��е����ݽ��м��ɲ�ѯ�������ݷ��ʺͷ�����
1. �����ͨ��������������ⲿ����Դ������ӳ�䵽���ݿ��У�ʹ���ڲ�ѯ�б���Ϊ���ر�
1. Զ�̷��ʣ�ͨ��FDW�����ݿ����ͨ���������Զ������Դ��ʵ�ֿ����ݿ�ϵͳ�����ݽ�����
1. �����Ż���FDW��������ⲿ����Դ���������Ż�����������Զ������Դ���������������Ƶȼ�����
1. ���ݰ�ȫ�ԣ�FDW����ͨ����ȫ��֤�ͷ���Ȩ�޿������������ⲿ����Դ�ķ��ʡ�

�ܶ���֮��FDW��һ�����ݿ���չģ�飬ͨ������ӿں�ʵ�ֻص�������ʹ���ݿ��ܹ�͸���ط��ʺͲ����ⲿ����Դ�����ݡ����ṩ��һ������ǿ��Ļ��ƣ��������ݼ��ɺͿ�����Դ��ѯ����չ�����ݿ�ϵͳ�Ĺ��ܺ�Ӧ�ó�����

### ����
- extern Datum get_hostname(PG_FUNCTION_ARGS);
������ȡ������
#define PG_FUNCTION_ARGS FunctionCallInfo fcinfo

sizeof(Datum) == sizeof(void *) == 4 or 8
sizeof(char) == 1
sizeof(short) == 2

```c++
Datum get_hostname(PG_FUNCTION_ARGS)
{
    char hostname[256] = {0};

    (void)gethostname(hostname, 255);

    PG_RETURN_TEXT_P(cstring_to_text(hostname));
}

int gethostname(char* name, int namelen)
{
    static struct utsname mname;
    static int called = 0;
    errno_t ss_rc = 0;
    if (!called) {
        called++;
        uname(&mname);
    }
    ss_rc = strncpy_s(name, namelen, mname.nodename, ((SYS_NMLN < namelen) ? SYS_NMLN : namelen));
    securec_check_c(ss_rc, "\0", "\0");
    return 0;
}

static inline Datum get_hostname_text(void)
{
    /* get hostname */
    FunctionCallInfo fcinfo = NULL;
    return get_hostname(fcinfo);
}
```


- FunctionCallInfoͨ����PostgreSQL�������ṩ�йص�ǰ�������������ĵ���Ϣ��

> typedef struct FunctionCallInfoData* FunctionCallInfo;
struct FunctionCallInfoData �ṹ���ָ����������Ϊ FunctionCallInfo��

```c
typedef struct
{
    FmgrInfo   *flinfo;         /* ptr to lookup info used for this call */
    Node       *context;        /* pass info about context of call */
    Node       *resultinfo;     /* pass or return extra info about result */
    Oid         fncollation;    /* collation for function to use */
    bool        isnull;         /* function must set true if result is NULL */
    short       nargs;          /* # arguments actually passed */
    Datum       arg[FUNC_MAX_ARGS];  /* Arguments passed to function */
    bool        argnull[FUNC_MAX_ARGS];  /* T if arg[i] is actually NULL */
} FunctionCallInfoData;
typedef FunctionCallInfoData* FunctionCallInfo;```

flinfo��ָ�����ڸõ��õĲ�����Ϣ��ָ�롣
context�������йص��������ĵ���Ϣ��ָ�롣
resultinfo�����ݻ򷵻��йؽ���Ķ�����Ϣ��ָ�롣
fncollation������ʹ�õ��������collation���� Oid��
isnull��������Ϊ NULL���������뽫������Ϊ true��
nargs��ʵ�ʴ��ݵĲ���������
arg�����ݸ������Ĳ��������顣
argnull����ʾÿ�������Ƿ�Ϊ NULL �Ĳ������顣

typedef struct FmgrInfo {
    PGFunction fn_addr;       /* pointer to function or handler to be called */
    Oid fn_oid;               /* OID of function (NOT of handler, if any) */
    short fn_nargs;           /* 0..FUNC_MAX_ARGS, or -1 if variable arg
                               * count */
    bool fn_strict;           /* function is "strict" (NULL in => NULL out) */
    bool fn_retset;           /* function returns a set */
    unsigned char fn_stats;   /* collect stats if track_functions > this */
    void* fn_extra;           /* extra space for use by handler */
    MemoryContext fn_mcxt;    /* memory context to store fn_extra in */
    fmNodePtr fn_expr;        /* expression parse tree for call, or NULL */
    Oid fn_rettype;           // Oid of function return type
    Oid fn_rettypemod;        /* Oid of the function returnt typmod */
    char fnName[NAMEDATALEN]; /* function name */
    char* fnLibPath;          /* library path for c-udf
                               * package.class.method(args) for java-udf */
    bool fn_fenced;
    Oid fn_languageId; /* function language id*/
    char fn_volatile;  /* procvolatile */
    // Vector Function
    VectorFunction vec_fn_addr;
    VectorFunction* vec_fn_cache;
    GenericFunRuntime* genericRuntime;
} FmgrInfo;

��δ��붨����һ����Ϊ`FmgrInfo`�Ľṹ�塣�����Ǹýṹ��ĸ�����Ա�Ĺ��ܽ��ͣ�

- `fn_addr`��ָ��Ҫ���õĺ�����������ָ�롣
- `fn_oid`�������Ķ����ʶ����OID���������Ǵ������ı�ʶ��������У���
- `fn_nargs`�������Ĳ�����������Χ��0��`FUNC_MAX_ARGS`��������������ǿɱ�ģ���Ϊ-1��
- `fn_strict`�������Ƿ���"strict"�ģ������������ΪNULL�������ҲΪNULL����
- `fn_retset`�������Ƿ񷵻�һ�����ϡ�
- `fn_stats`�����`track_functions`���ڴ�ֵ�����ռ�������ͳ����Ϣ��
- `fn_extra`���������ʹ�õĶ���ռ䡣
- `fn_mcxt`���洢`fn_extra`���ڴ������ġ�
- `fn_expr`���������õı��ʽ�����������û����ΪNULL��
- `fn_rettype`����������ֵ�Ķ����ʶ����OID����
- `fn_rettypemod`����������ֵ���������η���typmod���Ķ����ʶ����OID����
- `fnName`���������ƣ����ַ�����ʽ�洢��
- `fnLibPath`������C UDF���û����庯�����Ŀ�·�������߶���Java UDF���û����庯��������`package.class.method(args)`����ʽ��ʾ��
- `fn_fenced`�������Ƿ��Ǹ���ģ�fenced����
- `fn_languageId`�����������Ա�ʶ����
- `fn_volatile`�������Ĺ����ױ��ԣ�procvolatile����
- `vec_fn_addr`�����������ĵ�ַ��
- `vec_fn_cache`�����������Ļ��档
- `genericRuntime`��ͨ�ú�������ʱ��

����ṹ�����ڴ洢�ʹ��ݹ��ں�������Ϣ�����������ĵ�ַ������������ֵ�����Եȡ��������ݿ�ϵͳ�еĺ��������ִ�й����а�����Ҫ��ɫ��
```
### text*

typedef struct varlena text;

struct varlena {
    char vl_len_[4]; /* Do not touch this field directly! */
    char vl_dat[FLEXIBLE_ARRAY_MEMBER];
};

### logFdwPlanState
��δ��붨����һ����Ϊ `logFdwPlanState` �Ľṹ�����ͣ��ýṹ�������һϵ�����ڴ�����־�ⲿ��������Ϣ�ͺ���ָ�롣�����ǶԽṹ���Ա�ļ�Ҫ���ͣ�

1. `MemoryContext m_memcnxt;`
   ˽���ڴ��������

1. `MemoryContext m_pertup_memcnxt;`
   ÿ��Ԫ����ڴ������ġ�

1. һϵ�к���ָ�룬���ڴ���ͬ���͵���־���ݣ�������ȡ�ⲿ��ϵ��С����ȡ�ⲿ·������ȡ�ⲿ�ƻ�����ʼ�ⲿɨ�衢�����ⲿɨ�衢���¿�ʼ�ⲿɨ�衢�����ⲿɨ��ͽ����ⲿɨ�衣

1. `bool* m_isnull;`
   һ�����������飬���ڱ�ʾԪ����ÿ�������Ƿ�Ϊ NULL��

1. `Datum* m_values;`
   һ�� Datum ���͵����飬���ڴ洢����Ԫ������ݡ�

1. `union { pglogPlanState pg_log; gsprofilePlanState profile_log; } log_state;`
   ���ڴ洢��ͬ������־�ļƻ�״̬��

1. `text* dname;`
   ��־Ŀ¼����

1. `logdir_scanner dir_scan;`
   ��־Ŀ¼ɨ������

1. `log_file_info* logfile_info;`
   �ļ����;���·������־�ļ���Ϣ��

1. `text* filename;`
   ���ļ������ı����ݡ�

1. `void* m_vfd;`
   �ļ���������

1. `logfile_open m_open;`
   ����־�ļ��ĺ���ָ�롣

1. `logfile_read m_read;`
   ����־�ļ��ж�ȡ���ݵĺ���ָ�롣

1. `logfile_close m_close;`
   �ر���־�ļ��ĺ���ָ�롣

�������������Ա���������ڿ�����־���ݵļ��ء����˺ͻ���Ȳ�����

�˽ṹ�������ƺ�������ʵ����־�ⲿ��ļƻ�״̬�����а���������־��صĸ��ֲ����ͺ���������ִ�в�ѯ�ʹ�����־���ݡ�

-   ss.ps.targetlist   planstate

��δ��붨����һ����Ϊ`PlanState`�Ľṹ�塣���������³�Ա��

1. `NodeTag type`����ʾ�ṹ�����͵ı�ǡ�

1. `Plan* plan`��ָ�������`Plan`�ڵ��ָ�롣`Plan`�ڵ������˲�ѯ�ƻ���һ���֡�

1. `EState* state`��ָ��ִ���ڼ��״̬��ָ�롣����������ƻ��У������ڵ��״̬��ָ��ͬһ��`EState`��

1. `ExecProcNodeMtd ExecProcNode`��ָ�򷵻���һ��Ԫ��ĺ�����ָ�롣

1. `ExecProcNodeMtd ExecProcNodeReal`���������������һ����װ����������ָ��ʵ�ʵĺ�����

1. `Instrumentation* instrument`����ѡ������ʱͳ����Ϣ�����ڸýڵ㡣

1. `MemoryContext nodeContext`�����ڸýڵ���ڴ������ģ�Memory Context�������������ڴ�����ʱ���ڡ�

1. �����ṹ���ݣ����������мƻ����͵�ͨ�ýṹ���ݡ���Щ���ӵ�����״̬������������صļƻ����е�����ƽ�У�����`subPlan`�б��ڼƻ����в����ڣ���

   - `List* qual`����ʽ��AND���ӵ�����������
   - `struct PlanState* lefttree`������ƻ�������������
   - `struct PlanState* righttree`������ƻ�������������
   - `List* initPlan`����ʼ����`SubPlanState`�ڵ㣨����ر��ʽ�Ӳ�ѯ����
   - `List* subPlan`���ڸýڵ���ʽ�е�`SubPlanState`�ڵ㡣

1. ������������������ɨ������״̬��

   - `Bitmapset* chgParam`���Ѹ��Ĳ�����ID���ϡ�

1. ��������������ȫ�����ڵ����Ͷ���Ҫ����������ʱ״̬��

   - `TupleTableSlot* ps_ResultTupleSlot`�����ڴ洢���Ԫ��Ĳۡ�
   - `ExprContext* ps_ExprContext`���ڵ�ı��ʽ��ֵ�����ġ�
   - `ProjectionInfo* ps_ProjInfo`������ִ��Ԫ��ͶӰ����Ϣ��

1. �����ֶΡ�

   - `int64 ps_rownum`���洢��ǰ�кš�
   - `List* targetlist`���ڸýڵ�����Ŀ���б�
   - `HbktScanSlot hbktScanSlot`����ϣ��Ͱɨ��ۡ�
   - `bool vectorized`���Ƿ����ʸ��������
   - `bool earlyFreed`���ڵ��ڴ��Ƿ��ѱ��ͷš�
   - `uint8 stubType`���ڵ���ִ�����͡�
   - `bool recursive_reset`���ڵ��Ƿ������á�
   - `bool qual_is_inited`�����������Ƿ��ѳ�ʼ����
   - `bool do_not_reset_rownum`���Ƿ��ֹ�����кš�
   - `bool ps_vec_TupFromTlist`�����ڴ���Ŀ���б��еļ���ֵ������״̬��־��
   - `vectarget_func jitted_vectarget`��ָ��Ŀ���б���ʽ��LLVM IR����ָ�롣

1. ������ǰ�ƻ��ڵ��з��ֵ����⣬��Ҫ����������б�Ͳ�׼ȷ�Ĺ�������������ȥ�ء�

   - `List* plan_issues`���ƻ�������б�

��ˣ�����ṹ��`PlanState`���ڴ洢���ѯ�ƻ��ڵ���ص�״̬��Ϣ�͹��ܺ���������������ƻ�ִ����ص��ֶΣ��Լ�һЩ���ڹ����ڴ桢�ƻ����������������ֶΡ�

- FdwLogType

- typedef unsigned int Oid

- opexpr
  
  ��δ��붨����һ����ΪOpExpr�Ľṹ�����͡��ýṹ��������³�Ա��

xpr��һ��Expr���͵ĳ�Ա�����ڱ�ʾ���ʽ��ͨ�����ԡ�
opno����ʾ��������PG_OPERATOR��OID�������ʶ������
opfuncid����ʾ�������ײ㺯����PG_PROC��OID��
opresulttype����ʾ�������ֵ��PG_TYPE��OID��
opretset��һ������ֵ��������������ص��Ǽ��ϣ�set������Ϊtrue��
opcollid����ʾ���ֵ���������collation����OID��
inputcollid����ʾ������Ӧ��ʹ�õ���������OID��
args��һ��ָ��������Ĳ�����1����2�������б�
location��һ����������ʾ���Ƶ�λ�ã����λ��δ֪��Ϊ-1��
����ṹ���������ڱ�ʾ���������ʽ���������˲������������Ϣ�����������OID���ײ㺯����OID�����ֵ��OID���Ƿ񷵻ؼ��ϡ���������OID�Լ��������Ĳ����ȡ�

- var

��δ��붨����һ����Ϊ`Var`�Ľṹ�����͡��ýṹ��������³�Ա��

- `xpr`��һ��`Expr`���͵ĳ�Ա�����ڱ�ʾ���ʽ��ͨ�����ԡ�
- `varno`����ʾ�˱����ڷ�Χ���й�ϵ��������������`INNER_VAR`/`OUTER_VAR`/`INDEX_VAR`֮һ��
- `varattno`����ʾ�˱��������Ժţ����������������Ϊ0��
- `vartype`����ʾ�˱��������͵�`pg_type`��OID�������ʶ������
- `vartypmod`����ʾ�˱������������η���typmod����ֵ��
- `varcollid`����ʾ�˱������������collation����OID�����û�����������Ϊ`InvalidOid`��
- `varlevelsup`�������Ӳ�ѯ���������ⲿ��ϵ�Ĳ���������ͨ������Ϊ0������0��ʾN���ϡ�
- `varnoold`�����ڵ��Ե�`varno`��ԭʼֵ��
- `varoattno`�����ڵ��Ե�`varattno`��ԭʼֵ��
- `location`��һ����������ʾ���Ƶ�λ�ã����λ��δ֪��Ϊ-1��

����ṹ���������ڱ�ʾ�������ʽ���������˱����������Ϣ�������������ϵ�����������Ժš����͵�OID���������η�����������OID�ȡ���������һЩ�����Ӳ�ѯ���������ⲿ��ϵ���ֶ��Լ����ڵ��Ե�ԭʼֵ�ֶΡ�

- Node  NodeTag
ö������

- Const  �������˳����������Ϣ
��δ��붨����һ����Ϊ`Const`�Ľṹ�����͡��ýṹ��������³�Ա��

- `xpr`��һ��`Expr`���͵ĳ�Ա�����ڱ�ʾ���ʽ��ͨ�����ԡ�
- `consttype`����ʾ�������������͵�`pg_type`��OID�������ʶ������
- `consttypmod`����ʾ�������������η���typmod����ֵ��
- `constcollid`����ʾ�������������collation����OID�����û�����������Ϊ`InvalidOid`��
- `constlen`����ʾ�������������͵ĳ��ȣ�typlen����
- `constvalue`����ʾ������ֵ��`Datum`��
- `constisnull`��һ������ֵ����ʾ�����Ƿ�ΪNULL�����Ϊ`true`����`constvalue`��ֵδ���塣
- `constbyval`��һ������ֵ����ʾ�����������Ƿ�ֵ���ݡ����Ϊ`true`����������Ϣ���洢��`constvalue`�У����Ϊ`false`����`constvalue`����ָ����Ϣ��ָ�롣
- `location`��һ����������ʾ���Ƶ�λ�ã����λ��δ֪��Ϊ-1��
- `ismaxvalue`��һ������ֵ����ʾ�ó����Ƿ��ʾ���ֵ��maxValue����maxValue�����ڷ�����
- `cursor_data`��һ��`Cursor_Data`���͵ĳ�Ա�����ڴ洢�α����ݡ�

����ṹ���������ڱ�ʾ�������ʽ���������˳����������Ϣ�����������͵�OID���������η�����������OID�����ȡ�ֵ�ȡ�����������һЩ���ڱ�ʾNULLֵ����ֵ���ݵ����Ե��ֶΣ��Լ����ڴ洢�α����ݵ��ֶΡ�


-   List*  ListCell

struct ListCell {
    union {
        void* ptr_value;
        int int_value;
        Oid oid_value;
    } data;
    ListCell* next;
};
��δ��붨����һ����Ϊ`ListCell`�Ľṹ�����͡�

�ýṹ��������³�Ա��

- `data`��һ�������壨`union`�����ͣ����ڴ洢���ݡ�����������������Ա��

  - `ptr_value`��һ��`void*`���͵�ָ��ֵ��
  - `int_value`��һ��`int`���͵�����ֵ��
  - `oid_value`��һ��`Oid`���͵Ķ����ʶ��ֵ��
    ��������Ա����ͬһ���ڴ棬ֻ��ͬʱ�洢����һ����Ա��ֵ��

- `next`��һ��ָ����һ��`ListCell`�ṹ���ָ�롣

����ṹ������ͨ������ʵ���������ݽṹ��ÿ��`ListCell`�ڵ����һ�����ݳ�Ա`data`��������ָ�롢����������ʶ�������⣬ÿ���ڵ㻹����һ��ָ����һ���ڵ��ָ��`next`�������������б��������ӽڵ㡣


- log_file_info
  
typedef struct log_file_info {
    char* name;            /* log file name */
    time_t tm_create;      /* create time */
    time_t tm_last_modify; /* last modify time */
} log_file_info;

��δ��붨����һ����Ϊ`log_file_info`�Ľṹ�����͡�

�ýṹ��������³�Ա��

- `name`��һ��ָ��`char`���͵�ָ�룬��ʾ��־�ļ������ơ�
- `tm_create`��һ��`time_t`���͵ı�������ʾ��־�ļ��Ĵ���ʱ�䡣
- `tm_last_modify`��һ��`time_t`���͵ı�������ʾ��־�ļ�������޸�ʱ�䡣

����ṹ���������ڱ�ʾ��־�ļ�����Ϣ������������־�ļ��������Լ�����־�ļ���ص�ʱ����Ϣ����������ʱ�������޸�ʱ�䡣

- __MINGW_EXTENSION
`__MINGW_EXTENSION` ��һ����������չ��������ʹ��MinGW������ʱ����һЩ�ض������ͻ�����

`typedef __int64 __time64_t;` ��ʹ�� `__int64` Ϊ `__time64_t` ������һ��������`__int64` ��һ��64λ�������ͣ�ͨ�����ڱ�ʾʱ���������ʱ��ֵ��

��ˣ���δ��붨����һ����Ϊ `__time64_t` �����ͱ���������һ��64λ�������ͣ����ܱ����ڱ�ʾʱ����ص�ֵ�����������MinGW����������չ�������Ǳ�׼C���Ե�һ���֡�


- TimestampTz
* ʱ�����ʾ����ʱ�䡣
 *
 * �����ʾ����ʱ�䡣�����·ݣ����꣩�����ӣ�
 * ��Сʱ/����/��ֱ���Ϊ��ȵľ���ʱ����
 * δ֪��ֱ������ھ���ʱ��ʵ������
 *
 * ��ע�⣬openGauss ʹ�á�ʱ����������ʾ�н����䣬
 * �ɿ�ʼ�ͽ���ʱ����ɣ�������ʱ���� - Thomas 97/03/20
 *
 * ����������ʵ�֣�һ��ʹ�� int64 ֵ����λΪ
 * ΢�룬�Լ�ʹ������Ϊ��λ��˫����ֵ��
 *
 * TimeOffset �� fsec_t ����ʱ�����ķ������Ͷ���
 * ��������������ڲ�ͬ�����͡� ��Ҫ��ֵ��ʹ��fsec_t
 * �洢�ڴ����ϣ���Ϊ��������ʵ���еĴ�С��ͬ��
 * ���⣬fsec_t��������*С��*��;�������
 * �������Ҫ�洢��ֵ����Ϊ���롣

- typedef int64 pg_time_t;


- Value 

typedef struct Value {
    NodeTag type; /* tag appropriately (eg. T_String) */
    union ValUnion {
        long ival; /* machine integer */
        char* str; /* string */
        
    } val;
} Value;

��δ��붨����һ����Ϊ `Value` �Ľṹ�����͡�

�ýṹ��������³�Ա��

- `type`��һ�� `NodeTag` ���͵ĳ�Ա�����ڱ�ʶֵ�����͡�`NodeTag` ��һ��ö�����ͣ����Ը�����Ҫ����Ϊ��ͬ�����ͱ�ǩ������ `T_String` ��ʾ�ַ������͡�

- `val`��һ�������壨`union`�����ͣ����ڴ洢ֵ�ľ������ݡ�����������������Ա��

  - `ival`��һ�� `long` ���͵�����ֵ�����ڴ洢����������
  - `str`��һ��ָ�� `char` ���͵�ָ�룬���ڴ洢�ַ�����

����ṹ���������ڱ�ʾͨ�õ�ֵ���������������ַ�����������Ҫ���������� `type` ��Ա����ʶֵ�ľ������ͣ���ʹ�� `val` �������е���Ӧ��Ա���洢ֵ�����ݡ�

- typedef struct ld_scanner* logdir_scanner;
- struct ld_scanner {
    MemoryContext m_memcnxt;
    char* top_dir;           /* len < 1024, top directory */
    List* dir_list;          /* total directories to scan */
    List* dir_list_copy;     /* for rescan support */
    char* cur_dir;           /* len < 1024, sub-directory under top_dir */
    log_file_info* log_file; /* len < 1024, log file under cur_dir */
    List* log_file_list;     /* list of log files */
    dirname_match dir_cb;    /* callback for dir name */
    fname_match fname_cb;    /* callback for file name */
    int m_latest_files_num;  /* apply for each directory */
};
��δ��붨����һ����Ϊ `ld_scanner` �Ľṹ�����͡�

�ýṹ��������³�Ա��

- `m_memcnxt`��һ�� `MemoryContext` ���͵ĳ�Ա����ʾ�ڴ������ġ�

- `top_dir`��һ��ָ�� `char` ���͵�ָ�룬��ʾ����Ŀ¼��·�����ַ������Ȳ����� 1024��

- `dir_list`��һ������`List`�����͵�ָ�룬��ʾ��Ҫɨ�����Ŀ¼�б�

- `dir_list_copy`��һ������`List`�����͵�ָ�룬����֧������ɨ�裨rescan����

- `cur_dir`��һ��ָ�� `char` ���͵�ָ�룬��ʾ����Ŀ¼�µ���Ŀ¼·�����ַ������Ȳ����� 1024��

- `log_file`��һ��ָ�� `log_file_info` �ṹ�����͵�ָ�룬��ʾ��ǰĿ¼�µ���־�ļ����ַ������Ȳ����� 1024��

- `log_file_list`��һ������`List`�����͵�ָ�룬��ʾ��־�ļ����б�

- `dir_cb`��һ�� `dirname_match` ���͵Ļص�����������ƥ��Ŀ¼���ơ�

- `fname_cb`��һ�� `fname_match` ���͵Ļص�����������ƥ���ļ����ơ�

- `m_latest_files_num`��һ����������ʾÿ��Ŀ¼Ӧ�õ������ļ�����

����ṹ���������ڱ�ʾ��־ɨ�����������Ϣ�����������ڴ������ġ�����Ŀ¼��Ŀ¼�б���Ŀ¼����־�ļ����ص���������Ϣ������֧����־�ļ���ɨ���ƥ�������


- stat
#ifndef	NO_OLDNAMES
  struct stat {
    _dev_t st_dev;
    _ino_t st_ino;
    unsigned short st_mode;
    short st_nlink;
    short st_uid;
    short st_gid;
    _dev_t st_rdev;
    _off_t st_size;
    time_t st_atime;
    time_t st_mtime;
    time_t st_ctime;
  };
#endif /* NO_OLDNAMES */

��δ�����һ����������Ľṹ�嶨�壬�����ڲ����� `NO_OLDNAMES` ���������¶�����Ϊ `stat` �Ľṹ�����͡�

�ýṹ�������һЩ�ļ�״̬��Ϣ�ĳ�Ա��������

- `st_dev`����ʾ�豸��ʶ�������� `_dev_t`��
- `st_ino`����ʾ�ļ������ڵ������ `_ino_t`��
- `st_mode`����ʾ�ļ�����Ȩ�޺��ļ����͵��޷��Ŷ����͡�
- `st_nlink`����ʾӲ������Ŀ�Ķ����͡�
- `st_uid`����ʾ�ļ������ߵ��û���ʶ�Ķ����͡�
- `st_gid`����ʾ�ļ������ߵ����ʶ�Ķ����͡�
- `st_rdev`����ʾ�����ļ��豸��ʶ�������� `_dev_t`��
- `st_size`����ʾ�ļ���С������ `_off_t`��
- `st_atime`����ʾ�ļ�������ʱ��� `time_t` ���͡�
- `st_mtime`����ʾ�ļ�����޸�ʱ��� `time_t` ���͡�
- `st_ctime`����ʾ�ļ�״̬����ʱ��� `time_t` ���͡�

����ṹ�����Ͷ������ļ���״̬��Ϣ�������ļ����豸��ʶ���������ڵ㡢����Ȩ�ޡ�������Ŀ�������ߡ��ļ���С�Լ������ʡ��޸ĺ�״̬���ĵ�ʱ�䡣������������ָ���������� `NO_OLDNAMES`���򲻻��������ṹ��Ķ��塣

- dirent

��δ��붨����һ���ṹ�� `dirent`�������ڱ�ʾĿ¼�е�һ����Ŀ���ļ�����Ŀ¼������Ϣ���ýṹ��������³�Ա��

- `d_ino`�����ڱ�ʾ�ļ��� inode ���룬��������ṹ����ʼ��Ϊ�㣬��Ϊ�� Windows ϵͳ�в���ʹ�� inode ���롣
- `d_reclen`�����ڱ�ʾ��Ŀ¼��Ŀ�ļ�¼���ȣ���������ṹ����ʼ��Ϊ�㣬��Ϊ�� Windows ϵͳ�в�ʹ�ô��ֶΡ�
- `d_namlen`�����ڱ�ʾ`d_name` �ֶ����ļ����ĳ��ȡ�
- `d_name`�����ڴ洢�ļ������ַ����顣����� `d_name` ���鳤��Ϊ 260����ʾ�ļ�������󳤶�Ϊ 260 ���ַ���

��δ����������� Windows ϵͳ�е� dirent.h ͷ�ļ��������� Windows �·���Ŀ¼���ļ��������Ϣ������ṹ���ṩ��һ�ֱ�ʾĿ¼���ļ���Ϣ�ķ�ʽ��ʹ�ÿ��Ի�ȡ�ļ��������Ⱥ�����������ԡ�

- ld_scanner

��δ��붨����һ���ṹ�� `ld_scanner`���Լ�һ��ָ��ýṹ�����͵�ָ�� `logdir_scanner`��

`ld_scanner` �ṹ����������³�Ա��

- `m_memcnxt`�����ڹ����ڴ������ͷŵ��ڴ������ġ�
- `top_dir`��ָ�򶥲�Ŀ¼���ַ���ָ�룬��ʾҪɨ������ϲ�Ŀ¼��
- `dir_list`��ָ�� `List` ���͵�ָ�룬��ʾҪɨ�������Ŀ¼���б�
- `dir_list_copy`��ָ�� `List` ���͵�ָ�룬����֧������ɨ�裨rescan��������
- `cur_dir`��ָ��ǰ��Ŀ¼���ַ���ָ�룬��ʾ�� `top_dir` �µĵ�ǰ��Ŀ¼��
- `log_file`��ָ�� `log_file_info` ���͵�ָ�룬��ʾ�� `cur_dir` �µ���־�ļ���
- `log_file_list`��ָ�� `List` ���͵�ָ�룬��ʾ��־�ļ����б�
- `dir_cb`��ָ�� `dirname_match` ���͵Ļص�����ָ�룬���ڴ���Ŀ¼����ƥ�������
- `fname_cb`��ָ�� `fname_match` ���͵Ļص�����ָ�룬���ڴ����ļ�����ƥ�������
- `m_latest_files_num`�����ͱ�������ʾÿ��Ŀ¼Ӧ�õ������ļ�����

`logdir_scanner` ��һ��ָ�� `ld_scanner` �ṹ���ָ�����ͣ����ڷ�������úͲ����ýṹ�塣

�ýṹ���Ŀ�����ṩһ����־Ŀ¼ɨ������ʵ�֣����ڹ���Ͳ�����־Ŀ¼�����ļ��������Ϣ�������������ڴ洢Ŀ¼���ļ���Ϣ�ĳ�Ա���������ṩ�˻ص��������ڴ���Ŀ¼�����ļ�����ƥ�������

- addrinfo

��δ��붨����һ���ṹ�� `addrinfo`�����ڱ�ʾ�����ַ��Ϣ��

�ýṹ��������³�Ա��

- `ai_flags`�����ͱ�������ʾ��ַ��Ϣ�ı�־��
- `ai_family`�����ͱ�������ʾ��ַ���壨���� IPv4��IPv6�ȣ���
- `ai_socktype`�����ͱ�������ʾ�׽������ͣ����� SOCK_STREAM��SOCK_DGRAM�ȣ���
- `ai_protocol`�����ͱ�������ʾЭ�����͡�
- `ai_addrlen`��`size_t` ���͵ı�������ʾ��ַ���ȡ�
- `ai_canonname`��ָ��淶���������ַ���ָ�롣
- `ai_addr`��ָ�� `struct sockaddr` ���͵�ָ�룬��ʾ��ַ�ṹ��ָ�롣
- `ai_next`��ָ����һ�� `addrinfo` �ṹ���ָ�룬�����γ�����ṹ��

��δ����е�ע���ᵽ���� Win32 ƽ̨�ϣ��ṹ���Ա��˳�����׼�涨��˳��һ�£���Ϊ��ʹ IPv6 �����������������׼�涨��˳�򱣳�һ�¡�

`addrinfo` �ṹ���������������л�ȡ�ͱ�ʾ�����ͷ���ĵ�ַ��Ϣ�����ڸ�����������й㷺ʹ�ã����紴���׽��֡��󶨵�ַ������Զ�������ȡ���ͬ�Ĳ���ϵͳ���������ܶԸýṹ��Ķ���������ͬ��������ض�ƽ̨��ʹ��ʱ��Ҫע��ṹ���Ա��˳��Ͷ��塣

- typedef struct RelationData* Relation;
- RelationData �ṹ�����Ҫ�������� PostgreSQL ���ݿ�ϵͳ�б�ʾ�͹����ϵ�������������Ԫ���ݺ������Ϣ�����洢�˹�ϵ�������ʶ�������ü����������ļ������Ԫ�������������ʷ���ʵ�֡������б���������Ϣ����д�����м���ȫ���Ե�

- cmp_func
typedef int (*cmp_func)(const void*, const void*);

## gc_fdw
��ѯgc_fdw

List* get_str_targetlist(List* fdw_private)

������fdw_private��������ȡĿ���б��е��ַ���ֵ��������Щ�ַ���ֵ��װ��һ���µ��б��з��ء�

��aggת��remote sqlʱ��explain������ʹ��ForeignScan��targetlist���ַ����б�

- PlannerInfo*

Per-query information for planning/optimization
ÿ��ѯ��Ϣ�滮/�Ż�

�ýṹ���Ŀ�����ڲ�ѯ�滮���Ż������д洢�͸��ٸ�����Ϣ���Ա���������Ѳ�ѯ�ƻ�ʱʹ�á�

- ForeignScan �ṹ������ PostgreSQL �����ڱ�ʾʹ���ⲿ���ݰ�װ����Foreign Data Wrapper��FDW��ɨ���ⲿ��Ĳ�ѯִ�мƻ��ڵ�Ķ��塣

- ForeignScanState
ScanState ss������һ��ScanState���͵Ľṹ�壬��ΪForeignScanState�ĵ�һ���ֶΡ�ScanState�Ǳ�ʾɨ��״̬�Ľṹ�壬���ܰ���ɨ����ص���Ϣ��

ExprState* fdw_recheck_quals������һ��ָ��ExprState���͵�ָ�룬��ʾδ������ss.ps.qual�е�ԭʼ����������ExprState���ڱ�ʾ���ʽ��״̬��

struct FdwRoutine* fdwroutine������һ��ָ��struct FdwRoutine���͵�ָ�롣�ýṹ��ָ�����ⲿ���ݰ�װ����foreign-data wrapper���ĺ����ӿڣ�����ִ���ⲿ���ݷ��ʲ�����

void* fdw_state������һ��ָ��void���͵�ָ�룬���ڴ洢�ⲿ���ݰ�װ����״̬��Ϣ���ⲿ���ݰ�װ�������ڴ˴������Զ����״̬��

MemoryContext scanMcxt������һ��MemoryContext���͵ı�������ʾ����ɨ��״̬���ڴ������ģ�Memory Context����

ForeignOptions* options������һ��ָ��ForeignOptions���͵�ָ�룬��ʾ�ⲿɨ���ѡ�