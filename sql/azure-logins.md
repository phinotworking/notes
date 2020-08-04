### Create New Azure SQL Login
Currently, the SQL Azure portal does not allow you to administrate additional users and logins, in order to do this you need to use Transact-SQL.
The easiest way to execute Transact -SQL against SQL Azure is to use the SQL Server Management Studio 2008 R2.
SQL Server Management Studio 2008 R2 will list the users and logins associated with the databases; however, at this time it does not provide a graphical user interface for creating the users and logins.

```sql
Use master
go
CREATE LOGIN [svc_reports] WITH password='myPassword3498!';

use MY_DATABASE
Go
CREATE USER svc_reports FROM LOGIN [svc_reports];
go
EXEC sp_addrolemember 'db_datareader', 'svc_reports';
```

### References
https://azure.microsoft.com/en-us/blog/adding-users-to-your-sql-azure-database/
