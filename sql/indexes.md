### Create a New Index
```sql
CREATE NONCLUSTERED INDEX [IX_Class_IsDeleted] ON [dbo].[Class] ([IsDeleted])
GO
```

### Rebuild indexes in a table using Transact-SQL
So you have an Index created, but you need to re-build it?

```sql
USE MyDatabase;
GO

ALTER INDEX ALL ON MyTable REBUILD;
GO
```

References

https://solutioncenter.apexsql.com/why-when-and-how-to-rebuild-and-reorganize-sql-server-indexes/
