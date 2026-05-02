---
title: "RMAN-05541 & RMAN-05001 al duplicar una base de datos desde un backup consistente (en frío) de RMAN"
date: 2021-06-15T10:00:00+02:00
categories: ["Oracle Database", "Migrated"]
tags: ["Oracle", "RMAN", "Duplicate", "oldBlog"]
---

### RMAN-05541: no archived logs found in target database.

**Problema:** 
Estás obteniendo el error RMAN-05541 al duplicar una base de datos desde un backup consistente (en frío) de RMAN.
```text
RMAN-00571: ===========================================================
RMAN-00569: =============== ERROR MESSAGE STACK FOLLOWS ===============
RMAN-00571: ===========================================================
RMAN-03002: failure of Duplicate Db command at 04/16/2021 12:57:12
RMAN-05501: aborting duplication of target database
RMAN-05541: no archived logs found in target database
```

**Solución:** 
El error debería resolverse usando la cláusula NOREDO en tu comando de duplicación.
```sql
DUPLICATE DATABASE TO 'TEST02' BACKUP LOCATION '/mnt/export/oracle/orabackup/full/' NOREDO;
```

---

### RMAN-05501: aborting duplication of target database (Auxiliary file name /.../.../oracle/.../oradata/perfstat01.dbf conflicts with a file used by the target database).

**Problema:** 
Estás obteniendo el error RMAN-05001 al duplicar una base de datos desde un backup consistente (en frío) de RMAN.
```text
RMAN-00571: ===========================================================
RMAN-00569: =============== ERROR MESSAGE STACK FOLLOWS ===============
RMAN-00571: ===========================================================
RMAN-03002: failure of Duplicate Db command at 04/16/2021 13:12:33
RMAN-05501: aborting duplication of target database
RMAN-05001: auxiliary file name /mnt/export/oracle/oradata/perfstat01.dbf conflicts with a file used by the target database
```

**Solución:** 
El error debería resolverse usando la cláusula NOFILENAMECHECK en tu comando de duplicación.

```sql
DUPLICATE DATABASE TO 'TEST02' BACKUP LOCATION '/mnt/export/oracle/orabackup/full/' NOFILENAMECHECK;
```

> 💡 **Tip:** Ambas cláusulas pueden usarse en el mismo comando DUPLICATE en caso de que estemos duplicando una base de datos non-OMF desde un backup en frío de RMAN de la siguiente manera:
> 
> ```sql
> DUPLICATE DATABASE TO 'TEST02' BACKUP LOCATION '/mnt/export/oracle/orabackup/full/' NOREDO NOFILENAMECHECK;
> ```