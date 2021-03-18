In CTP 3.2, Microsoft added a new option to allow you to suppress the array wrappers around JSON output. For example, if you run this query:

```sql
SELECT TOP (2) name 
  FROM sys.all_objects 
  ORDER BY name
  FOR JSON PATH;
```
You get this result, which includes `[square brackets]` around the entire document:

```
[{"name":"all_columns"},{"name":"all_objects"}]
```

But now with the new option, `WITHOUT_ARRAY_WRAPPER`, you can run this query:

```sql
SELECT TOP (2) name 
FROM sys.all_objects 
ORDER BY name
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER;
```

And you get results that are slightly different (namely, they are missing the outer square brackets):

```
{"name":"all_columns"},{"name":"all_objects"}
```
This can be important if, for example, you are extracting bits of data to combine into larger JSON strings.

### JSON_QUERY() SQL Server Function
This new function allows you to retrieve an object or an array from within a larger JSON string. Let's say we have this simple table:

```sql
CREATE TABLE dbo.Cars
(
  CarID INT NOT NULL,
  Attributes NVARCHAR(4000),
  CONSTRAINT PK_Car PRIMARY KEY(CarID),
  CONSTRAINT IsValidJSON CHECK (ISJSON(Attributes) = 1)
);
```
And this rather simplistic sample data, having several attributes for a couple of cars:

```sql
INSERT dbo.Cars(CarID, Attributes)
  VALUES(1, '{"year":1973,"make":"Datsun","model":"B210",
    "features": [{"power windows":1}]}'),
        (2, '{"year":1974,"make":"Datsun","model":"B210",
    "features": [{"power windows":1},{"block heater":1}]}');
```
We can use `JSON_QUERY()` to just pull out the set of features for each car:

```sql
SELECT CarID, Features = JSON_QUERY(Attributes, '$.features')
  FROM dbo.Cars;
```
These results show the set of features for each car:

|CarID|Features|
|:-----|:----------------------------------------|
|1|[{"power windows":1}]|
|2|[{"power windows":1},{"block heater":1}]|

We can break it down further to a feature per row, by using CROSS APPLY():

```sql
SELECT c.CarID, x.[value]
  FROM dbo.Cars AS c
  CROSS APPLY OPENJSON(JSON_QUERY(Attributes, '$.features')) AS x;
```

The results, though, still contain some of the JSON scaffolding:

|CarID|value|
|:---|:----|
|1|{"power windows":1}|
|2|{"power windows":1}|
|2|{"block heater":1}|

You could really turn this into a set-based result by using OPENJSON() again:

```sql
SELECT c.CarID, y.[key], y.[value]
  FROM dbo.Cars AS c
  CROSS APPLY OPENJSON(JSON_QUERY(Attributes, '$.features')) AS x
  CROSS APPLY OPENJSON(x.[value], '$') AS y;
```
These results show the output as proper key-value pairs:


|CarID|key|value|
|:---|:----|:----|
|1|power|windows|1
|2|power|windows|1
|2|block|heater|1


### References
https://www.mssqltips.com/sqlservertip/4138/advanced-json-techniques-in-sql-server-part-3/
