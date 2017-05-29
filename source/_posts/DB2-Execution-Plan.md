---
title: DB2 Execution Plan
date: 2017-05-29 19:06:34
tags: [db2, note]
---
## DB2 execution plan for stored procedure:
[collecting explain data](http://www-01.ibm.com/support/docview.wss?uid=swg21279292)

### Configuration
```
db2 "connect to dbname user username using password"
db2 -tvf ~/sqllib/misc/EXPLAIN.DDL
```

### Obtain the explain plan when creating the stored procedure:
1. Turn on explain
    * Dynamically within the scope of the current session by:
      ```
      db2 "call SET_ROUTINE_OPTS('EXPLAIN ALL')" 
      ```
    * globally at the instance level if the procudure is not being called within the current session
      ```
      db2set DB2_SQLROUTINE_PREPOPTS="EXPLAIN ALL"
      db2 terminate 
      db2stop 
      db2start 
      ```
1. Run the create statement for the SQL procedure that requires explaining. If one of the same name already exists, you may need to drop the procedure first or create a similar procedure under a different schema or name. 

1. The resulting explain data will be stored in the explain tables and can be extracted with a command such as the following:
   ```
   db2exfmt -d dbname -u username password -e username -g TIC -w -1 -n % -s % -# 0 -o filename.out
   ```

1. Disable explain by either issuing either one of these commands depending on how it was enabled. 
    * If it was turned on for the current session
      ```
      db2 "CALL SYSPROC.SET_ROUTINE_OPTS('')"
      ```
    * If it was turned on globally
      ```
      db2set DB2_SQLROUTINE_PREPOPTS=
      db2 terminate 
      db2stop 
      db2start
      ```

## SQLs
### get package name
```sql
select
r.routineschema,r.routinename,rd.bname as packagename
from syscat.routines r,syscat.routinedep rd
where r.specificname=rd.specificname
and r.routineschema=rd.routineschema
and rd.btype='K'
and r.routineschema = upper('schema')
and r.routinename = upper('spname')
```

### get section of sp
```sql
--package name from above
select sectno, cast(text as varchar(32000)) from syscat.statements where pkgschema='schema' and pkgname='P0000000001'
```
