# Static members

> See [krzychu124#211](https://github.com/krzychu124/Cities-Skylines-Traffic-Manager-President-Edition/issues/211) for more information and links to related issues.

### Overview

While static members (classes, methods, properties, etc.) are often useful and time-saving, they can cause problems in the following situations:

* Local and workshop verions of the same mod
* Multiple versions of the same mod

We discovered this with Traffic Manager: President Edition:

* When both a local (dev build) and workshop (subscription) were present, things like traffic light configs would stop working
* When Labs and Stable versions were present (not that users should ever do this, but it happens), load order determined which static members were being accessed
* When mods by pcfantasy, which duplicate chunks of TM:PE, were present alongside TM:PE, access to static members was not deterministic

In addition, static members can prevent hotswapping mods while C:SL is running.

As such, it is recommended to avoid using static members where possible.

### User story

> _As a developer I want my application to be hot-swappable to allow for a faster development time._
>  
> **Assumptions/Preconditions**
> - Static variables and methods prevent the mod from being modified and updated while the game is running: C:S cannot fully unload old instances.
> - Other features relevant for hot-swappability, like loading/saving custom data or deployment/removal of method detours are triggered at specific OnLoad/OnUnload events.
>  
> **Tasks**
> - Remove as many static fields/methods as possible. Convert them into instance-level members.
> - If not everything can be converted, collect remaining members and put them at a central place.
> - Extend OnLoad/OnUnload event handlers such they take care that any remaining static members are unset/set.
> - Check if the mod can be hot-swapped. If any further problems arise that were not foreseen create an issue on GitHub.
