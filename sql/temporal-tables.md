### Disable temporal tables

```sql
ALTER TABLE dbo.[Table1] SET (SYSTEM_VERSIONING = OFF);   
drop table __Table1History
```

### Re-enable temporal tables

```sql
ALTER TABLE dbo.[Table1]
	Add PERIOD FOR SYSTEM_TIME (SysStartTime,SysEndTime);

ALTER TABLE dbo.[Table1]
  Set  (SYSTEM_VERSIONING = ON  (HISTORY_TABLE = dbo.[Table1History]));
GO
```

### Important remarks
* A system-versioned temporal table must have a primary key defined and have exactly one PERIOD FOR SYSTEM_TIME defined with two datetime2 columns, declared as GENERATED ALWAYS AS ROW START / END
* The PERIOD columns are always assumed to be non-nullable, even if nullability is not specified. If thePERIOD columns are explicitly defined as nullable, the CREATE TABLE statement will fail.
* The history table must always be schema-aligned with the current or temporal table, in terms of number of columns, column names, ordering and data types.
* An anonymous history table is automatically created in the same schema as current or temporal table.
* The anonymous history table name has the following format: MSSQL_TemporalHistoryFor_<current_temporal_table_object_id>_[suffix]. Suffix is optional and it will be added only if the first part of the table name is not unique.
* The history table is created as a rowstore table. PAGE compression is applied if possible, otherwise the history table will be uncompressed. For example, some table configurations, such as SPARSE columns, do not allow compression.
* A default clustered index is created for the history table with an auto-generated name in format IX_<history_table_name>. The clustered index contains the PERIOD columns (end, start).
* To create the current table as a memory-optimized table, see [System-Versioned Temporal Tables with Memory-Optimized Tables](https://docs.microsoft.com/en-us/sql/relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables?view=sql-server-ver15).

References

https://docs.microsoft.com/en-us/sql/relational-databases/tables/creating-a-system-versioned-temporal-table?view=sql-server-ver15
https://dba.stackexchange.com/questions/224898/poor-temporal-table-performance-on-older-values
