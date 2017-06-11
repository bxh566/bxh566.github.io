---
title: DB2 execution plan
date: 2017-05-31 05:56:48
tags: [note, db2]
---
## create explain table:
[explain table](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.1.0/com.ibm.db2.luw.sql.ref.doc/doc/r0008441.html)
* Call the SYSPROC.SYSINSTALLOBJECTS procedure:
```sql
db2 CONNECT TO database-name
db2 "CALL SYSPROC.SYSINSTALLOBJECTS('EXPLAIN', 'C', CAST (NULL AS VARCHAR(128)), CAST (NULL AS VARCHAR(128)))"
```
This call creates the explain tables under the SYSTOOLS schema. To create them under a different schema, specify a schema name as the last parameter in the call.

* Run the EXPLAIN.DDL DB2® command file:
```sql
db2 CONNECT TO database-name
db2 -tf EXPLAIN.DDL
```
This command file creates explain tables under the current schema.It is located at the DB2PATH\misc directory on Windows operating systems, and the INSTHOME/sqllib/misc directory on Linux and UNIX operating systems. DB2PATH is the location where you install your DB2 copy and INSTHOME is the instance home directory.



## use below script to generate execution plan
```bash
#!/bin/sh
DB=$1
USER=$2
PASS=$3
PREPAREFILE=$4
QUERYFILE=$5
OPTLEVEL=$6
OUTFILE=$DB_$QUERYFILE.fmt_$OPTLEVEL.txt
if [ $# -ne 6 ]
then
echo "Usage: $0 <DBNAME> <user> <password> <prepare_sql_file> <input_sql_file> <optimization level> " echo
exit 1
fi
db2 terminate;
db2 change isolation to UR;
db2 connect to ${DB} user ${USER} using ${PASS};
#Pre-processing: create and populate session table
db2 -tvf ${PREPAREFILE}
db2 SET CURRENT query OPTIMIZATION $OPTLEVEL;
db2 set current explain mode explain
db2 -tvf ${QUERYFILE}
db2exfmt -d ${DB} -u ${USER} ${PASS} -g tic -w -1 -o ${OUTFILE} -n % -s % -# 0 
db2 set current explain mode no
db2 terminate;
```

## use db2expln
```
db2expln -database <dbname> -u <user> <password> -t -g -f <inputsqlfile>
```


