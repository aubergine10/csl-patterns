# Auto-increment mod version

Keep forgetting to bump the version of your mod? Get the compiler to do it for you...

> Note: If you do this, and use the debug trace style menioned in the 'Bonus' section at the end of this article, the log analyser will be able to link together much more information about your mod.

### Step 1: Disable deterministic setting

While VS2017 is closed, edit the `.csproj` file of your mod and look for the following:

```xml
<Deterministic>true</Deterministic>
```

Change `true` to `false` and save the file.

### Step 2: Add a version wildcard

Open the `AssemblyInfo.cs` file in your project and find the following line (usually near the end of the file):

```csharp
[assembly: AssemblyVersion("1.0.0.0")]
```

Add a wildcard to denote where the auto-incrementing part of the version number should start (recommend using the minor or build position):

```csharp
// this will auto-increment the minor, build and revision parts of the version
[assembly: AssemblyVersion("1.*")]
```

### Step 3: Access the generated value in your mod

Here we set a static `ver` field that pulls the version number from the assembly. We're also pulling the mod `name` from the assembly too, and concatenating it with the `ver` field.

```csharp
public class Mod : IUserMod
{

  // Name and version from assembly
  public static readonly string ver = $"{Assembly.GetExecutingAssembly().GetName().Version.Major}."
                                    + $"{Assembly.GetExecutingAssembly().GetName().Version.Minor}";
  public static readonly string name = $"{Assembly.GetExecutingAssembly().GetName().Name} {ver}";

  // Required by C:SL
  public string Name => name;
  public string Description => "bleh..."

  // ...
}
```

### Bonus: Prefixing your log messages

You can now access the static `ver` or `name` fields from anywhere in your mod, for example in a `foo.cs` you could do something like this:

```csharp
Debug.log($"[{Mod.name}] hello!");
```

> The CLS Log Analyser will automatically detect trace in that format and link it to your mod, making trawling log files much less painful.

If you want the full veersion string:

```csharp
Debug.log($"[{Mod.name}] Build: {Assembly.GetExecutingAssembly().GetName().Version}");
```

I normally output that when the mod is enabled:

```csharp
public class Mod : IUserMod
{
  // ...
   
  public void OnEnabled()
  {
    Debug.Log($"[{name}] Build {Assembly.GetExecutingAssembly().GetName().Version} Enabled.");

    // ...
  }
  
  public void OnDisabled()
  {
    Debug.Log($"[{name}] Disabled");
  }
}
```
