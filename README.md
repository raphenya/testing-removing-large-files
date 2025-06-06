# testing-removing-large-files

This repository contains commands used to remove a large files from a repository history.

### NOTICE

BEFORE FOLLOWING THESE INSTRUCTIONS BACKUP YOUR REPOSITORY

### Resources

1. https://www.geeksforgeeks.org/remove-large-files-from-commit-history-in-git/

2. https://www.youtube.com/watch?v=_XUxVMQn5xY

### Step 1: Make sure the repo is not protected

Check if the repository is protected and toggle to un-protect.

### Step 2: Remove file (large-file.txt) from full repo at '/Users/amos/Documents/repos/testing-removing-large-files'

```
cd /Users/amos/Documents/repos/testing-removing-large-files
git rm large-file.txt
git commit -m "removed file large-file.txt"
git push
```

###  Step 3: Mirror repo

```
cd ~/Desktop/cleaning
git clone --mirror git@github.com:raphenya/testing-removing-large-files.git
```

Please note: I have bare repo at `~/Desktop/cleaning` and full repo at `/Users/amos/Documents/repos/testing-removing-large-files`.

All the cleaning is done on the bare repo (`~/Desktop/cleaning`).

### Step 4: Download and install BFG

More downloads at https://rtyley.github.io/bfg-repo-cleaner/ and jars for other versions at https://repo1.maven.org/maven2/com/madgag/bfg/

I used the following for my Mac OS.

```
cd ~/Desktop/cleaning
wget https://repo1.maven.org/maven2/com/madgag/bfg/1.14.0/bfg-1.14.0.jar
```

### Step 5: Remove the File from history

```
cd ~/Desktop/cleaning
java -jar bfg-1.14.0.jar --delete-files large-file.txt testing-removing-large-files.git/
```

```
amos@Amogelangs-MacBook-Pro cleaning % java -jar bfg-1.14.0.jar --delete-files large-file.txt testing-removing-large-files.git/

Using repo : /Users/amos/Desktop/cleaning/testing-removing-large-files.git

Found 3 objects to protect
Found 2 commit-pointing refs : HEAD, refs/heads/main

Protected commits
-----------------

These are your protected commits, and so their contents will NOT be altered:

 * commit 91ece9e9 (protected by 'HEAD')

Cleaning
--------

Found 4 commits
Cleaning commits:       100% (4/4)
Cleaning commits completed in 48 ms.

Updating 1 Ref
--------------

	Ref               Before     After   
	-------------------------------------
	refs/heads/main | 91ece9e9 | 8383d09a

Updating references:    100% (1/1)
...Ref update completed in 13 ms.

Commit Tree-Dirt History
------------------------

	Earliest      Latest
	|                  |
	  .    D    D    m  

	D = dirty commits (file tree fixed)
	m = modified commits (commit message or parents changed)
	. = clean commits (no changes to file tree)

	                        Before     After   
	-------------------------------------------
	First modified commit | af8a4ff6 | b119aaa5
	Last dirty commit     | 382c87f8 | f210be0c

Deleted files
-------------

	Filename         Git id         
	--------------------------------
	large-file.txt | e69de29b (0 B )


In total, 5 object ids were changed. Full details are logged here:

	/Users/amos/Desktop/cleaning/testing-removing-large-files.git.bfg-report/2025-06-06/09-30-50

BFG run is complete! When ready, run: git reflog expire --expire=now --all && git gc --prune=now --aggressive
```


### Step 6: Clean Up

```
cd testing-removing-large-files.git/
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```

```
amos@Amogelangs-MacBook-Pro testing-removing-large-files.git % git reflog expire --expire=now --all && git gc --prune=now --aggressive
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 16 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (8/8), done.
Building bitmaps: 100% (4/4), done.
Total 8 (delta 2), reused 3 (delta 0), pack-reused 0
```


### Step 7: Push or (force push) the Changes

```
git push
```

```
amos@Amogelangs-MacBook-Pro testing-removing-large-files.git % git push
Enumerating objects: 8, done.
Writing objects: 100% (8/8), 689 bytes | 689.00 KiB/s, done.
Total 8 (delta 0), reused 0 (delta 0), pack-reused 8
remote: Resolving deltas: 100% (2/2), done.
To github.com:raphenya/testing-removing-large-files.git
 + 91ece9e...8383d09 main -> main (forced update)
```

### Step 8: Check file in history

Checking on a commit that added 'large-file.txt' at https://github.com/raphenya/testing-removing-large-files/commit/b119aaa5f8de096a8ce3cde98498cc4b185eb187 shows that the file was removed. Yey!

### Step 9: Decision to keep copies

Okay, now the repository at the remote location has the large file removed, and your local repository has the large file in its history.

So, archive your local repository (or delete it).

Then re-clone from remote with a large file removed from history.

### Step 10: Restore repository protection

If there was repository protection, please restore it.