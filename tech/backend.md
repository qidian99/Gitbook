# Backend

## MySQL

### Show all views

```sql
SHOW FULL TABLES 
WHERE table_type = 'VIEW';
```

### Show view definition \(somehow not working\)

```sql
SHOW CREATE VIEW 'viewName'
```

returns the SQL definition with `create view` statement while

```sql
SELECT  VIEW_DEFINITION 
    FROM    INFORMATION_SCHEMA.VIEWS
    WHERE   TABLE_SCHEMA    = 'DATABASENAME' 
    AND     TABLE_NAME      = 'VIEWNAME';
```

returns only definition

### Create the view using definition

```sql
CREATE VIEW test.v AS SELECT * FROM t;
```

### Resetting root user password

