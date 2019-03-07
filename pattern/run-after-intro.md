# Run after intro

If your mod is enabled when the game starts, the `OnEnabled()` method of your main class (that uses the `IUserMod` interface) will be invoked while the loading screen (usually Paradox logo) is visible. During this stage of the app loading, UIView features will not be fully initialised. This can be trublesome if you want to create some UI when your mod is enabled (such as a 'Mod Incompatibility' warning panel).

The code snippet below shows how to ovecome this issue:

```csharp
public class Mod : IUserMod
{
  // ...

  public void OnEnabled()
  {
    if (UIView.GetAView() != null) // mod was just enabled in content manager
    {
      DoStuff();
    }
    else // mod was already enabled when cities.exe was launched
    {
      LoadingManager.instance.m_introLoaded += DoStuff;
    }
  }
  
  public void OnDisabled()
  {
    // remove event listener (won't error if there isn't one)
    LoadingManager.instance.m_introLoaded -= DoStuff;
  }

  public void DoStuff() {
    // UIView is fully initialised at this point
  }
}
```
