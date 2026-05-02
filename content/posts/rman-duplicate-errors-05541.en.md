---
title: "RMAN-05541 & RMAN-05001 when duplicating database from a consistent (cold) RMAN backup"
date: 2021-06-15T10:00:00+02:00
categories: ["Oracle Database", "Migrated"]
tags: ["Oracle", "RMAN", "Duplicate", "oldBlog"]
---

### RMAN-05541: no archived logs found in target database.

**Problem:** 
You are getting RMAN-05541 error when duplicating database from a consistent (cold) RMAN backup.
```text
RMAN-00571: ===========================================================
RMAN-00569: =============== ERROR MESSAGE STACK FOLLOWS ===============
RMAN-00571: ===========================================================
RMAN-03002: failure of Duplicate Db command at 04/16/2021 12:57:12
RMAN-05501: aborting duplication of target database
RMAN-05541: no archived logs found in target database
```

**Solution:** 
The error should be resolved by using NOREDO clause for your duplicate command.

```sql
DUPLICATE DATABASE TO 'TEST02' BACKUP LOCATION '/mnt/export/oracle/orabackup/full/' NOREDO;
```
---

### RMAN-05501: aborting duplication of target database (Auxiliary file name /.../.../oracle/.../oradata/perfstat01.dbf conflicts with a file used by the target database).

**Problem:** 
You are getting RMAN-05001 error when duplicating database from a consistent (cold) RMAN backup.
```text
RMAN-00571: ===========================================================
RMAN-00569: =============== ERROR MESSAGE STACK FOLLOWS ===============
RMAN-00571: ===========================================================
RMAN-03002: failure of Duplicate Db command at 04/16/2021 13:12:33
RMAN-05501: aborting duplication of target database
RMAN-05001: auxiliary file name /mnt/export/oracle/oradata/perfstat01.dbf conflicts with a file used by the target database
```

**Solution:** 
The error should be resolved by using NOFILENAMECHECK clause for your duplicate command.

```sql
DUPLICATE DATABASE TO 'TEST02' BACKUP LOCATION '/mnt/export/oracle/orabackup/full/' NOFILENAMECHECK;
```

> 💡 Tip: Both can be used in the same DUPLICATE command in case we are duplicating non-OMF database from a cold RMAN backup as follows:
> 
> ```sql
> DUPLICATE DATABASE TO 'TEST02' BACKUP LOCATION '/mnt/export/oracle/orabackup/full/' NOREDO NOFILENAMECHECK;
> ```

