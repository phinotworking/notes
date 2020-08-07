### Find the space used by all tables in a database
Both options works on Azure SQL

Option 1 - List all tables
```sql
SELECT   
      o.name AS [table_name],
      sum(p.reserved_page_count) * 8.0 / 1024  AS [size_in_mb],
      p.row_count AS [records]
FROM  
      sys.dm_db_partition_stats AS p,
      sys.objects AS o
WHERE   
      p.object_id = o.object_id
      AND o.is_ms_shipped = 0 

GROUP BY o.name , p.row_count
ORDER BY o.name DESC
```

Option 2 - `sp_spaceused` stored procedure
```sql
Exec sp_spaceused [Users] -- Table Name
```

### References
http://dataap.org/blog/2016/02/29/sql-azure-database-table-size/

