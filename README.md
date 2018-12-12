bitscrub
===

bitscrub walks a directory tree and writes/updates/verifies the CRC32C of each file to a xattr.  This is useful for detecting bitrot on filesystems with no data checksum functionality.

`(time_marked, mtime, checksum)` data is stored in the `user._C` xattr.

Basic usage
---
`-v/--verify`, `-w/--write`, and `-i/--inspect` can be combined.  If none are specified, files will be checked only for lack of checksum data or updated mtime.

Write new checksums and update checksums where file has mtime > checksum time:

    bitscrub -w ~/

Verify checksums only:

    bitscrub -v ~/

Write and verify checksums:

    bitscrub -vw ~/

Inspect checksum xattr:

    bitscrub -i FILENAME

--help
---

```
# bitscrub --help
usage: scan.py [-h] [-v] [-w] [-i] [-q] [-l LISTING] [-n] PATH [PATH ...]

Walks a directory tree and reads and/or writes the CRC32C of each file to a
xattr "user._C". Useful for detecting bitrot. --verify, --write, and --inspect
can be combined. If none of these are specified, files will be checked only
for lack of checksum data or updated mtime.

positional arguments:
  PATH                  a file or directory

optional arguments:
  -h, --help            show this help message and exit
  -v, --verify          verify already-stored checksums to detect file
                        corruption
  -w, --write           calculate and write checksums for files that have no
                        checksum, or have an updated mtime
  -i, --inspect         print information about existing checksum data
  -q, --quiet           don't print both important and unimportant messages to
                        stderr; still print important messages to stdout
  -l LISTING, --listing LISTING
                        generate a file listing into this file (columns:
                        dentry type, CRC32C, mtime, size, filename)
  -n, --normalize-listing
                        print relative path
```
