1. ��fdw��β���ļ� �󲿷ֳ��ֵĽṹ����������غϲ���
2. ���ڷ�����־���ݵ��������ݰ�װ�� 
3. ʹ�����ݿ����ͨ��SQL��ѯ���Զ��ⲿ���ݽ��в�ѯ�����롢���º�ɾ�����������ⲿ���ݼ��ɵ����ݿ��У��Ա������ݷ��������Ӳ�ѯ��
4. ��Ҫ�����Զ���Ľṹ�����


- Datum
sizeof(Datum) == sizeof(void *) == 4 or 8
sizeof(char) == 1
sizeof(short) == 2

- FunctionCallInfo
����PostgreSQL�������ṩ�йص�ǰ�������������ĵ���Ϣ
typedef struct FunctionCallInfoData* FunctionCallInfo;

- FmgrInfo
���ڴ洢�ʹ��ݹ��ں�������Ϣ�����������ĵ�ַ������������ֵ������

- text

typedef struct varlena text;
varlena�����ڱ�ʾ�ɱ䳤�ȵ�����

- logFdwPlanState
�ýṹ�������һϵ�����ڴ�����־�ⲿ��������Ϣ�ͺ���ָ��

- planstate
�ڴ洢���ѯ�ƻ��ڵ���ص�״̬��Ϣ�͹��ܺ���

- FdwLogType ö�ٵ���������
enum FdwLogType { FLT_PG_LOG = 0, FLT_GS_PROFILE, FLT_UNKNOWN };
/*
FLT_PG_LOG����ʾPostgreSQL��־���͡�
FLT_GS_PROFILE����ʾGreenplum���ܷ�����־���͡�
FLT_UNKNOWN����ʾδ֪����־���͡�*/

- typedef unsigned int Oid

- opexpr ��ʾ���������ʽ
�����˲������������Ϣ�����������OID���ײ㺯����OID�����ֵ��OID���Ƿ񷵻ؼ��ϡ���������OID�Լ��������Ĳ����ȡ�

- var
��ʾ�������ʽ���������˱����������Ϣ�������������ϵ�����������Ժš����͵�OID���������η�����������OID

- Node  NodeTag
ö������

- Const  �������˳����������Ϣ ��ʾ�������ʽ

- List*  ListCell ʵ���������ݽṹ

- log_file_info ��ʾ��־�ļ�����Ϣ

- __MINGW_EXTENSION ��һ����������չ��������ʹ��MinGW������ʱ����һЩ�ض������ͻ�����

- `typedef __int64 __time64_t;` 
`__int64` ��һ��64λ�������ͣ�ͨ�����ڱ�ʾʱ���������ʱ��ֵ��
`__time64_t` �����ͱ���������һ��64λ�������ͣ������ڱ�ʾʱ����ص�ֵ��

- TimestampTz

- typedef int64 pg_time_t;

- Value ͨ�õ�ֵ���������������ַ���

- typedef struct ld_scanner* logdir_scanner;
��ʾ��־ɨ�����������Ϣ

- stat �����ڲ����� `NO_OLDNAMES` ���������¡� �������ļ���״̬��Ϣ

- dirent ��ʾĿ¼�е�һ����Ŀ���ļ�����Ŀ¼������Ϣ

- ld_scanner һ����־Ŀ¼ɨ������ʵ�֣����ڹ���Ͳ�����־Ŀ¼�����ļ��������Ϣ

- addrinfo ������һ���ṹ�� `addrinfo`�����ڱ�ʾ�����ַ��Ϣ��


- typedef struct RelationData* Relation;
��ʾ�͹����ϵ�������������Ԫ���ݺ������Ϣ�����洢�˹�ϵ�������ʶ�������ü����������ļ������Ԫ�������������ʷ���ʵ�֡������б���������Ϣ����д�����м���ȫ���Ե�

- cmp_func
typedef int (*cmp_func)(const void*, const void*);

## gc_fdw
 Query deparser for gc_fdw

List* get_str_targetlist(List* fdw_private)

������fdw_private��������ȡĿ���б��е��ַ���ֵ��������Щ�ַ���ֵ��װ��һ���µ��б��з��ء�


- PlannerInfo* �ýṹ���Ŀ�����ڲ�ѯ�滮���Ż������д洢�͸��ٸ�����Ϣ���Ա���������Ѳ�ѯ�ƻ�ʱʹ�á�

- RelOptInfo �ṹ������ PostgreSQL ���ݿ����ϵͳ�еĶ��塣�ýṹ���ʾ��ѯ�Ż��ڼ��ѯ�滮��ʹ�õĹ�ϵ��������ӣ�����Ϣ��

- ForeignScan �ṹ������ PostgreSQL �����ڱ�ʾʹ���ⲿ���ݰ�װ��ɨ���ⲿ��Ĳ�ѯִ�мƻ��ڵ�Ķ��塣

- ForeignScanState  ������ɨ���ⲿ����ʱ�洢��ص�״̬��Ϣ��ѡ��

- TupleTableSlot �洢Ԫ�����ݵĲ۽ṹ�壬������Ԫ��ĸ������Ժ�״̬��Ϣ���Լ����ڷ��ʺͲ���Ԫ��ķ����͹����ռ�

- VectorBatch������һ�����ڴ����������������ݵ��ࡣ�������˹������������ݵĸ��ֳ�Ա�����ͷ������������л��ͷ����л����ݡ�������ݡ��������ݵ�

- VecForeignScanStat ���ⲿ����Դ��ִ��������ɨ��������������˴���ɨ�����ݵ����ζ���������ζ����Լ���ɨ����ص������ĺ�״̬��Ϣ��

- RelationData �ṹ������ PostgreSQL �ж����һ�����ݽṹ������ PostgreSQL ���ݿ�ϵͳ�����ڱ�ʾ��ϵ������������Ļ������ݽṹ��

- gc_fdw_single ����ִ��FDWѡ���
- deparse_single ��ȡĿ���б��е��ַ���ֵ��Ȼ���װ��һ���µ��б��з���
- option_single ���Զ���Ľṹ������ת���ɶ�Ӧ����������

## oracle_fdw

��δ�����һ��Makefile�ļ������ڹ����Ͱ�װOracle FDW��Foreign Data Wrapper����չ��

all��Ĭ��Ŀ��Ϊoracle_fdw_target��������Oracle FDW��
install��Ĭ��Ŀ��Ϊinstall-data������װOracle FDW��
top_builddir��ָ���˶��㹹��Ŀ¼��
include $(top_builddir)/src/Makefile.global������ȫ��Makefile�ļ���
CODE_DIR��ָ����Oracle FDW�Ĵ���Ŀ¼��
oracle_fdw_targetĿ�꣺������prep_checkedĿ�ִ꣬����ORCFDW_HOMEĿ¼�µ�make����������Oracle FDW�����У�NO_PGXS=1��ʾ��ʹ��PGXS����ϵͳ��TOP_DIR=$(abs_top_srcdir)ָ���˶���Դ����Ŀ¼��
prep_checkedĿ�꣺���ORCFDW_HOMEĿ¼�Ƿ���ڣ���������������������Ϣ�����˳�1��������ڣ����ڵ�ǰĿ¼�´���һ����Ϊprep_checked���ļ�����ʾ�Ѿ�������
install-dataĿ�꣺������oracle_fdw_targetĿ�ִ꣬����ORCFDW_HOMEĿ¼�µ�make��������װOracle FDW��
uninstall��distclean��cleanĿ�꣺�ֱ�����ж�ء��������������Ŀ���ļ��Ϳ��ļ����Լ�ɾ��prep_checked�ļ���
�ڹ����Ͱ�װ�����У�ʹ����ORCFDW_HOME����������ָ��Oracle FDW��Ŀ¼��

## mysql_fdw

���ڹ����Ͱ�װMySQL FDW��Foreign Data Wrapper����չ��

all��Ĭ��Ŀ��Ϊmysql_fdw_target��������MySQL FDW��
install��Ĭ��Ŀ��Ϊinstall-data������װMySQL FDW��
top_builddir��ָ���˶��㹹��Ŀ¼��
include $(top_builddir)/src/Makefile.global������ȫ��Makefile�ļ���
CODE_DIR��ָ����MySQL FDW�Ĵ���Ŀ¼��
mysql_fdw_targetĿ�꣺������prep_checkedĿ�ִ꣬����MYFDW_HOMEĿ¼�µ�make����������MySQL FDW�����У�TOP_DIR=$(abs_top_srcdir)ָ���˶���Դ����Ŀ¼��
prep_checkedĿ�꣺���MYFDW_HOMEĿ¼�Ƿ���ڣ���������������������Ϣ�����˳�1��������ڣ����ڵ�ǰĿ¼�´���һ����Ϊprep_checked���ļ�����ʾ�Ѿ�������
install-dataĿ�꣺������mysql_fdw_targetĿ�ִ꣬����MYFDW_HOMEĿ¼�µ�make��������װMySQL FDW��
uninstall��distclean��cleanĿ�꣺�ֱ�����ж�ء��������������Ŀ���ļ��Ϳ��ļ����Լ�ɾ��prep_checked�ļ���
�ڹ����Ͱ�װ�����У�ʹ����MYFDW_HOME����������ָ��MySQL FDW��Ŀ¼��

## postgres_fdw
- conncacheentry ͨ������ConnCacheKey�ṹ�壬���Դ���һ�����ڻ���������Ϣ�ļ���key�����ü���������ʶ��ͼ����ض��ⲿ���������û�ӳ���������Ϣ���Ա������ӻ����н��п��ٲ��Һ����á�ʹ�������ļ�����������ӵ�Ч�ʺ����ܡ�