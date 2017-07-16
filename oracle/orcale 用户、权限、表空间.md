# orcale 用户、权限、表空间

## 表空间
### 临时表空间
#### 临时表空间创建
```
create temporary tablespace new_temp
tempfile 'F:\DB\oracle\oradata\gs\new_temp1.dbf'
size 512m
autoextend on
next 256m maxsize 4096m
extent management local;
```

#### 删除临时表空间
```
drop tablespace NEW_TEMP including contents and datafiles cascade constraints;
```

#### 临时表空间查看
```

select tablespace_name,file_name,bytes/1024/1024 file_size,autoextensible from dba_temp_files;
--sys用户查看
select status,enabled, name, bytes/1024/1024 file_size from v_$tempfile;

SELECT temp_used.tablespace_name,
       total - used as "Free",
       total        as "Total",
       round(nvl(total - used, 0) * 100 / total, 3) "Free percent"
  FROM (SELECT tablespace_name, SUM(bytes_used) / 1024 / 1024 used
          FROM GV_$TEMP_SPACE_HEADER
         GROUP BY tablespace_name) temp_used,
       (SELECT tablespace_name, SUM(bytes) / 1024 / 1024 total
          FROM dba_temp_files
         GROUP BY tablespace_name) temp_total
 WHERE temp_used.tablespace_name = temp_total.tablespace_name;
```
[oracle 临时表空间的增删改查](http://www.blogjava.net/japper/archive/2012/06/28/381721.html)

---

### 数据表空间
#### 创建数据表空间
```
create tablespace newdata_tablespace
logging datafile 'F:\DB\oracle\oradata\gs\newdata_tablespace1.dbf'
size 512m autoextend on next 256m
maxsize 4096m
extent management local;
```

#### 追加数据表空间
```
alter tablespace newdata_tablespace
add datafile 'F:\DB\oracle\oradata\gs\newdata_tablespace2.dbf'
size 1024M autoextend on next 500M
maxsize 4096M
extent management local;
```

#### 扩大表空间
```
alter database
datafile 'F:\DB\oracle\oradata\gs\newdata_tablespace1.dbf'
autoextend on next 500M maxsize 8192M;
```

#### 删除数据表空间
```
--可以先将其 offline、再将磁盘上的数据文件一同删除
alter tablespace newdata_tablespace offline;
drop tablespace newdata_tablespace including contents and datafiles;
--如果其他表空间中的表有外键等约束关联到了本表空间中的表的字段，就要加上 CASCADE CONSTRAINTS
drop tablespace tablespace_name including contents and datafiles CASCADE CONSTRAINTS;
```

#### 查看表空间
```
select * from dba_tablespaces;
select * from dba_free_space;
select * from dba_data_files;

SELECT   a.tablespace_name,
         ROUND (a.total_size) "total_size(MB)",
         ROUND (a.total_size) - ROUND (b.free_size, 3) "used_size(MB)",
         ROUND (b.free_size, 3) "free_size(MB)",
         ROUND (b.free_size / total_size * 100, 2) || '%' free_rate
  FROM   (  SELECT   tablespace_name, SUM (bytes) / 1024 / 1024 total_size
              FROM   dba_data_files
          GROUP BY   tablespace_name) a,
         (  SELECT   tablespace_name, SUM (bytes) / 1024 / 1024 free_size
              FROM   dba_free_space
          GROUP BY   tablespace_name) b
 WHERE   a.tablespace_name = b.tablespace_name(+);
```
[oracle_区管理extent local/dictionary](http://blog.sina.com.cn/s/blog_8317516b0100xqsz.html)
---

## 用户&权限
#### 新建用户
```
create user new_user identified by new_user
default tablespace newdata_tablespace
temporary tablespace temp
quota unlimited on newdata_tablespace;
```
#### 分配权限
```
select * from dba_role_privs a where a.grantee='new_user';

grant connect,resource to new_user;
grant dba to new_user;

grant dba to new_user with admin option;
grant resource to new_user with admin option;
grant connect to new_user;
```
[with admin option和with grant option的区别](http://www.2cto.com/database/201311/255421.html)

**重点来了**
```
select * from dba_sys_privs where grantee='new_user';
```
> 在分配 resource 权限时也分配了 UNLIMITED TABLESPACE 权限
>
> 也就是 new_user 这个用户可以在其他表空间里随意建表,这个是我们不希望看到的。

**补救方法**
#### 更改用户的表空间限额
> 1、把 UNLIMITED TABLESPACE 权限关掉
>
> 2、使 new_user 在表空间 newdata_tablespace 中无限制
```
revoke unlimited tablespace from new_user ;
alter user new_user quota unlimited on newdata_tablespace;
```

```
--更改用户的表空间限额：
--全局：
grant unlimited tablespace to new_user;
--针对某个表空间：
alter user abc quota unlimited on test;
--回收：
revoke unlimited tablespace from new_user;
alter user abc quota 0 on test;

--在查看配额  
select tablespace_name,username,max_bytes from DBA_TS_QUOTAS where username='new_user';
```

> 在此需要注意两个概念：表空间不足和用户配额不足
> 这两着不是一个概念。表空间的大小是指实际的用户表空间的大小；配额大小是用户指定使用表空间的大小
> 二者的解决方法也不相同。配额问题的解决：alter user abc auota 2g on tablespace_name;表空间不足的话就是扩展>表空间或者增加数据文件了。

#### 删除用户

```
--删除用户及用户的数据
drop user user_name cascade;
```
