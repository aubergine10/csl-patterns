# Run after intro

If enabled, the `OnEnabled()` method of your main class (that uses the `IUserMod` interface) will be invoked while the loading screen (usually Paradox logo) is visible meaning that the UIView features will not be fully initialised. This can cause a problem if you want to create some sort of UI element at that point (such as a 'Mod Incompatibility' warning).

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
    // ...
  }
}
```
