# Unity + Git Merge Experiment

## Pre-requisites

Please setup this repository following this [README from the game project](https://github.com/vinayg-usc/alter-ego-game/tree/env-setup#step-2-configure-unity-smart-merge).

## Steps

There are two branches:

```
Create a prefab ----> Add BoxCollider2D component (*main)
                 |
                 |----> Add a C# script component (*feature)
```

We will try to merge the `feature` branch into `main`. We desire to have both components after merging, however the default merge will fail:

```
git status
# Verify that it works and shows the current branch as main

git merge --no-commit origin/feature
# This should merge the C# files properly, but show a conflict for the prefab
```

### Using Smart Merge

Let's now resolve this conflict using smart merge:

```
git mergetool
# This detects the file having conflict and uses Unity's program to merge them

git status
# Ensure that there are exactly 3 files staged by the merge (if all went well)
```

If this worked correctly, you should see both components when you launch Unity. At any point if things go wrong, you can run `git merge --abort` to cancel the merge.
