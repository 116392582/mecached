### Memcached 1.4.13 Release Notes

Date: 2012-2-2

### Download

Download Link:

http://memcached.org/files/old/memcached-1.4.13.tar.gz


### Overview

Really tiny release with some important build fixes which were accidentally
omitted from 1.4.12.

For the interesting meat see [ReleaseNotes1412] and [ReleaseNotes1411]
especially, for slab memory reassignment!

### Fixes

  * - Fix inline issue with older compilers (gcc 4.2.2)
  * Better detection of sasl_callback_ft

### New Features

Sigh.

### Contributors

The following people contributed to this release since 1.4.12.

Note that this is based on who contributed changes, not how they were
done.  In many cases, a code snippet on the mailing list or a bug
report ended up as a commit with your name on it.

Note that this is just a summary of how many changes each person made
which doesn't necessarily reflect how significant each change was.
For details on what led up into a branch, either grab the git repo and
look at the output of `git log 1.4.12..1.4.13` or use a web view.

  * Repo list:  https://github.com/memcached/memcached/wiki/DevelopmentRepos
  * Web View: http://github.com/memcached/memcached/commits/1.4.13

```
     1	Dustin Sallings
     1	Steve Wills
```

