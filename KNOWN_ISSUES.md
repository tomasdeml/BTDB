# Known Issues
## Object DB

### General
- Do not create dictionary properties with recursive value types, e.g.  
```csharp
class Man 
{
  public IDictionary<string, Man> Brothers { get; set; }
}
```
This will cause an exception during object deserialization. You can workaround it by using a common base type instead:
```csharp
class Man : Person
{
  public IDictionary<string, Person> Brothers { get; set; }
}
```

### Dictionaries
#### Keys
- Do not use `string`, `sbyte`, `byte[]` in compound keys
- Do not use `DateTime` instances with other than `DateTime.Utc` date time kind in compound keys - it is checked on all places except compound keys by BTDB automatically

### Transactions
- IObjectDbTransaction.Singleton() method will throw `NullReferenceException` if the ObjectDb transaction was started without anactive KeyValueDb transaction. Full stack trace:
```
System.NullReferenceException : Object reference not set to an instance of an object.
   at BTDB.KVDBLayer.ExtensionMethods.SetKeyPrefix(IKeyValueDBTransaction transaction, Byte[] prefix)
   at BTDB.ODBLayer.ObjectDBTransaction.Singleton(Type type)
```

*To enable debugging against BTDB sources, setup .pdb locations as described in http://www.symbolsource.org/Public/Home/VisualStudio*

*TODO Convert to Wiki page in the end?*

