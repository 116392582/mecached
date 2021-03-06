### Memcached 1.4.0 Release Notes

Date:  Thu July 9, 2009

### 1 Download

Download Link:

http://memcached.org/files/old/memcached-1.4.0.tar.gz

### New Features

#### Binary Protocol

A new feature that brings new features.  We now have goodness like
CAS-everywhere (e.g. delete), silent, but verifiable mutation
commands, and many other wonders.

Note that the original protocol is *not* deprecated.  It will be
supported indefinitely, although some new features may only be
available in the binary protocol.

==== Client Availability ====

Many clients for the binary protocol are available.

* C

  libmemcached supports just about anything you can do with a memcached
  protocol and is the foundation for many clients in many different
  languages (which you can find linked from the project page).

  Project page:  http://tangent.org/552/libmemcached.html

* Java

  spymemcached has very good text and binary protocol support over IPv4
  and IPv6 with a quite comprehensive test suite.

  Project page:  http://code.google.com/p/spymemcached/

* Protocol Spec

  NIH problem?  Go write your own client.  :)

  http://cloud.github.com/downloads/memcached/memcached/protocol-binary.txt


#### Performance

Lots of effort has gone into increasing performance.

There is no longer a build-time distinction between a single-threaded
and multi-threaded memcached.  If you want a single-threaded
memcached, ask for one thread (though there'll still be utility
threads and other such things in the background).  This change lets us
focus on a future where multiple cores can be saturated for servicing
requests.

Facebook-inspired contention reduction with per-thread stat collection
and the Facebook connection dispatch and thread starvation prevention
contributions helped our scalability.

Lock analysis also showed us that we had quite a bit of contention on
hash table expansion which has been moved into its own thread, greatly
improving the scalability on multicore hardware.

A variety of smaller things also shook out of performance testing and
analysis.

There's also a memory optimization for users who don't actually make
use of CAS.  Running memcached with -C disables the use of CAS
resulting in a savings of about eight bytes per item.  If you have big
caches, and don't use CAS, this can lead to a considerable savings.

#### Stats

There are several new stats and some new ways to look at older stats.

==== New Stats ====

* stats settings

  Show all current server settings (useful for troubleshooting as well
  as internal verification).

* Delete

  The global stats now contain statistics on deletion.

  delete_hits refers to the number of times a deletion command was
  issued which resulted in a modification of the cache while
  delete_misses refers to the number of times a deletion command was
  issued which had no effect due to a key mismatch.

* Incr/Decr

  Incr and decr each have a pair of stats showing when a
  successful/unsuccessful incr occurred.  incr_hits, incr_misses,
  decr_hits, and decr_misses show where such mutations worked and where
  they failed to find an existing object to mutate.

* CAS

  CAS stats are tracked in three different ways:

  * cas_hits

    Number of attempts to CAS in a new value that worked.

  * cas_misses

    Number of attempts to CAS in a value where the key was not found.

  * cas_badval

    Number of attempts to CAS in a value where the CAS failed due to the
    object changing between the gets and the update.

* slab class evicted time

  Per slab class, you can now see how recently accessed the most recent
  evicted data was.  This is a useful gauge to determine eviction
  velocity on a slab so you can know whether evictions are healthy or if
  you've got a problem.

* Connection yield counts

  The -R option allows you to limit how many requests are handled in a
  single trip through the network state machine.  The conn_yields stat
  reports how many times this has occurred.

  This is most useful when you have very high rates of operations from a
  single client which seems to be hogging server resources unfairly,
  such as extremely large multi-get operations.  We do not expect this
  to be a common situation, but this stat can help diagnose such issues
  if they should occur.


==== More Granular Stats ====

Where possible, stats are now tracked individually by slab class.  The
following stats are available on a per-slab-class basis (via "stats slabs"):

  * get_hits
  * cmd_set
  * delete_hits
  * incr_hits
  * decr_hits
  * cas_hits
  * cas_badval

(misses are obviously not available as they refer to a non-existent item)

==== Removed stats ====

"stats malloc" and "stats maps" have been removed.

If you depended on these commands for anything, please let us know so
we may suggest alternatives for you.

#### Misc

  * More tests
  * More/better documentation
  * Code cleanup

### Bug Fixes

  * Build fixes on ubuntu (gcc and icc) and FreeBSD
  * bad interaction with cas + incr (bug 15)
  * setuid failures are reported properly at daemonization time
  * decr overflow causing unnecessary truncation to 0 (bug 21)
  * failure to bind on Linux with no network (i.e. laptop dev)
  * some memcached-tool cleanup
  * Alignment bug in binary stats (bug26)
  * Occasional buffer overflow in stats (bug27)
  * Try to recycle memory more aggressively. (bug14)
  * incr validation (bug31)
  * 64-bit incr/decr delta fixes (bug21)
  * ascii UDP set (bug36)
  * stats slabs' used chunks (bug29)
  * stats reset should reset item stats, eviction counters, etc... (bug22)
  * Fix all stat buffer management
  * Fixes for -R handling (bug61)
  * GCC 4.4 fixes (bug60)

### Development Info

We've added a bunch of tests and new code coverage reports.

All included code in this release has been tested against the
following platforms (using the in-tree test suite):

  * ubuntu 8.10 (64-bit, both gcc and icc)
  * ubuntu 8.04 (32-bit)
  * ubuntu 9.10 (64-bit, gcc 4.4)
  * OS X 10.5 (ppc and intel)
  * OpenSolaris 5.11 x86 (with and without dtrace)
  * Solaris 10 sparc (with and without dtrace)
  * FreeBSD 7 x86

### Feedback

This version is considered a stable release and has been well-tested,
but bugs are always possible.  Report feedback to the list or file
bugs as you find them.

  * Mailing List:  http://groups.google.com/group/memcached
  * Issue Tracker:  https://github.com/memcached/memcached/issues/list
  * IRC:  #memcached on freenode

### Contributors

The following people contributed to this release since 1.2.8.

Note that this is based on who contributed changes, not how they were
done.  In many cases, a code snippet on the mailing list or a bug
report ended up as a commit with your name on it.

Note that this is just a summary of how many changes each person made
which doesn't necessarily reflect how significant each change was.
For details on what led up into a branch, either grab the git repo and
look at the output of `git log 1.2.8..1.4.0` or use a web view.

  * Repo list:  https://github.com/memcached/memcached/wiki/DevelopmentRepos
  * Web View: http://github.com/memcached/memcached/commits/1.4.0

```
  149  Dustin Sallings
   61  Trond Norbye
   33  Toru Maesaka
   30  dormando
   13  Steve Yen
    7  hachi
    6  Aaron Stone
    5  Brad Fitzpatrick
    4  Victor Kirkebo
    3  Eric Lambert
    2  Brian Aker
    1  Chris Goffinet
    1  Clinton Webb
    1  Matt Ingenthron
    1  Ricky Zhou
    1  Evan Klitzke
```