# Unity + Git Merge Experiment

## 1. Configure [Unity Smart Merge](https://docs.unity3d.com/Manual/SmartMerge.html)

Locate your Unity installation, hence denoted as `UNITY_TOOLS_FOLDER`.
  - For example, on Windows it is `C:/Program Files/Unity/Hub/Editor/2021.3.8f1/Editor/Data/Tools/`
 - This folder has two files of interest:
   - `UnityYAMLMerge.exe`: The program that performs smart merge
   - `mergespecfile.txt`: Configuration of "fallback" merge tool for remaining conflicts

Open your local git config using: `git config --local --edit` and paste the following:
```conf
[merge]
    tool = unityyamlmerge
[mergetool "unityyamlmerge"]
    keepBackup = false
    trustExitCode = false
    cmd = \"<UNITY_TOOLS_FOLDER>/UnityYAMLMerge.exe\" merge -p "$BASE" "$REMOTE" "$LOCAL" "$MERGED"
```

Replace the contents of `mergespecfile.txt`, to setup your "fallback" merge tool. The below shows an example for VS Code:

```txt
#
# UnityYAMLMerge fallback file
#
# %l is replaced with the path of you local version
# %r is replaced with the path of the incoming remote version
# %b is replaced with the common base version
# %d is replaced with a path where the result should be written to
# On Windows %programs% is replaced with "C:\Program Files" and "C:\Program Files (x86)" there by resulting in two entries to try out
# On OSX %programs% is replaced with "/Applications" and "$HOME/Applications" thereby resulting in two entries to try out

* use "%programs%/Microsoft VS Code/bin/code" --wait --merge %r %l %b %d
```

**If you are using SourceTree, you may want to check this [YouTube tutorial](https://youtu.be/P_vLYDq2YkE).**

## 2. Doing the merge

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
