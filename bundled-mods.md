# Bundled Mods

The game comes with several bundled mods - here's some useful snippets for working with them...

> Note: The `{Mod.name}` in `Debug` logs assumes you are using the [auto-incremented-version](auto-increment-version.md) pearl.

### PluginInfo of all bundled mods

```csharp
public List<PluginInfo> GetBundledMods()
{
    List<PluginInfo> bundledMods = new List<PluginInfo>();
    
    try
    {
        foreach (PluginInfo mod in Singleton<PluginManager>.instance.GetPluginsInfo())
        {
            if (mod.isBuiltin)
            {
                bundledMods.Add(mod);
            }
        }
    }
    catch (Exception e)
    {
        Debug.Log($"[{Mod.name}] GetBundledMods({name}) ERROR");
        Debug.LogException(e);
    }
    
    return bundledMods;
}
```

### PluginInfo for specific bundled mod

This returns the `PluginInfo` for a bundled mod of specified name...

> Requires [get-mod-name](get-mod-name.md)

```csharp
public PluginInfo GetBundledModInfo(string name)
{
    try
    {
        foreach (PluginInfo mod in Singleton<PluginManager>.instance.GetPluginsInfo())
        {
            if (mod.isBuiltin && GetModName(mod) == name)
            {
                return mod;
            }
        }
    }
    catch (Exception e)
    {
        Debug.Log($"[{Mod.name}] GetBundledModInfo({name}) ERROR");
        Debug.LogException(e);
    }
    
    return null; // make sure you handle this scenario!
}
```

### Enable or Disable a bundled mod

Quickly enable or disable a bundled mod...

```csharp
public void SetBundledModState(string name, bool enabled)
{
    try
    {
        GetBundledModInfo(name).isEnabled = enabled;
    }
    catch (Exception e)
    {
        // most likely mod not found
        Debug.Log($"[{Mod.name}] SetBundledModState({name},{enabled}) ERROR");
        Debug.LogException(e);
    }
}
```
