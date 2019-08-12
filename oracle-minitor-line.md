# ORACLE monitor query line #
1. 查看表空间名称及大小情况
```
    SELECT t.tablespace_name, round(SUM(bytes / (1024 * 1024)), 0) ts_size 
    FROM dba_tablespaces t, dba_data_files d 
    WHERE t.tablespace_name = d.tablespace_name 
    GROUP BY t.tablespace_name; 
```

2. 查看表空间物理文件的名称及大小情况
```
    SELECT tablespace_name, file_id, file_name, round(bytes / (1024 * 1024), 0) total_space 
    FROM dba_data_files 
    ORDER BY tablespace_name; 
```

3. 查看回滚段的名称和大小    
```
    SELECT segment_name, tablespace_name, r.status, (initial_extent / 1024) initialextent, 
    (next_extent / 1024) nextextent, max_extents, v.curext curextent 
    FROM dba_rollback_segs r, v$rollstat v 
    WHERE r.segment_id = v.usn(+) 
    ORDER BY segment_name; 
```

4. 查看控制文件信息
```
    记录了当前数据库的结构信息,同时也包含数据文件及日志文件的信息以及相关的状态,归档信息等等
    SELECT NAME FROM v$controlfile; 
```

5. 查看数据库库对象 
```
    SELECT owner, object_type, status, COUNT(*) count# 
    FROM all_objects 
    GROUP BY owner, object_type, status; 
```

6. 查看数据库版本
```
    SELECT version 
    FROM product_component_version 
    WHERE substr(product, 1, 6) = 'Oracle'; 
```

7. 查看当前等锁的所有会话
```
    SELECT * FROM DBA_WAITERS;
```

8. 查看死锁信息
```
    SELECT (
       SELECT username  FROM v$session 
       WHERE SID = a.SID) blocker, a.SID, 'is blocking', 
       (
             SELECT username 
             FROM v$session 
             WHERE SID = b.SID
        ) blockee, b.SID 
    FROM v$lock a, v$lock b 
    WHERE a.BLOCK = 1 AND b.request > 0 AND a.id1 = b.id1 AND a.id2 = b.id2; 
```

9. 杀死锁表进程
```
    ALTER SYSTEM KILL SESSION sessionid;
```


10. 查看当前连接的数据库地址：
```
    select sys_context('userenv'，'ip_address') from dual 
```
