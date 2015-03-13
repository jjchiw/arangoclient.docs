##ArangoDatabase

####What is ArangoDatabase?
Creating a document, executing a query and all other operation against ArangoDB are accessible through `ArangoDatabase` object. it performs all operations by communicating through ArangoDB Http API.


```csharp
using (IArangoDatabase db = new ArangoDatabase("http://localhost:8529", "SampleDB"))
{
	var info = db.CurrentDatabaseInformation();
	
	// output: "C:\ArangoDB 2.3.4\var\lib\arangodb\databases\database-179610003747"     
	Console.WriteLine(info.Path);
}
```

####Creating a new ArangoDatabase object
Creating `ArangoDatabase` object is a cheap operation and it does not perform any expensive action against database, so it can be create anytime database operation is needed.

As an example above `ArangoDatabase` could be create with `database` and `url` but if you have database operations in many places, soon it will became a boilerplate code. `ArangoDatabase` could also be create with a `DatabaseSharedSetting`.


    
```csharp
var sharedSetting = new DatabaseSharedSetting
{
	Database = "SampleDB",
	Url = "http://localhost:8529",
	WaitForSync = true,
	CreateCollectionOnTheFly = false
};

using (IArangoDatabase db = new ArangoDatabase(sharedSetting))
{
}
```

As you can see there are properties other than `database` and `url` that could be used to change client default behavior 
([database setting in detail](/DatabaseSetting.html)).
So instead of pass the `database` and `url` property each time you create `ArangoDatabase`,  you can use a `DatabaseSharedSetting` and pass it to all places `ArangoDatabase` is used, you can even store it for the application life time.

To store `DatabaseSharedSetting` for the application life time, there are static methods
that can be used to create new `ArangoDatabase`:

```csharp
// this will create a new SharedSetting
ArangoDatabase.ChangeSetting(s =>
{
	s.Database = "SampleDB";
	s.Url = "http://localhost:8529";
	s.WaitForSync = true;
	s.CreateCollectionOnTheFly = false;
});

// above setting can be used to create new ArangoDatabase
using(IArangoDatabase db = ArangoDatabase.CreateWithSetting())
{
}
```

It is also possible to create as many settings as needed

```csharp
ArangoDatabase.ChangeSetting("SampleDBSetting", s =>
{
	s.Database = "SampleDB";
	s.Url = "http://localhost:8529";
	s.WaitForSync = true;
	s.CreateCollectionOnTheFly = false;
});

using(IArangoDatabase db = ArangoDatabase.CreateWithSetting("SampleDBSetting"))
{
}
```

<div class="document-caution">
Note that `ArangoDatabase` is also responsible for `Change Tracking` and 
tracks documents to perform easy update of documents. so do not keep
`ArangoDatabase` object for a long time. 
</div>







