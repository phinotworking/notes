### Don't use nvarchar(max) without performance considerations
https://sqlperformance.com/2017/06/sql-plan/performance-myths-oversizing-strings


So when trying to alter your columns to fit, you can do this to get around the 'String or binary data would be truncated" error
SET ANSI_WARNINGS OFF

```sql
-- So if you already have a column on nvarchar(max), you can truncate it here
alter table MyTable1 alter column Description nvarchar(2000) not null
alter table MyTable1 alter column Tooltip nvarchar(2000) not null
```
