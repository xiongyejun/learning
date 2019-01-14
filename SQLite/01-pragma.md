# pragma

 [ԭ�ĵ�ַ](https://iihero.iteye.com/blog/1189633)
 
PRAGMA�����SQLITE���ݵ�SQL��չ���������е����ԣ���Ҫ�����޸�SQLITE����������ݲ�ѯ�Ĳ�������������SELECT��INSERT�����һ������ʽ���������󣬵�Ҳ�м�����Ҫ�Ĳ�ͬ�� 
1. �ض���PRAGMA�����ܱ����ߣ��µ�PRAGMA���������µİ汾����ӡ���ˣ���������޷���֤�� 
2. δ֪��PRAGMA������д�����Ϣ���֣���ֻ�Ǽ򵥵ĺ��ԡ� 
3. ��ЩPRAGMAֻ��SQL�ı���׶������ã�������ִ�н׶Ρ�������ζ�����ʹ��C���ԣ�sqlite3_prepare(), sqlite3_step(), sqlite3_finalize()�⼸��API��pragma�������ֻ��prepare()�ĵ��������У��������ں�����API����ִ�С����ߣ�pragma������sqlite3_step()ִ�е�ʱ�����С��������ĸ��׶�ִ�У�ȡ����pragma�ӱ����Լ����ĸ�sqlite��release�汾�� 
4. pragma������sqlite���еģ������ϲ��������������ݿⱣ�ּ��ݡ� 

PRAGMA������﷨��ʽ����ͼ�� 

![](images/01-01.jpg)

![](images/01-02.jpg)


�����Բ�������������ֻ��һ��������������������ǵȺŸ�ֵ��Ҳ����������������������Ч��һ�����ܶ�����£�����ֵ�ǲ����ͣ�ֵΪ(1,yes,true ��on)����(0, no, false, off) 

�ؼ��ֲ���������ʹ��������������e.g. 'yes' [FALSE]����Щpragma�����ʹ���ַ�����Ϊ������"0"��"no"��ʾ��ͬ�ĺ��塣����ѯĳ���õ�ֵʱ���ܶ�����·��ص�����ֵ�������ǹؼ��֡� 

pragma��֮ǰ������ѡ�����ݿ�����֡����ݿ����Ǳ�"attach"���������ϵ����ݿ����֣�������"main", "temp"����ʾ�����ݿ����ʱ���ݿ⡣�����ѡ�����ݿ�������ȥ����Ĭ��Ϊ"main"���ݿ⡣����Щpragma��������ݿ���û�����壬��򵥵ĺ��Ե��� 

�������ǿ���sqlite������Щ���õ�pragma��� 

    auto_vacuum 
    automatic_index 
    cache_size 
    case_sensitive_like 
    checkpoint_fullfsync 
    collation_list 
    compile_options 
    count_changes1 
    database_list 
    default_cache_size1 
    empty_result_callbacks1 
    encoding 
    foreign_key_list 
    foreign_keys 
    freelist_count 
    full_column_names1 
    fullfsync 
    ignore_check_constraints 
    incremental_vacuum 
    index_info 
    index_list 
    integrity_check 
    journal_mode 
    journal_size_limit 
    legacy_file_format 
    locking_mode 
    max_page_count 
    page_count 
    page_size 
    parser_trace2 
    quick_check 
    read_uncommitted 
    recursive_triggers 
    reverse_unordered_selects 
    schema_version 
    secure_delete 
    short_column_names1 
    synchronous 
    table_info 
    temp_store 
    temp_store_directory1 
    user_version 
    vdbe_listing2 
    vdbe_trace2 
    wal_autocheckpoint 
    wal_checkpoint 
    writable_schema 
������м����������ϱ�Ϊ1�ģ��ƺ��Ѿ���obsoleted���ˡ���Ϊ2�ģ�ֻ������debug,����sqlite��Ԥ�����SQLITE_DEBUG��build�����������á� 

���濴����Щ����ľ����÷��� 

## 1. PRAGMA auto_vacuum; 
  
    PRAGMA auto_vacuum = 0 �� NONE | 1 �� FULL | 2 �� INCREMENTAL; 

���0��NONE��ʾ�ĺ�����ͬ�� 

ȱʡֵΪ0, ��ʾ����auto vacuum. 

����SQLITE_DEFAULT_AUTOVACUUM���ڱ����ʱ�����ˡ�����ɾ����ʱ�����ݿ��С����ı䡣û�õ����ݿ��ļ�ҳ��ᱻ��ӵ�freelist��ͷ�����ڽ������á���ʱ��ʹ��VACUUM��������ؽ��������ݿ⣬�Ի������õĴ��̿ռ䡣 

- ֵΪ1ʱ�����е�freelistҳ�ᱻ�ƶ����ļ�ĩβ��ÿ�������ύ��ʱ���ļ��ᱻ�ض̡�ע�⣬�Զ�vacuumֻ�Ǵ��ļ��ǽض�freelistҳ����û�н�����Ƭ�����Ȳ�����Ҳ����˵����û��VACUUM�������ó��ס���ʵ�ϣ��Զ�vacuum������Ƭ���ࡣ 

ֻ�������ݿ�洢ĳЩ������Ϣ��ʱ��������ÿ�����ݿ�ҳ��������������ҳ���Զ�vacuum���õ��ϡ���������û�д����κα����������á���һ�����Ѿ�������֮���ǲ������ú�ͣ��auto-vacuum�ġ� 

- ֵΪ2ʱ����ʾ����vacuum����ζ�Ų�������ÿ���ύ�����ʱ���Զ�vacuum����Ҫ����һ��������incremental_vacuum���������auto-vacuum�� 

 ���ݿ������1��2����vacuumģʽ�½����л������ǲ��ܴ�none��full��incremental���л���Ҫ���л���Ҫô���ݿ���ȫ�µ����ݿ⣨û���κα��������ߵ�������vacuum�����Ժ󡣸ı��Զ�vacuumģʽ������ִ��auto_vacuum��������µ�ģʽ��Ȼ�����VACUUM���������ݿ⡣ 

����������auto_vacuum��䷵�ص�ǰ��auto_vacuumģʽֵ�� 

## 2. PRAGMA automatic_index; 

    PRAGMA automatic_index = boolean; 
   ��ѯ�����û�������Զ������Ĺ��ܡ�ȱʡֵΪtrue (1). 

## 3. PRAGMA cache_size; 

    PRAGMA cache_size = <number of pages>; 
   
   ��ѯ�����޸Ĵ򿪵����ݿ��ڴ���ͷ�����ɵ��������ݿ�ҳ����ȱʡֵ��2000.�������趨ֻ��ı䵱ǰ�Ự�е�cache size�������ݿ����´򿪣��ֻ�ָ�Ĭ��ֵ�������ʹ��default_cache_size���趨���лỰ�е�cache size 

## 4. PRAGMA case_sensitive_like=boolean; 
  
   Ĭ����Ϊ�Ǻ���ascii�ַ��Ĵ�Сд��'a' LIKE 'A'����true. ������case_sensitive_likeʱ������Ĭ�ϵ�like��Ϊ����������ʱ���ͻ����ִ�Сд�� 

## 5. PRAGMA checkpoint_fullfsync 
    PRAGMA checkpoint_fullfsync=boolean; 

��ѯ������fullfsync�ı�־ֵ����������˸�ֵ����F_FULLFSYNCͬ����������checkpoint����ʱ���ã�Ĭ��ֵ��off��ֻ��Mac OS-X����ϵͳ֧��F_FULLFSYNC�����⣬����趨��fullfsyncֵ����ôF_FULLFSYNCͬ��������������sync������ʹ�ã�Ҳcheckpoint_fullfsync��־��ȫ�޹ء� 

## 6. PRAGMA collation_list; 
   
   ���ص�ǰ���ݿ����Ӷ������������˳�� 

## 7. PRAGMA compile_options; 

���Ҫ�ޣ����ر���SQLITEʱʹ�õ�����Ԥ����ꡣ��Ȼ����"SQLITE_"��ͷ��ǰ׺�ᱻ���ԡ�ʵ��������ͨ������sqlite3_compileoption_get()�������صġ� 

## 8. PRAGMA count_changes; 

    PRAGMA count_changes=boolean; 
  
   �������Ѿ�ͣ��. ֻ��Ϊ�˱��ֺ������. ��������ô�ֵ��INSERT, UPDATE, DELETE��䲻�᷵�ض����иı�����ݡ� 
   ��ʵ�ϣ�sqlite3_changes()���Ի�ȡ�ı�������� 

## 9. PRAGMA database_list; 

 ���ص�ǰ���ݿ����ӹ��������ݿ��б�. 

## 10. PRAGMA default_cache_size; 
    
    PRAGMA default_cache_size = Number-of-pages; 

����ȱʡ��cache sie, ����ҳΪ��λ�����ҵ��ǣ�������Ҳ���������� 

## 11. PRAGMA empty_result_callbacks; 
    
    PRAGMA empty_result_callbacks = boolean; 

 ������������á�������ñ�־ֵ�����sqlite3_exec()�ṩ�Ļص�����������0��������ݣ������������� 

## 12. PRAGMA encoding; 
    
        PRAGMA encoding = "UTF-8"; 
        PRAGMA encoding = "UTF-16"; 
        PRAGMA encoding = "UTF-16le"; 
        PRAGMA encoding = "UTF-16be"; 

 ȱʡֵ��utf-8�����ʹ��attach������Ҫ��ʹ����main���ݿ���ͬ���ַ������룬����µ����ݿ������main��ͬ�����ʧ�ܡ� 

## 13. PRAGMA foreign_key_list(table-name); 

��������б� 

## 14. PRAGMA foreign_keys; 
   
    PRAGMA foreign_keys = boolean; 

��ѯ���û�������������������, �������ֻ����BEGIN����SAVEPOINT����PENDING״̬ʱ���ò���Ч�� 

�ı�����û�Ӱ�������Ѿ�׼���õ�SQL����ִ�С� 

��3.6.19��ʼ��Ĭ�ϵ�FKǿ��������OFF��Ҳ����˵������ǿ����������� 

## 15. PRAGMA freelist_count; 
    
    �������ݿ��ļ���δʹ��ҳ����Ŀ 

## 16. PRAGMA full_column_names; 

    PRAGMA full_column_names = boolean; 
    deprecated. 
    
1. �����AS�Ӿ䣬�����ͻ���AS��ı��� 
2. ������ֻ����ͨ�ı��ʽ��������Դ�������������ñ��ʽ���ı� 
3. ���ʹ����short_column_names����ΪON�������Դ�����������Ҳ�������ǰ׺ 
4. ����������ض���ΪOFF������õ�2������ 
5. �������ѧ��Դ��Դ�е���ϣ�TABLE.COLUMN 

## 17. PRAGMA fullfsync; 

    PRAGMA fullfsync = boolean; 
    
ȱʡֵΪOFF��Ҳֻ��MAC os֧��F_FULLFSYNC 

## 18. PRAGMA ignore_check_constraints = boolean; 
 
    �Ƿ�ǿ��checkԼ����ȱʡֵΪoff 

## 19. PRAGMA incremental_vacuum(N); 

Nҳ��freelist���Ƴ��������趨�˲�����ÿ�νض���ͬ��ҳ�����������������auto_vacuum=incrementalģʽ�²���Ч�����freelist�е�ҳ������N������NС��1������N����ȫ���ԣ���ô����freelist�ᱻ����� 

## 20. PRAGMA index_info(index-name); 
  
��ȡ������index��Ϣ�� 

## 21. PRAGMA index_list(table-name); 
   
��ȡ��Ŀ�������������ĵ������Ϣ 

## 22. PRAGMA integrity_check; 
    
    PRAGMA integrity_check(integer); 
    
ִ�����������ȫ�Լ�飬��鿴����ļ�¼����ʧ��ҳ���ٻ��������ȡ� 

## 23. 
   
    PRAGMA journal_mode; 
    PRAGMA database.journal_mode; 
    PRAGMA journal_mode = DELETE | TRUNCATE | PERSIST | MEMORY | WAL | OFF 
    PRAGMA database.journal_mode = DELETE | TRUNCATE | PERSIST | MEMORY | WAL | OFF 

�����������ݿ��journal_mode. DELETE��ȱʡ����Ϊ���ڴ�ģʽ�£�ÿ��������ֹ��ʱ��journal�ļ��ᱻɾ�������ᵼ�������ύ�� 

TRUNCATEģʽ��ͨ�����ع�journal�ض̳�0��������ɾ�����������������£���Ҫ��DELETEģʽ�ٶȿ�(��Ϊ����ɾ���ļ��� 

PERSISTģʽ��ÿ���������ʱ������ɾ��rollback journal����ֻ����journal��ͷ�����0����������ֹ������ݿ�������rollback. ��ģʽ��ĳЩƽ̨�£���һ���Ż����ر���ɾ������truncateһ���ļ��ȸ����ļ��ĵ�һ����۸ߵ�ʱ�� 

MEMORYģʽ��ֻ��rollback��־�洢��RAM�У���ʡ�˴���I/O���������Ĵ������ȶ��Ժ��������ϵ���ʧ������м�crash���ˣ����ݿ��п����𻵡� 
WALģʽ��Ҳ����write-ahead��logȡ��rollback journal����ģʽ�ǳ־û��ģ���������Ϊ���ӣ������´����ݿ��Ժ���Ȼ��Ч����ģʽֻ��3.7.0�Ժ����Ч�� 
(����ʵ�飬���֣��������������ļ���.shm��.wal) 

OFFģʽ��������û������֧���ˡ��� 

����Ҫע����ǣ�����memory���ݿ⣬ֻ������ģʽ: 
MEMORY����OFF�����ң���ǰ����л�Ծ������������ı�����ģʽ�� 

## 24. PRAGMA journal_size_limit 
    
    PRAGMA journal_size_limit = N ; 

�������ʱ������"exclusive mode(PRAGMA locking_mode=exclusive)����(PRAGMA journal_mode=persist), �ύ�����Ժ�journal�ļ�����Ȼ���ļ�ϵϵͳ���С�����ܻ������Ч�ʣ�����Ҳ����˿ռ䡣һ�����������VACUUM)����ķѴ����Ĵ��̿ռ䡣 

�����û�����journal�ļ��Ĵ�С��Ĭ��ֵ�ǣ�1�� 

## 25. PRAGMA legacy_file_format; 

    PRAGMA legacy_file_format = boolean; 

�����ֵΪON��������3.0.0�ļ���ʽ�����Ϊoff, ���������µ��ļ���ʽ�����ܵ��¾ɰ汾��sqlite�޷��򿪸��ļ��� 

��һ�����ļ���ʽ��sqlite3���ݿ��ʱ����ֵΪoff.����Ĭ��ֵ����on. 

## 26. PRAGMA locking_mode; 
    
    PRAGMA locking_mode = NORMAL | EXCLUSIVE 

ȱʡֵ��NORMAL. ���ݿ�������ÿһ������д�����յ��ʱ��ŵ��ļ����������EXCLUSIVEģʽ��������Զ�����ͷ��ļ������ڴ�ģʽ�£���һ��ִ�ж�����ʱ�����ȡ�����й���������һ��д�����ȡ�������������� 

�ͷ��������������ر����ݿ����ӣ����߽���ģʽ�Ļ�ΪNORMALʱ���ٴη������ݿ��ļ�������д���Ż�ŵ����򵥵�����ΪNORMAL�ǲ����ģ�ֻ�е��´��ٷ���ʱ�Ż��ͷ��������� 

�������������ɣ�ȥ������ģʽΪEXCLUSIVE 

1. Ӧ�ó�����Ҫ��ֹ�������̷������ݿ��ļ� 
2. �ļ�ϵͳ��ϵͳ�������������ˣ�����Щ�������½� 
3. WAL��־ģʽ������EXCLUSIVEģʽ��ʹ�ã�������Ҫ�õ������ڴ� 

��ָ�����ݿ���ʱ��ֻ��Ŀ�����ݿ���Ч���磺 
    
    PRAGMA main.locking_mode=EXCLUSIVE;  

��ָ�����ݿ���ʱ��������д򿪵����ݿ���Ч��temp����memory���ݿ�����ʹ��exclusive��ģʽ�� 

��һ�ν���WAL��־ģʽʱ����ģʽʹ�õ���exclusive�����Ժ���ģʽҲ���ܸı䣬ֱ���˳�WAL��־ģʽ�������ģʽ��ʼʱʹ�õ���NORMAL����һ�ν���WAL����ʱ��ģʽ���Ըı䣬���Ҳ���Ҫ�˳�WALģʽ�� 

## 27. PRAGMA max_page_count; 
    
    PRAGMA max_page_count = N; 

��ѯ�����������ݿ��ļ������ҳ�� 

## 28. PRAGMA page_count; 

�������ݿ��ļ���ҳ�� 

## 29. PRAGMA page_size; 
   
    PRAGMA page_size = bytes; 

��ѯ�����������ݿ��ļ���ҳ��С, ������2�ĳ˷������ҽ���512��65536֮�䡣 

�������ݿ�ʱ�������һ��ȱʡ�Ĵ�С��page_size����������ı�ҳ��С��������ݿ��ǿյĻ�������˵��û�д����κα������£������ָ�����´�С����������VACUUM����֮�䣬ͬʱ���ݿⲻ����WAL��־ģʽ�£���ôVACUUM����Ὣҳ��С�������µĴ�С����ʱӦ��û�����´���������ƣ� 
    SQLITE_DEFAULT_PAGE_SIZE ȱʡֵ��1024������ȱʡҳ��С��8192. windows�£���ʱ�����ȱʡҳ��С����1024��ȡ����GetDiskFreeSpace()����ȡ��ʵ������������С�� 

## 30. PRAGMA parser_trace = boolean; 

����DEBUG��ʱ�� 

## 31. PRAGMA quick_check; 

    PRAGMA quick_check(integer) 

��integrity_check���񣬵�����ȥ�˶����������������ƥ���У�顣 

## 32. PRAGMA read_uncommitted; 
    
    PRAGMA read_uncommitted = boolean; 

��δ�ύ���ء�ȱʡ��������뼶�ǣ��ɴ��л����κν��̻��̶߳��������ö�δ�ύ���뼶�����ǣ�SERIALIZABLE�Ա�ʹ�ã����˹���ĳҳ�ͱ�ģʽ�Ļ������Щ���ӡ� 

## 33. PRAGMA recursive_triggers; 
    
    PRAGMA recursive_triggers = boolean; 

��Ӱ�����е����ִ�С�3.6.18��ǰ����������ǲ�֧�ֵġ�ȱʡֵ��off. 

## 34. PRAGMA reverse_unordered_selects; 
    
    PRAGMA reverse_unordered_selects = boolean; 

�������˿���ʱ������order by��select��䣬������෴˳��Ľ���� 

## 35. PRAGMA schema_version; 

    PRAGMA schema_version = integer ; 
    PRAGMA user_version; 
    PRAGMA user_version = integer ; 

schema��user version�������ݿ��ļ�ͷ40��60�ֽڴ���32λ��������˱�ʾ���� 

schema�汾��sqlite�ڲ�ά������schema�ı�ʱ���ͻ����Ӹ�ֵ����ʽ�ı��ֵ�ǳ�Σ�ա� 

user�汾���Ա�Ӧ�ó���ʹ�á� 

## 36. PRAGMA secure_delete; 
    
    PRAGMA database.secure_delete; 
    PRAGMA secure_delete = boolean 
    PRAGMA database.secure_delete = boolean 

��ΪONʱ��ɾ�������ݻ���0�����ǡ�ȱʡֵ�ɺ�SQLITE_SECURE_DELETE �������Ǿ���OFF�ˡ� 

## 37. PRAGMA short_column_names; 

    PRAGMA short_column_names = boolean; 
    deprecated. 

## 38. PRAGMA synchronous; 
    
    PRAGMA synchronous = 0 | OFF | 1 | NORMAL | 2 | FULL; 

��ѯ����sync��־ֵ��ȱʡֵ��FULL. 

## 39. PRAGMA table_info(table-name); 

���ر�Ļ�����Ϣ 

## 40. PRAGMA temp_store; 
    
    PRAGMA temp_store = 0 | DEFAULT | 1 | FILE | 2 | MEMORY; 

    ��ѯ������temp_store����ֵ�� 

<table>
<tr>
	<td>SQLITE_TEMP_STORE</td>
	<td>PRAGMA temp_store</td>
	<td>Storage used forTEMP tables</td>
</tr>
<tr>
	<td>0</td>
	<td>any</td>
	<td>file </td>
</tr>
<tr>
	<td>1</td>
	<td>0</td>
	<td>file </td>
</tr>
<tr>
	<td>1</td>
	<td>1</td>
	<td>file </td>
</tr>
<tr>
	<td>1</td>
	<td>2</td>
	<td>memory </td>
</tr>
<tr>
	<td>2</td>
	<td>0</td>
	<td>memory </td>
</tr>
<tr>
	<td>2</td>
	<td>1</td>
	<td>file </td>
</tr>
<tr>
	<td>2</td>
	<td>2</td>
	<td>memory </td>
</tr>
<tr>
	<td>3</td>
	<td>any</td>
	<td>memory </td>
</tr>
</table>

## 40. PRAGMA temp_store_directory; 
    
    PRAGMA temp_store_directory = 'directory-name'; 

���û�ı�temp_store��Ŀ¼λ��. deprecated. 

## 41. PRAGMA vdbe_listing = boolean; 
    
����DEBUG 

## 42. PRAGMA vdbe_trace = boolean; 

����DEBUG 

## 43. PRAGMA wal_autocheckpoint; 
    
    PRAGMA wal_autocheckpoint=N; 

����WAL�Զ�����ļ������ҳΪ��λ��, ȱʡֵ��1000�� 

## 44. PRAGMA database.wal_checkpoint; 
    
    PRAGMA database.wal_checkpoint(PASSIVE); 
    PRAGMA database.wal_checkpoint(FULL); 
    PRAGMA database.wal_checkpoint(RESTART); 

## 45. PRAGMA writable_schema = boolean; 

����ΪONʱ��SQLITE_MASTER�����ִ��CUD��������������Σ��!! 