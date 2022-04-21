# Restore

Imagine you have branches featuer and master:

```bash
git switch feature
echo newline_feature_file1_1 >> file1
git commit -am 'feature change file1'
echo newline_feature_file2_1 >> file2
git commit -am 'feature change file2'

# switch to master and make some changes
git switch master
echo newline_master_file1_1 >> file1
git commit -am 'master line change 1'
echo newline_master_file1_2 >> file1
```

At this point you decide that the **uncommitted** change to file1 is not needed, so you can `git restore` it to its previous, committed state.

```bash
git restore file1
```

At this point you decide that you want to bring the contents of file1 from the feature branch into the master branch. You can do this by specifying the `--source` flag to git restore:

```bash
git restore --source=feature file1
```

Then you want to again undo this **uncommitted** change and go to feature branch again:

```bash
git restore file1
```

But you don't remember what was your branch name! No matter, you can use a trick similar to cd - to return to the last branch you were on:

```bash
git switch -
```


