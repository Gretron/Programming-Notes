# SQLite & Linq

## Querying Data With SQLite & Manipulating Data With Linq

### 1. SQLite NuGet Package

To get SQLite via NuGet, download the package called "**sqlite-net-pcl**":

![image-20220712154712223](..\.img\image-20220712154712223.png)

To use SQLite package in C# code:

```c#
using SQLite;
```



### 2. SQLite Connection

To create SQLiteConnection in order to execute database queries:

```c#
using (SQLiteConnection connection = new SQLiteConnection(connectionPath))
{
    // Database Queries
}
```



### 3. SQLite Attributes

While creating DataModel classes for database tables, use the following flags:

#### Primary Key

To define database primary key

```c#
[PrimaryKey]
public int Id { get; set; }
```

#### AutoIncrement

To automatically increment integer columns on insertion

```c#
[AutoIncrement]
public int Id { get; set; }
```

#### Table & Column

To override class and member names for database table

```c#
[Table("Person")] // Appears as 'Person' table in database, not 'Human' 
public class Human
{
    [Column("FirstName")] // Appears as 'FirstName' column in database, not 'Name'
    public string Name { get; set; }
}
```

#### Indexed

To define database foreign key

```c#
public class Publication
{
    public int Id { get; set; }
    
    [Indexed] // Requires exisiting 'UserId' from other table
    public int UserId { get; set; }
    
    public string Content { get; set; }
}
```

#### MaxLength

To define the maximum length of a value

```c#
public class Person
{
    public int Id { get; set; }
    
    [MaxLength(50)]
    public string Name { get; set; }
}
```

#### Ignore

To prevent member from being included in database

```c#
public class Person
{
    public int Id { get; set; }
    
    public string FirstName { get; set; }
    
    public string LastName { get; set; }
    
    [Ignore] // Will not appear in database table 'Person'
    public string FullName => $"{FirstName} {LastName}";
}
```



### 4. SQLite Queries

To execute a variety of SQL queries, use the following examples:

#### Create Table

```c#
using (SQLiteConnection connection = new SQLiteConnection(connectionPath))
{
    connection.CreateTable<DataModel>(); // Where 'DataModel' is a user-defined type made for database tables
}
```

#### Insert Row

```c#
using (SQLiteConnection connection = new SQLiteConnection(connectionPath))
{
    connection.Insert(dataItem); // Where 'dataItem' is an instance of a user-defined type made for an existing database table
}
```

#### Select

```c#
using (SQLiteConnection connection = new SQLiteConnection(connectionPath))
{
    connection.Table<DataModel>().ToList(); // Returns all rows of 'DataModel' table as 'DataModel' objects in a list
}
```

#### Update

```c#
using (SQLiteConnection connection = new SQLiteConnection(connectionPath))
{
    connection.CreateTable<DataModel>(); // To avoid throwing exception related to missing table
    connection.Update(dataItem); // Updates row(s) in table of type 'DataModel' with new 'dataItem' using primary key
}
```

#### Delete

```c#
using (SQLiteConnection connection = new SQLiteConnection(connectionPath))
{
    connection.CreateTable<DataModel>(); // To avoid throwing exception related to missing table
    connection.Delete(dataItem); // Deletes row(s) in table of type 'DataModel' using 'dataItem'  primary key
}
```



### 5. Linq

To filter list using Linq:

```c#
var filteredList = from s in strings
    where s.Contains(str)
    select s;
```

To order list using Linq:

```c#
var orderedList = from s in strings
    orderby s.Member
    select s; // Returns IOrderedEnumerable

var orderedList = (from s in strings
    orderby s.Member
    select s).ToList(); // Returns IEnumerable
```



