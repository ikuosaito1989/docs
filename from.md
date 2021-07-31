# From

## From a Table or View
The `Query` constructor takes an optional parameter to set the `from` clause

```cs
//:playground
new Query("Posts");
```

Or you can use the `From` method to set it

```cs
//:playground
new Query().From("Posts");
```

```sql
SELECT * FROM [Posts]
```

### Alias
To alias the table you should use the `as` syntax

```cs
//:playground
new Query("Posts as p")
```

```sql
SELECT * FROM [Posts] AS [p]
```

## From a Sub Query

You can select from a sub query by passing a `Query` instance to the `From` method or you can use the `Lambda` function overload.

```cs
//:playground
var fewMonthsAgo = DateTime.UtcNow.AddMonths(-6);
var oldPostsQuery = new Query("Posts").Where("Date", "<", fewMonthsAgo).As("old");

var query = new Query().From(oldPostsQuery).OrderByDesc("Date");
```


```sql
SELECT * FROM (SELECT * FROM [Posts] WHERE [Date] < '2017-06-01 6:31:26') AS [old] ORDER BY [Date] DESC
```

## From a Raw expression

The `FromRaw` method let you write raw expressions.

for example in SqlServer you can use the `TABLESAMPLE` to get a 10% sample of the total rows in the comments table.

```cs
    var query = new Query().FromRaw("Comments TABLESAMPLE SYSTEM (10 PERCENT)")
```

```sql
SELECT * FROM Comments TABLESAMPLE SYSTEM (10 PERCENT)
```

> **Note:** Remember you can use the `[]` to wrap identifiers word in your string