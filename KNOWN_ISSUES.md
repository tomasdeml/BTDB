# Known Issues
## Event Store

### General
- [Fixed in issue Bobris/BTDB#19] Do not create events containing dictionary properties with recursive value type (applies to transitive relationships as well), e.g.  
```csharp
class ObjectValidatedEvent 
{
  public IList<ErrorInfo> Errors { get; set }
}

class ErrorInfo
{
  public IDictionary<string, IList<ErrorInfo>> PropertyErrors { get; set; }
}
```
This will cause an exception during event deserialization. You can workaround it by using a common base type instead:
```csharp
class ErrorInfo : ErrorInfoBase
{
  public IDictionary<string, IList<ErrorInfoBase>> PropertyErrors { get; set; }
}
```

## Object DB
### General
- Properties with type `ISet<T>` / `HashSet<T>` are not supported

### Dictionaries
#### Keys
- Do not use `string`, `sbyte`, `byte[]` in compound keys
- Do not use `DateTime` instances with other than `DateTime.Utc` date time kind in compound keys - it is checked on all places except compound keys by BTDB automatically

### Transactions
- `IObjectDbTransaction.Singleton()` method will throw `NullReferenceException` if the ObjectDb transaction was started without an active KeyValueDb transaction. Full stack trace:
```
System.NullReferenceException : Object reference not set to an instance of an object.
   at BTDB.KVDBLayer.ExtensionMethods.SetKeyPrefix(IKeyValueDBTransaction transaction, Byte[] prefix)
   at BTDB.ODBLayer.ObjectDBTransaction.Singleton(Type type)
```

To enable debugging against BTDB sources, setup .pdb locations as described in http://www.symbolsource.org/Public/Home/VisualStudio. Please note that the PDBs may be unusable under newer version of Visual Studio / .net as the used format of the PDBs was deprecated.

*TODO Convert to Wiki page in the end?*

