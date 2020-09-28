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

I locate the mysqladmin under the mysql location, close the mysql service, sudo the mysqladmin and follow the help text to change the password for root user.

### Copy/Move a table from one database to another

{% embed url="https://stackoverflow.com/questions/12242772/easiest-way-to-copy-a-table-from-one-database-to-another" %}

### Grant user privileges

```sql
GRANT ALL ON example_database.* TO 'example_user'@'%';
```

## Sharding

{% embed url="https://blog.csdn.net/bluishglc/article/details/7696085" %}







