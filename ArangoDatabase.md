##ArangoDatabase
APIs for working with ArangoDB are accessible through `ArangoDatabase` object, 
it performs all operations against ArangoDB by communicating through ArangoDB Http API.

```csharp
using (IArangoDatabase db = new ArangoDatabase("http://localhost:8529", "SampleDB"))
{
	var info = db.CurrentDatabaseInformation();
	
	// output: "C:\ArangoDB 2.3.4\var\lib\arangodb\databases\database-179610003747"     
	Console.WriteLine(info.Path);
}
```