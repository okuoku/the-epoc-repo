The Oldest Repo
===============

This repository contains commits that around the UNIX epoc (1970-01-01 midnight).

How to reproduce
----------------

Prepare a tree and just set `GIT_AUTHOR_DATE` and `GIT_COMMITTER_DATE` then `git commit-tree`

```
git init
git add README.txt
git write-tree > tree.txt
GIT_AUTHOR_DATE="1970-01-01 00:00:00 +0000" GIT_COMMITTER_DATE="1970-01-01 00:00:00 +0000" git commit-tree -F msg1.txt `cat tree.txt`
```

Negative offset glitch
----------------------

Currently, GitHub denies negative 

We can generate a commit prior the UNIX epoc using timezone offset; say `1970-01-01 00:00:00 +0900` and will get

```
author okuoku <mjt@cltn.org> 18446744073709519216 +0900
```

These digits `18446744073709519216` mean `0xffffffffffff8170` in hex.

(Un)fortunately, GitHub denies such commit;

```
Counting objects: 4, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 979 bytes | 979.00 KiB/s, done.
Total 4 (delta 0), reused 0 (delta 0)
remote: error: object 32b0128451f66ca872431bc6c23fb2854b20e85d: badDateOverflow: invalid author/committer line - date causes integer overflow
remote: fatal: fsck error in packed object
error: remote unpack failed: index-pack abnormal exit
To github.com:okuoku/the-oldest-repo.git
 ! [remote rejected] master -> master (failed)
error: failed to push some refs to 'git@github.com:okuoku/the-oldest-repo.git'
```

References
----------

- https://github.com/dspinellis/unix-history-repo
  - Unix-history-repo is one of the seriously-oldest repo
- https://stackoverflow.com/questions/5093533/git-ignores-git-author-date-is-this-a-bug/5093714#5093714
  - SO answer that describes why can't we use `0 +0000` for date
- https://github.com/git/git/blob/6e0cc6776106079ed4efa0cc9abace4107657abf/date.c#L637
  - Actual code in `git` that denies the digit.
