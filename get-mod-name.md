# Get mod name

Ddespite appearances, you can't get the mod name from a `PluginInfo` instance - that merely returns the filename without extension.

If you want the `.Name` from the `IUserMod`-interfaced class within the mod, you'll need to do a bit more work...

> Note: If you only have a workshop id, use [get-plugin-info](get-plugininfo.md) to retrieve the `PluginInfo` instance.

```csharp
public string GetModName(PluginInfo pluginInfo)
{
    // if the mod doesn't specify '.Name', fallback to the filename (with extension removed)
    string name = pluginInfo.name;
    
    try
    {
        IUserMod[] instances = pluginInfo.GetInstances<IUserMod>();
        if ((int)instances.Length > 0)
        {
            name = instances[0].Name;
        }
    }
    catch (Exception e)
    {
        Debug.Log($"[{Mod.name}] GetModName() ERROR");
        Debug.LogException(e);
    }
    
    return name;
}
```
