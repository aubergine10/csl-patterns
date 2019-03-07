# Get `PluginInfo`

If you have the workshop id, you can get the associated `PluginInfo` instance if that mod is subscribed...

```csharp
public static PluginInfo GetPluginInfo(ulong workshopId)
{
    try
    {
        foreach (PluginInfo mod in Singleton<PluginManager>.instance.GetPluginsInfo())
        {
            if (mod.publishedFileID.AsUInt64 == workshopId)
            {
                return mod;
            }
        }
    }
    catch (Exception e)
    {
        Debug.Log($"[{Mod.name}] GetPluginInfo({workshopId}) ERROR");
        Debug.LogException(e);
    }
    
    return null; // make sure you handle this scenario!
}
```
