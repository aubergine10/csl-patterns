# Get mod name

Ddespite appearances, you can't get the mod name from a `PluginInfo` instance - that merely returns the filename without extension.

If you want the `.Name` from the `IUserMod` interfaced class within the mod, you'll need to do a bit more work...

> Note: If you only have a workshop id, you'll need to use [get-plugin-info](get-plugininfo).

```csharp
public string GetModName(PluginInfo pluginInfo)
{
    // if the mod doesn't specify '.Name', fallback to the filename
    string name = pluginInfo.name;
    
    IUserMod[] instances = pluginInfo.GetInstances<IUserMod>();
    if ((int)instances.Length > 0)
    {
        name = instances[0].Name;
    }
    
    return name;
}
```
